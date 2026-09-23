	### **1. Tablas Principales**

```sql
-- Enable UUID generation
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

-- 1. Roles
CREATE TABLE roles (
    id          UUID PRIMARY KEY DEFAULT uuid_generate_v4(),  -- Identificador único del rol
    name        TEXT NOT NULL UNIQUE,                         -- Nombre interno del rol (e.g. 'admin', 'user')
    description TEXT                                               -- Descripción del rol
);

-- 2. Usuarios y Autenticación
CREATE TABLE users (
    id                       UUID      PRIMARY KEY DEFAULT uuid_generate_v4(),  -- ID único de usuario
    email                    TEXT      NOT NULL UNIQUE,                         -- Correo electrónico de login
    password_hash            TEXT      NOT NULL,                                -- Hash de la contraseña
    name                     TEXT,                                           -- Nombre completo
    role_id                  UUID      NOT NULL REFERENCES roles(id) ON DELETE RESTRICT, -- Referencia al rol del usuario
    password_reset_token     TEXT,                                           -- Token temporal para reset de contraseña
    password_reset_expires   TIMESTAMPTZ,                                    -- Fecha/hora de expiración del token
    locale                   TEXT,                                           -- Preferencia de idioma/región
    currency                 CHAR(3),                                        -- Moneda preferida (ISO 4217)
    other_settings           JSONB,                                          -- Ajustes adicionales (tema, notificaciones...)
    created_at               TIMESTAMPTZ NOT NULL DEFAULT now(),              -- Fecha de creación de la cuenta
    updated_at               TIMESTAMPTZ NOT NULL DEFAULT now(),              -- Última actualización de perfil
    is_active                BOOLEAN   NOT NULL DEFAULT TRUE                  -- Si la cuenta está activa o suspendida
);

-- 3. Zonas/Categorías de Productos
CREATE TABLE zones (
    id             UUID      PRIMARY KEY DEFAULT uuid_generate_v4(),  -- ID único de zona
    name           TEXT      NOT NULL,                               -- Nombre de la zona/categoría
    description    TEXT,                                             -- Descripción de la zona
    parent_zone_id UUID      REFERENCES zones(id) ON DELETE SET NULL -- Zona padre para jerarquías
);

-- 4. Productos
CREATE TABLE products (
    id                UUID      PRIMARY KEY DEFAULT uuid_generate_v4(),  -- ID único de producto
    asin              TEXT      NOT NULL UNIQUE,                         -- Identificador ASIN de Amazon
    title             TEXT      NOT NULL,                                -- Título del producto
    description       TEXT,                                             -- Descripción detallada
    price             NUMERIC(12,2),                                    -- Precio actual
    currency          CHAR(3),                                          -- Moneda (ISO 4217)
    url               TEXT      NOT NULL,                                -- Enlace completo con afiliado
    last_checked_at   TIMESTAMPTZ,                                      -- Fecha/hora de última comprobación de precio
    is_public         BOOLEAN   NOT NULL DEFAULT FALSE,                 -- Si el producto está visible públicamente
    created_at        TIMESTAMPTZ NOT NULL DEFAULT now(),                -- Fecha de alta en el sistema
    updated_at        TIMESTAMPTZ NOT NULL DEFAULT now()                 -- Fecha de última modificación
);

-- Imágenes de productos (varias por producto)
CREATE TABLE product_images (
    id         UUID      PRIMARY KEY DEFAULT uuid_generate_v4(),  -- ID único de la imagen
    product_id UUID      NOT NULL REFERENCES products(id) ON DELETE CASCADE,  -- Producto asociado
    image_url  TEXT      NOT NULL,                                -- URL de la imagen
    position   INTEGER   NOT NULL DEFAULT 0                       -- Orden de presentación
);

-- Relación n-m Productos ↔ Zonas
CREATE TABLE product_zones (
    id         UUID      PRIMARY KEY DEFAULT uuid_generate_v4(),  -- ID único de la relación
    product_id UUID      NOT NULL REFERENCES products(id) ON DELETE CASCADE,
    zone_id    UUID      NOT NULL REFERENCES zones(id) ON DELETE CASCADE,
    UNIQUE(product_id, zone_id)
);

-- 5. Conversaciones con IA
CREATE TABLE conversations (
    id            UUID      PRIMARY KEY DEFAULT uuid_generate_v4(),  -- ID de la conversación
    user_id       UUID      NOT NULL REFERENCES users(id) ON DELETE CASCADE,  -- Usuario que inicia
    title         TEXT,                                             -- Título descriptivo de la conversación
    summary       TEXT,                                             -- Resumen automático o manual de la charla
    started_at    TIMESTAMPTZ NOT NULL DEFAULT now(),                -- Cuándo empezó
    ended_at      TIMESTAMPTZ                                      -- Cuándo finalizó (opcional)
);

CREATE TABLE messages (
    id              UUID      PRIMARY KEY DEFAULT uuid_generate_v4(),  -- ID del mensaje
    conversation_id UUID      NOT NULL REFERENCES conversations(id) ON DELETE CASCADE,  -- Conversación padre
    sender          TEXT      NOT NULL CHECK (sender IN ('user','ai')),  -- Quién envía: usuario o IA
    content         TEXT      NOT NULL,                                -- Texto del mensaje
    created_at      TIMESTAMPTZ NOT NULL DEFAULT now()                  -- Fecha/hora de envío
);

-- 6. Favoritos, Historial y Métricas de Búsqueda
CREATE TABLE favorites (
    id         UUID      PRIMARY KEY DEFAULT uuid_generate_v4(),  -- ID único
    user_id    UUID      NOT NULL REFERENCES users(id) ON DELETE CASCADE,  -- Usuario
    product_id UUID      NOT NULL REFERENCES products(id) ON DELETE CASCADE,  -- Producto
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),                -- Cuándo se marcó favorito
    UNIQUE(user_id, product_id)
);

CREATE TABLE view_history (
    id         UUID      PRIMARY KEY DEFAULT uuid_generate_v4(),  -- ID único
    user_id    UUID      NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    product_id UUID      NOT NULL REFERENCES products(id) ON DELETE CASCADE,
    viewed_at  TIMESTAMPTZ NOT NULL DEFAULT now()                  -- Cuándo vio el producto
);

CREATE TABLE search_metrics (
    id           UUID      PRIMARY KEY DEFAULT uuid_generate_v4(),  -- ID único de búsqueda
    user_id      UUID      REFERENCES users(id) ON DELETE SET NULL, -- Usuario (nullable si anónimo)
    query_text   TEXT      NOT NULL,                                -- Texto de búsqueda
    filters      JSONB,                                             -- Filtros aplicados (precio, marca…)
    result_count INTEGER,                                           -- Número de resultados devueltos
    searched_at  TIMESTAMPTZ NOT NULL DEFAULT now()                  -- Fecha/hora de búsqueda
    -- Podrías añadir duración de consulta, tasa de clics tras búsqueda, etc., según necesidad
);

-- 7. Alertas de Precio y Notificaciones
CREATE TABLE price_alerts (
    id           UUID      PRIMARY KEY DEFAULT uuid_generate_v4(),  -- ID única de alerta
    user_id      UUID      NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    product_id   UUID      NOT NULL REFERENCES products(id) ON DELETE CASCADE,
    target_price NUMERIC(12,2) NOT NULL,                           -- Precio deseado
    is_active    BOOLEAN   NOT NULL DEFAULT TRUE,                  -- Activa/desactivada
    created_at   TIMESTAMPTZ NOT NULL DEFAULT now()                -- Fecha de creación
);

CREATE TABLE notifications (
    id         UUID      PRIMARY KEY DEFAULT uuid_generate_v4(),  -- ID de notificación
    user_id    UUID      NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    type       TEXT      NOT NULL CHECK (type IN ('price_alert','promo','system')),  -- Tipo de alerta
    payload    JSONB,                                             -- Datos adicionales (mensaje, link…)
    is_read    BOOLEAN   NOT NULL DEFAULT FALSE,                  -- Leído/no leído
    created_at TIMESTAMPTZ NOT NULL DEFAULT now()                 -- Fecha de envío
);

-- 8. Clics en Productos (para métricas detalladas)
CREATE TABLE clicks (
    id          UUID      PRIMARY KEY DEFAULT uuid_generate_v4(),  -- ID único de clic
    user_id     UUID      REFERENCES users(id) ON DELETE SET NULL, -- Usuario que hizo clic (nullable)
    product_id  UUID      NOT NULL REFERENCES products(id) ON DELETE CASCADE, -- Producto clicado
    clicked_at  TIMESTAMPTZ NOT NULL DEFAULT now(),                -- Fecha/hora del clic
    context     TEXT                                               -- Contexto (e.g. 'search_result','recommendation')
);

-- 9. Métricas Agregadas de Producto (diarias)
CREATE TABLE product_metrics (
    id               UUID      PRIMARY KEY DEFAULT uuid_generate_v4(),  -- ID único de métrica
    product_id       UUID      NOT NULL REFERENCES products(id) ON DELETE CASCADE,
    views_count      INTEGER   NOT NULL DEFAULT 0,                     -- Número de vistas en el día
    favorites_count  INTEGER   NOT NULL DEFAULT 0,                     -- Veces marcado favorito en el día
    purchases_count  INTEGER   NOT NULL DEFAULT 0,                     -- Compras (si se rastrean)
    clicks_count     INTEGER   NOT NULL DEFAULT 0,                     -- Número de clics en el día
    metric_date      DATE      NOT NULL,                               -- Fecha de la métrica
    UNIQUE(product_id, metric_date)
);

-- 1. Categorías de blog (jerárquicas)
CREATE TABLE blog_categories (
  id   UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  name TEXT NOT NULL,
  slug TEXT NOT NULL UNIQUE,
  parent_id UUID REFERENCES blog_categories(id) ON DELETE SET NULL
);

-- 2. Etiquetas de blog
CREATE TABLE blog_tags (
  id   UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  name TEXT NOT NULL,
  slug TEXT NOT NULL UNIQUE
);

-- 3. Posts de blog
CREATE TABLE blog_posts (
  id            UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  author_id     UUID REFERENCES users(id) ON DELETE SET NULL,  -- autor opcional
  category_id   UUID REFERENCES blog_categories(id) ON DELETE SET NULL,  -- categoría principal
  title         TEXT NOT NULL,
  slug          TEXT NOT NULL UNIQUE,
  summary       TEXT,                       -- resumen corto
  content_html  TEXT NOT NULL,              -- HTML del post
  published_at  TIMESTAMPTZ,                -- fecha de publicación
  created_at    TIMESTAMPTZ NOT NULL DEFAULT now(),
  updated_at    TIMESTAMPTZ NOT NULL DEFAULT now()
);

-- 4. Relación Posts ↔ Etiquetas
CREATE TABLE blog_post_tags (
  post_id UUID NOT NULL REFERENCES blog_posts(id) ON DELETE CASCADE,
  tag_id  UUID NOT NULL REFERENCES blog_tags(id) ON DELETE CASCADE,
  PRIMARY KEY (post_id, tag_id)
);

-- 5. Productos relacionados en el post
CREATE TABLE blog_post_products (
  post_id    UUID NOT NULL REFERENCES blog_posts(id) ON DELETE CASCADE,
  product_id UUID NOT NULL REFERENCES products(id) ON DELETE CASCADE,
  PRIMARY KEY (post_id, product_id)
);

-- 6. Publicidad simple (opcional en el footer o sidebar)
CREATE TABLE blog_ads (
  id          UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  name        TEXT NOT NULL,     -- p. ej. 'sidebar promo'
  content_html TEXT NOT NULL,    -- HTML o script del anuncio
  is_active   BOOLEAN NOT NULL DEFAULT TRUE
);


```