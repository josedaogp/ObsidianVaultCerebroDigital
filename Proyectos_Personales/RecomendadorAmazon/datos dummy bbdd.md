

```sql
-- Dummy data for blog feature
-- Inserts de productos adicionales (10 nuevos) para ampliar el catálogo
INSERT INTO products (
    id, asin, title, description,
    price, currency, url, last_checked_at,
    created_at, updated_at, is_public
) VALUES
  (
    'dddddddd-0000-0000-0000-000000000004',
    'B07N4M94X8',
    'Philips Hue Bombilla Inteligente',
    'Bombilla LED regulable con conexión ZigBee, compatible con Alexa y Google Home',
    24.99, 'EUR',
    'https://www.amazon.es/dp/B07N4M94X8?tag=tu-afiliado-21',
    NOW() - INTERVAL '4 days',
    NOW() - INTERVAL '25 days',
    NOW() - INTERVAL '4 days',
    TRUE
  ),
  (
    'dddddddd-0000-0000-0000-000000000005',
    'B0899VXMHD',
    'JBL Flip 5 Altavoz Bluetooth',
    'Altavoz portátil resistente al agua con hasta 12 horas de autonomía',
    99.95, 'EUR',
    'https://www.amazon.es/dp/B0899VXMHD?tag=tu-afiliado-21',
    NOW() - INTERVAL '3 days',
    NOW() - INTERVAL '30 days',
    NOW() - INTERVAL '3 days',
    TRUE
  ),
  (
    'dddddddd-0000-0000-0000-000000000006',
    'B08FRSBKT8',
    'Samsung Galaxy Watch 4',
    'Smartwatch con monitor de frecuencia cardíaca y seguimiento de actividad',
    269.00, 'EUR',
    'https://www.amazon.es/dp/B08FRSBKT8?tag=tu-afiliado-21',
    NOW() - INTERVAL '2 days',
    NOW() - INTERVAL '20 days',
    NOW() - INTERVAL '2 days',
    TRUE
  ),
  (
    'dddddddd-0000-0000-0000-000000000007',
    'B0863TXGM3',
    'Apple iPad (10ª generación)',
    'Tablet de 10,9" con chip A14 Bionic y 64 GB de almacenamiento',
    379.00, 'EUR',
    'https://www.amazon.es/dp/B0863TXGM3?tag=tu-afiliado-21',
    NOW() - INTERVAL '5 days',
    NOW() - INTERVAL '40 days',
    NOW() - INTERVAL '5 days',
    TRUE
  ),
  (
    'dddddddd-0000-0000-0000-000000000008',
    'B07HDBZN7Q',
    'GoPro HERO9 Black',
    'Cámara de acción con grabación 5K y estabilización HyperSmooth 3.0',
    299.99, 'EUR',
    'https://www.amazon.es/dp/B07HDBZN7Q?tag=tu-afiliado-21',
    NOW() - INTERVAL '6 days',
    NOW() - INTERVAL '50 days',
    NOW() - INTERVAL '6 days',
    TRUE
  ),
  (
    'dddddddd-0000-0000-0000-000000000009',
    'B07VGRJDFY',
    'Instant Pot Duo 7 en 1',
    'Olla programable multifunción (olla a presión, vapor, cocción lenta...)',
    89.99, 'EUR',
    'https://www.amazon.es/dp/B07VGRJDFY?tag=tu-afiliado-21',
    NOW() - INTERVAL '7 days',
    NOW() - INTERVAL '60 days',
    NOW() - INTERVAL '7 days',
    TRUE
  ),
  (
    'dddddddd-0000-0000-0000-000000000010',
    'B07PGL2ZSL',
    'Ninja Air Fryer',
    'Freidora de aire con capacidad de 3.8 L y tecnología de circulación de aire caliente',
    99.99, 'EUR',
    'https://www.amazon.es/dp/B07PGL2ZSL?tag=tu-afiliado-21',
    NOW() - INTERVAL '8 days',
    NOW() - INTERVAL '70 days',
    NOW() - INTERVAL '8 days',
    FALSE
  ),
  (
    'dddddddd-0000-0000-0000-000000000011',
    'B08KTZ8249',
    'iRobot Roomba 692',
    'Robot aspirador con navegación inteligente y conexión Wi-Fi',
    249.00, 'EUR',
    'https://www.amazon.es/dp/B08KTZ8249?tag=tu-afiliado-21',
    NOW() - INTERVAL '9 days',
    NOW() - INTERVAL '80 days',
    NOW() - INTERVAL '9 days',
    TRUE
  ),
  (
    'dddddddd-0000-0000-0000-000000000012',
    'B07XJ8C8F5',
    'Logitech MX Master 3',
    'Ratón inalámbrico ergonómico con rueda de desplazamiento MagSpeed',
    99.99, 'EUR',
    'https://www.amazon.es/dp/B07XJ8C8F5?tag=tu-afiliado-21',
    NOW() - INTERVAL '3 days',
    NOW() - INTERVAL '35 days',
    NOW() - INTERVAL '3 days',
    TRUE
  ),
  (
    'dddddddd-0000-0000-0000-000000000013',
    'B086DL3FJ3',
    'Dell UltraSharp U2720Q',
    'Monitor 27" 4K UHD con USB-C y cobertura sRGB 99%',
    479.00, 'EUR',
    'https://www.amazon.es/dp/B086DL3FJ3?tag=tu-afiliado-21',
    NOW() - INTERVAL '4 days',
    NOW() - INTERVAL '45 days',
    NOW() - INTERVAL '4 days',
    TRUE
  ),
  (
    'dddddddd-0000-0000-0000-000000000014',
    'B07YVP6KBM',
    'Sony WH-1000XM4',
    'Auriculares inalámbricos con cancelación de ruido activa',
    279.00, 'EUR',
    'https://www.amazon.es/dp/B07YVP6KBM?tag=tu-afiliado-21',
    NOW() - INTERVAL '2 days',
    NOW() - INTERVAL '28 days',
    NOW() - INTERVAL '2 days',
    TRUE
  );

-- Asegúrate de que los aisns sean únicos y las URL de afiliado incluyan tu tag correcto.


-- 1. Categorías de blog
INSERT INTO blog_categories (id, name, slug, parent_id) VALUES
  ('c1a1c1a1-0000-0000-0000-000000000001', 'Reseñas', 'resenas', NULL),
  ('c2a2c2a2-0000-0000-0000-000000000002', 'Tutoriales', 'tutoriales', NULL),
  ('c3a3c3a3-0000-0000-0000-000000000003', 'Noticias',   'noticias',   NULL);

-- 2. Etiquetas de blog
INSERT INTO blog_tags (id, name, slug) VALUES
  ('t1b1t1b1-0000-0000-0000-000000000001', 'Smart Home',    'smart-home'),
  ('t2b2t2b2-0000-0000-0000-000000000002', 'Audio',         'audio'),
  ('t3b3t3b3-0000-0000-0000-000000000003', 'E-readers',     'e-readers'),
  ('t4b4t4b4-0000-0000-0000-000000000004', 'Streaming',     'streaming'),
  ('t5b5t5b5-0000-0000-0000-000000000005', 'Cocina',        'cocina');

-- 3. Posts de blog
INSERT INTO blog_posts (
  id, author_id, category_id, title, slug, summary, content_html, published_at, created_at, updated_at
) VALUES
  (
    'p1d1p1d1-0000-0000-0000-000000000001',
    'bbbbbbbb-0000-0000-0000-000000000002',  -- Juan Pérez
    'c1a1c1a1-0000-0000-0000-000000000001',  -- Reseñas
    'Reseña: Echo Dot (4ª generación)',
    'resena-echo-dot-4',
    'Análisis completo del Echo Dot 4ª generación.',
    '<p>En este artículo revisamos el Echo Dot 4ª gen: diseño, sonido y precio.</p>',
    NOW() - INTERVAL '5 days',
    NOW() - INTERVAL '6 days',
    NOW() - INTERVAL '5 days'
  ),
  (
    'p2d2p2d2-0000-0000-0000-000000000002',
    'bbbbbbbb-0000-0000-0000-000000000001',  -- Ana García
    'c2a2c2a2-0000-0000-0000-000000000002',  -- Tutoriales
    'Cómo configurar tu bombilla Philips Hue',
    'configurar-philips-hue',
    'Guía paso a paso para conectar tu bombilla Philips Hue.',
    '<ol><li>Instala la app.</li><li>Sincroniza el puente.</li><li>Controla con la voz.</li></ol>',
    NOW() - INTERVAL '4 days',
    NOW() - INTERVAL '5 days',
    NOW() - INTERVAL '4 days'
  ),
  (
    'p3d3p3d3-0000-0000-0000-000000000003',
    'bbbbbbbb-0000-0000-0000-000000000002',  -- Juan Pérez
    'c1a1c1a1-0000-0000-0000-000000000001',  -- Reseñas
    'Top 5 E-readers de 2025',
    'top-5-e-readers-2025',
    'Comparativa de los mejores e-readers del año.',
    '<ul><li>Kindle Paperwhite</li><li> Kobo Clara HD</li>…</ul>',
    NOW() - INTERVAL '3 days',
    NOW() - INTERVAL '4 days',
    NOW() - INTERVAL '3 days'
  ),
  (
    'p4d4p4d4-0000-0000-0000-000000000004',
    'bbbbbbbb-0000-0000-0000-000000000001',  -- Ana García
    'c3a3c3a3-0000-0000-0000-000000000003',  -- Noticias
    'Nuevos lanzamientos en streaming',
    'nuevos-lanzamientos-streaming',
    'Lo último en dispositivos streaming de 2025.',
    '<p>Fire TV Stick 4K Max y Chromecast con Google TV Pro ya están aquí.</p>',
    NOW() - INTERVAL '2 days',
    NOW() - INTERVAL '3 days',
    NOW() - INTERVAL '2 days'
  ),
  (
    'p5d5p5d5-0000-0000-0000-000000000005',
    'bbbbbbbb-0000-0000-0000-000000000002',  -- Juan Pérez
    'c2a2c2a2-0000-0000-0000-000000000002',  -- Tutoriales
    'Guía de recetas fáciles con Instant Pot',
    'recetas-instant-pot',
    'Cinco recetas rápidas para tu Instant Pot Duo.',
    '<p>Prepara arroz, estofados, yogur y más en una sola olla.</p>',
    NOW() - INTERVAL '1 day',
    NOW() - INTERVAL '2 days',
    NOW() - INTERVAL '1 day'
  );

-- 4. Relación Posts ↔ Etiquetas
INSERT INTO blog_post_tags (post_id, tag_id) VALUES
  ('p1d1p1d1-0000-0000-0000-000000000001', 't2b2t2b2-0000-0000-0000-000000000002'),  -- Audio
  ('p2d2p2d2-0000-0000-0000-000000000002', 't1b1t1b1-0000-0000-0000-000000000001'),  -- Smart Home
  ('p3d3p3d3-0000-0000-0000-000000000003', 't3b3t3b3-0000-0000-0000-000000000003'),  -- E-readers
  ('p4d4p4d4-0000-0000-0000-000000000004', 't4b4t4b4-0000-0000-0000-000000000004'),  -- Streaming
  ('p5d5p5d5-0000-0000-0000-000000000005', 't5b5t5b5-0000-0000-0000-000000000005');  -- Cocina

-- 5. Productos relacionados en el post
INSERT INTO blog_post_products (post_id, product_id) VALUES
  ('p1d1p1d1-0000-0000-0000-000000000001', 'dddddddd-0000-0000-0000-000000000001'),  -- Echo Dot
  ('p2d2p2d2-0000-0000-0000-000000000002', 'dddddddd-0000-0000-0000-000000000004'),  -- Philips Hue
  ('p3d3p3d3-0000-0000-0000-000000000003', 'dddddddd-0000-0000-0000-000000000002'),  -- Kindle Paperwhite
  ('p4d4p4d4-0000-0000-0000-000000000004', 'dddddddd-0000-0000-0000-000000000003'),  -- Fire TV Stick
  ('p5d5p5d5-0000-0000-0000-000000000005', 'dddddddd-0000-0000-0000-000000000009');  -- Instant Pot

-- 6. Publicidad simple
INSERT INTO blog_ads (id, name, content_html, is_active) VALUES
  (
    'a1a1ad1a-0000-0000-0000-000000000001',
    'Anuncio Sidebar',
    '<div style="padding:10px; border:1px solid #ccc;"><a href="https://www.amazon.es?tag=tu-afiliado-21">¡Compra ahora con descuento!</a></div>',
    TRUE
  ),
  (
    'a2a2ad2a-0000-0000-0000-000000000002',
    'Anuncio In-Post',
    '<script>/* código de Google AdSense */</script>',
    TRUE
  );



```

