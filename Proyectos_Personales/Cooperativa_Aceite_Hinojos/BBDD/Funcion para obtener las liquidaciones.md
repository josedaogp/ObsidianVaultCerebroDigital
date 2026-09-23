Falta por corregir un par de errores:

```
CREATE OR REPLACE FUNCTION generar_liquidacion(p_id_temporada INT, p_id_socio INT)
RETURNS TABLE (
    socio_nombre TEXT,
    socio_direccion TEXT,
    socio_dni TEXT,
    cuenta_bancaria TEXT,
    balance_tipo_aceituna TEXT,
    balance_calibre TEXT,
    balance_kilos NUMERIC,
    balance_subtotal NUMERIC,
    balance_gastos_calculado NUMERIC,
    balance_comision_calculado NUMERIC,
    totales_base_imponible NUMERIC,
    totales_iva_calculado NUMERIC,
    totales_subtotal_con_iva NUMERIC,
    totales_retencion_irpf NUMERIC,
    totales_saldo_a_su_favor NUMERIC,
    totales_total_anticipado NUMERIC,
    totales_total_neto NUMERIC
) AS $$
BEGIN
    RETURN QUERY
    WITH albaranes_agrupados AS (
        SELECT
            a.id_socio AS albaran_id_socio,
            ta.tipo_aceituna AS albaran_tipo_aceituna,
            ta.calibre AS albaran_calibre,
            SUM(a.total_albaran) AS albaran_subtotal,
            SUM(a.bruto - a.tara) AS albaran_kilos,
            MAX(ta.precio_por_kg) AS albaran_precio_por_kg,
            MAX(ta.gastos) AS albaran_gastos_por_kg,
            MAX(ta.comision) AS albaran_comision_porcentaje
        FROM
            albaranes a
            JOIN tipos_aceituna ta ON a.id_aceituna = ta.id_tipo_aceituna
        WHERE
            a.id_temporada = p_id_temporada  -- Parámetro de función
            AND a.id_socio = p_id_socio      -- Parámetro de función
        GROUP BY
            a.id_socio, ta.tipo_aceituna, ta.calibre
    ),
    gastos_comisiones AS (
        SELECT
            albaran_tipo_aceituna,
            albaran_calibre,
            albaran_subtotal,
            albaran_kilos,
            (albaran_kilos * albaran_gastos_por_kg) AS balance_gastos_calculado,
            (albaran_subtotal * albaran_comision_porcentaje / 100) AS balance_comision_calculado
        FROM
            albaranes_agrupados
    ),
    anticipos_total AS (
        SELECT
            COALESCE(SUM(importe), 0) AS totales_total_anticipado
        FROM
            anticipos
        WHERE
            id_temporada = p_id_temporada  -- Parámetro de función
            AND id_socio = p_id_socio      -- Parámetro de función
    ),
    totales AS (
        SELECT
            SUM(balance_gastos_calculado) AS totales_total_gastos,
            SUM(balance_comision_calculado) AS totales_total_comision,
            SUM(albaran_subtotal) AS totales_total_haber
        FROM
            gastos_comisiones
    )
    SELECT
        s.nombre AS socio_nombre,
        s.direccion AS socio_direccion,
        s.dni AS socio_dni,
        s.numero_cuenta AS cuenta_bancaria,
        g.albaran_tipo_aceituna AS balance_tipo_aceituna,
        g.albaran_calibre AS balance_calibre,
        g.albaran_kilos AS balance_kilos,
        g.albaran_subtotal AS balance_subtotal,
        g.balance_gastos_calculado,
        g.balance_comision_calculado,
        t.totales_total_haber - (t.totales_total_gastos + t.totales_total_comision) AS totales_base_imponible,
        (t.totales_total_haber - (t.totales_total_gastos + t.totales_total_comision)) * (SELECT iva FROM parametros LIMIT 1) / 100 AS totales_iva_calculado,
        (t.totales_total_haber - (t.totales_total_gastos + t.totales_total_comision)) + (t.totales_total_haber - (t.totales_total_gastos + t.totales_total_comision)) * (SELECT iva FROM parametros LIMIT 1) / 100 AS totales_subtotal_con_iva,
        ((t.totales_total_haber - (t.totales_total_gastos + t.totales_total_comision)) + (t.totales_total_haber - (t.totales_total_gastos + t.totales_total_comision)) * (SELECT iva FROM parametros LIMIT 1) / 100) * (SELECT retencion_irpf FROM parametros LIMIT 1) / 100 AS totales_retencion_irpf,
        ((t.totales_total_haber - (t.totales_total_gastos + t.totales_total_comision)) + (t.totales_total_haber - (t.totales_total_gastos + t.totales_total_comision)) * (SELECT iva FROM parametros LIMIT 1) / 100) - 
            (((t.totales_total_haber - (t.totales_total_gastos + t.totales_total_comision)) + (t.totales_total_haber - (t.totales_total_gastos + t.totales_total_comision)) * (SELECT iva FROM parametros LIMIT 1) / 100) * (SELECT retencion_irpf FROM parametros LIMIT 1) / 100) AS totales_saldo_a_su_favor,
        (SELECT totales_total_anticipado FROM anticipos_total) AS totales_total_anticipado,
        ((t.totales_total_haber - (t.totales_total_gastos + t.totales_total_comision)) + (t.totales_total_haber - (t.totales_total_gastos + t.totales_total_comision)) * (SELECT iva FROM parametros LIMIT 1) / 100) - 
            (((t.totales_total_haber - (t.totales_total_gastos + t.totales_total_comision)) + (t.totales_total_haber - (t.totales_total_gastos + t.totales_total_comision)) * (SELECT iva FROM parametros LIMIT 1) / 100) * (SELECT retencion_irpf FROM parametros LIMIT 1) / 100) - 
            (SELECT totales_total_anticipado FROM anticipos_total) AS totales_total_neto
    FROM
        socios s
        CROSS JOIN gastos_comisiones g
        CROSS JOIN totales t
    WHERE
        s.id_socio = p_id_socio;  -- Parámetro de función
END;
$$ LANGUAGE plpgsql;

```

Y para usarla:
```
SELECT * FROM generar_liquidacion(2, 4); -- Sustituye 1 y 2 por los valores de id_temporada y id_socio
```
