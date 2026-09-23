-- Insertar datos de prueba para la tabla 'variedades'
INSERT INTO variedades (variedad) VALUES
('Hojiblanca'),
('Picual'),
('Arbequina'),
('Cornicabra');

-- Insertar datos de prueba para la tabla 'temporadas'
INSERT INTO temporadas (nombre_temporada, fecha_inicio, fecha_fin) VALUES
('Temporada 2022', '2022-01-01', '2022-12-31'),
('Temporada 2023', '2023-01-01', '2023-12-31');

-- Insertar datos de prueba para la tabla 'bancos'
INSERT INTO bancos (codigo, nombre_banco, telefono1, telefono2, fax, direccion, notas) VALUES
('001', 'Banco Popular', '123456789', '987654321', '1122334455', 'Calle Falsa 123, Sevilla', 'Banco principal'),
('002', 'Santander', '223344556', '665544332', '3344556677', 'Calle Real 45, Huelva', 'Banco secundario');

-- Insertar datos de prueba para la tabla 'socios'
INSERT INTO socios (id_localidad, id_banco, numero_cliente, codigo_postal, activo, dni, nombre, direccion, telefono1, telefono2, fax, movil, notas, numero_cuenta, saldo) VALUES
(1, 1, '12345', '21001', TRUE, '12345678A', 'Juan Pérez', 'Calle Mayor 1, Sevilla', '123456789', NULL, NULL, '987654321', 'Cliente confiable', 'ES7620770024003102575766', 1000.50),
(2, 2, '67890', '41001', TRUE, '87654321B', 'María López', 'Avenida del Sol 22, Huelva', '987654321', '123456789', NULL, '665544332', 'Notas adicionales', 'ES9121000418450200051332', 250.75);

-- Insertar datos de prueba para la tabla 'albaranes'
INSERT INTO albaranes (id_temporada, id_socio, numero_albaran, fecha_albaran, tpc_iva, base_imponible, cuota_iva, subtotal, total_albaran, observacion, bruto, tara, seleccionado) VALUES
(1, 1, 'ALB-2023-001', '2023-05-10', 21.00, 1000.00, 210.00, 1210.00, 1210.00, 'Entrega de aceitunas', 1500.00, 500.00, TRUE),
(2, 2, 'ALB-2023-002', '2023-06-15', 10.00, 2000.00, 200.00, 2200.00, 2200.00, 'Entrega de aceitunas Picual', 2500.00, 500.00, FALSE);

-- Insertar datos de prueba para la tabla 'anticipos'
INSERT INTO anticipos (id_temporada, id_socio, numero_anticipo, fecha_anticipo, concepto, base_imponible, iva, compensacion, total_factura, retencion_irpf, retenido, importe, cheque_numero) VALUES
(1, 1, 'ANT-2023-001', '2023-03-01', 'Anticipo para temporada', 1000.00, 210.00, 0.00, 1210.00, 15.00, 150.00, 1060.00, 'CHQ123'),
(2, 2, 'ANT-2023-002', '2023-04-01', 'Anticipo especial', 2000.00, 200.00, 50.00, 2250.00, 15.00, 300.00, 1950.00, 'CHQ456');

-- Insertar datos de prueba para la tabla 'tipos_aceituna'
INSERT INTO tipos_aceituna (id_variedad, calibre, tipo_aceituna, precio_por_kg, gastos, comision, comentario) VALUES
(1, 'Extra', 'Aceituna de mesa', 2.50, 0.10, 5.00, 'Aceituna premium para exportación'),
(2, 'Medio', 'Aceituna de aceite', 1.80, 0.15, 3.00, 'Aceituna para extracción de aceite de calidad');

-- Insertar datos de prueba para la tabla 'saldos'
INSERT INTO saldos (id_agricultor, id_temporada, total_kg, saldo, fecha_liquidacion) VALUES
(1, 1, 1500.00, 1000.50, '2023-07-01'),
(2, 2, 2500.00, 250.75, '2023-08-01');

-- Insertar datos de prueba para la tabla 'parametros'
INSERT INTO parametros (iva, aviso_backup, periodo_aviso, backup_last, unidad, comision, retencion_irpf, tipo_impresora) VALUES
(21.00, TRUE, 7, '2023-11-15', 'kg', 2.50, 15.00, 'Termal'),
(10.00, FALSE, 14, '2023-10-01', 'litro', 3.00, 15.00, 'Laser');
