NOTA: Falta añadirle los anticipos


SELECT 
    s.id_socio,
    s.nombre AS nombre_socio,
    s.dni,
    s.numero_cliente,
    a.id_albaran,
    a.numero_albaran,
    a.fecha_albaran,
    a.total_albaran,
    ta.tipo_aceituna,
	ta.gastos,
	ta.comision,
	ta.calibre
FROM 
    socios s
JOIN 
    albaranes a ON a.id_socio = s.id_socio
JOIN 
    tipos_aceituna ta ON ta.id_tipo_aceituna = a.id_aceituna -- Ajusta esta relación si corresponde a otro campo
ORDER BY 
    a.fecha_albaran;
