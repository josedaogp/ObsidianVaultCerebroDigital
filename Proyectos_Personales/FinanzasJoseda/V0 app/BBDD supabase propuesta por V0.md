## Versión 2 (potenciada chagtpt)

```sql
-- =============================================
-- FinanzasJoseda - Base de Datos Multiusuario
-- Incluye Supabase Auth + RLS + Tipos en tablas separadas
-- =============================================

-- =============================================
-- FUNCIONES AUXILIARES
-- =============================================

CREATE OR REPLACE FUNCTION update_updated_at_column()
RETURNS TRIGGER AS $$
BEGIN
    NEW.updated_at = NOW();
    RETURN NEW;
END;
$$ LANGUAGE 'plpgsql';

-- =============================================
-- TABLAS DE TIPOS
-- =============================================

CREATE TABLE category_types (
    id SERIAL PRIMARY KEY,
    name VARCHAR(50) UNIQUE NOT NULL
);

CREATE TABLE asset_types (
    id SERIAL PRIMARY KEY,
    name VARCHAR(50) UNIQUE NOT NULL
);

CREATE TABLE wallet_transaction_types (
    id SERIAL PRIMARY KEY,
    name VARCHAR(50) UNIQUE NOT NULL
);

CREATE TABLE asset_transaction_types (
    id SERIAL PRIMARY KEY,
    name VARCHAR(50) UNIQUE NOT NULL
);

-- =============================================
-- TABLAS PRINCIPALES
-- =============================================

CREATE TABLE wallets (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    user_id UUID NOT NULL REFERENCES auth.users(id) ON DELETE CASCADE,
    name VARCHAR(255) NOT NULL,
    current_balance DECIMAL(10,2) NOT NULL DEFAULT 0,
    target_balance DECIMAL(10,2),
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

CREATE TABLE assets (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    user_id UUID NOT NULL REFERENCES auth.users(id) ON DELETE CASCADE,
    name VARCHAR(255) NOT NULL,
    asset_type_id INTEGER REFERENCES asset_types(id) ON DELETE SET NULL,
    current_balance DECIMAL(10,2) NOT NULL DEFAULT 0,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

CREATE TABLE categories (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    user_id UUID NOT NULL REFERENCES auth.users(id) ON DELETE CASCADE,
    name VARCHAR(255) NOT NULL,
    category_type_id INTEGER REFERENCES category_types(id) ON DELETE SET NULL,
    monthly_budget DECIMAL(10,2) NOT NULL DEFAULT 0,
    annual_budget DECIMAL(10,2),
    active BOOLEAN NOT NULL DEFAULT true,
    wallet_id UUID REFERENCES wallets(id) ON DELETE SET NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

CREATE TABLE monthly_ingestions (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    user_id UUID NOT NULL REFERENCES auth.users(id) ON DELETE CASCADE,
    month INTEGER NOT NULL CHECK (month BETWEEN 1 AND 12),
    year INTEGER NOT NULL CHECK (year >= 2000),
    date DATE NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    UNIQUE(user_id, month, year)
);

CREATE TABLE category_expenses (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    user_id UUID NOT NULL REFERENCES auth.users(id) ON DELETE CASCADE,
    monthly_ingestion_id UUID NOT NULL REFERENCES monthly_ingestions(id) ON DELETE CASCADE,
    category_id UUID NOT NULL REFERENCES categories(id) ON DELETE CASCADE,
    amount DECIMAL(10,2) NOT NULL DEFAULT 0,
    wallet_id UUID REFERENCES wallets(id) ON DELETE SET NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    UNIQUE(user_id, monthly_ingestion_id, category_id)
);

CREATE TABLE incomes (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    user_id UUID NOT NULL REFERENCES auth.users(id) ON DELETE CASCADE,
    monthly_ingestion_id UUID NOT NULL REFERENCES monthly_ingestions(id) ON DELETE CASCADE,
    amount DECIMAL(10,2) NOT NULL DEFAULT 0,
    asset_id UUID NOT NULL REFERENCES assets(id) ON DELETE CASCADE,
    description TEXT,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

CREATE TABLE distribution_rules (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    user_id UUID NOT NULL REFERENCES auth.users(id) ON DELETE CASCADE,
    wallet_id UUID NOT NULL REFERENCES wallets(id) ON DELETE CASCADE,
    type VARCHAR(20) NOT NULL CHECK (type IN ('percentage', 'fixed')),
    value DECIMAL(10,2) NOT NULL DEFAULT 0,
    priority INTEGER NOT NULL DEFAULT 1,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    UNIQUE(user_id, wallet_id)
);

CREATE TABLE wallet_transactions (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    user_id UUID NOT NULL REFERENCES auth.users(id) ON DELETE CASCADE,
    wallet_id UUID NOT NULL REFERENCES wallets(id) ON DELETE CASCADE,
    monthly_ingestion_id UUID REFERENCES monthly_ingestions(id) ON DELETE SET NULL,
    amount DECIMAL(10,2) NOT NULL,
    wallet_transaction_type_id INTEGER REFERENCES wallet_transaction_types(id) ON DELETE SET NULL,
    description TEXT,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

CREATE TABLE asset_transactions (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    user_id UUID NOT NULL REFERENCES auth.users(id) ON DELETE CASCADE,
    asset_id UUID NOT NULL REFERENCES assets(id) ON DELETE CASCADE,
    monthly_ingestion_id UUID REFERENCES monthly_ingestions(id) ON DELETE SET NULL,
    amount DECIMAL(10,2) NOT NULL,
    asset_transaction_type_id INTEGER REFERENCES asset_transaction_types(id) ON DELETE SET NULL,
    description TEXT,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- =============================================
-- TRIGGERS DE UPDATED_AT
-- =============================================

CREATE TRIGGER update_categories_updated_at
BEFORE UPDATE ON categories
FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_wallets_updated_at
BEFORE UPDATE ON wallets
FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_assets_updated_at
BEFORE UPDATE ON assets
FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_monthly_ingestions_updated_at
BEFORE UPDATE ON monthly_ingestions
FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_distribution_rules_updated_at
BEFORE UPDATE ON distribution_rules
FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

-- =============================================
-- RLS (ROW LEVEL SECURITY)
-- =============================================

ALTER TABLE wallets ENABLE ROW LEVEL SECURITY;
ALTER TABLE assets ENABLE ROW LEVEL SECURITY;
ALTER TABLE categories ENABLE ROW LEVEL SECURITY;
ALTER TABLE monthly_ingestions ENABLE ROW LEVEL SECURITY;
ALTER TABLE category_expenses ENABLE ROW LEVEL SECURITY;
ALTER TABLE incomes ENABLE ROW LEVEL SECURITY;
ALTER TABLE distribution_rules ENABLE ROW LEVEL SECURITY;
ALTER TABLE wallet_transactions ENABLE ROW LEVEL SECURITY;
ALTER TABLE asset_transactions ENABLE ROW LEVEL SECURITY;

-- Políticas por usuario autenticado
-- Políticas por usuario autenticado (una por tabla)
CREATE POLICY "User can access own wallets"
  ON wallets FOR ALL
  USING (user_id = auth.uid());

CREATE POLICY "User can access own assets"
  ON assets FOR ALL
  USING (user_id = auth.uid());

CREATE POLICY "User can access own categories"
  ON categories FOR ALL
  USING (user_id = auth.uid());

CREATE POLICY "User can access own monthly_ingestions"
  ON monthly_ingestions FOR ALL
  USING (user_id = auth.uid());

CREATE POLICY "User can access own category_expenses"
  ON category_expenses FOR ALL
  USING (user_id = auth.uid());

CREATE POLICY "User can access own incomes"
  ON incomes FOR ALL
  USING (user_id = auth.uid());

CREATE POLICY "User can access own distribution_rules"
  ON distribution_rules FOR ALL
  USING (user_id = auth.uid());

CREATE POLICY "User can access own wallet_transactions"
  ON wallet_transactions FOR ALL
  USING (user_id = auth.uid());

CREATE POLICY "User can access own asset_transactions"
  ON asset_transactions FOR ALL
  USING (user_id = auth.uid());


```

```sql
-- =============================================
-- DATOS DE EJEMPLO PARA TIPOS
-- =============================================

INSERT INTO category_types (name) VALUES 
    ('gasto'),
    ('gasto_acumulativo'),
    ('gasto_mixto'),
    ('gasto_acumulativo_opcional');

INSERT INTO asset_types (name) VALUES 
    ('cuenta_bancaria'),
    ('efectivo'),
    ('inversion'),
    ('propiedad'),
    ('otro');

INSERT INTO wallet_transaction_types (name) VALUES 
    ('excess'),
    ('surplus'),
    ('accumulative'),
    ('distribution'),
    ('manual');

INSERT INTO asset_transaction_types (name) VALUES 
    ('income'),
    ('manual');

```
## Versión 1
```sql
-- =============================================
-- ESQUEMA COMPLETO PARA SUPABASE
-- Aplicación de Gestión Financiera Personal
-- =============================================

-- 1. TABLA: categories (Categorías de Gasto)
CREATE TABLE categories (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    type VARCHAR(50) NOT NULL CHECK (type IN ('gasto', 'gasto_acumulativo', 'gasto_mixto', 'gasto_acumulativo_opcional')),
    monthly_budget DECIMAL(10,2) NOT NULL DEFAULT 0,
    annual_budget DECIMAL(10,2),
    active BOOLEAN NOT NULL DEFAULT true,
    wallet_id UUID REFERENCES wallets(id) ON DELETE SET NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- 2. TABLA: wallets (Monederos/Fondos por Objetivos)
CREATE TABLE wallets (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    current_balance DECIMAL(10,2) NOT NULL DEFAULT 0,
    target_balance DECIMAL(10,2),
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- 3. TABLA: assets (Bienes/Cuentas/Activos)
CREATE TABLE assets (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    type VARCHAR(50) NOT NULL CHECK (type IN ('cuenta_bancaria', 'efectivo', 'inversion', 'propiedad', 'otro')),
    current_balance DECIMAL(10,2) NOT NULL DEFAULT 0,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- 4. TABLA: monthly_ingestions (Ingestas Mensuales)
CREATE TABLE monthly_ingestions (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    month INTEGER NOT NULL CHECK (month >= 1 AND month <= 12),
    year INTEGER NOT NULL CHECK (year >= 2000),
    date DATE NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    UNIQUE(month, year)
);

-- 5. TABLA: category_expenses (Gastos por Categoría en cada Ingesta)
CREATE TABLE category_expenses (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    monthly_ingestion_id UUID NOT NULL REFERENCES monthly_ingestions(id) ON DELETE CASCADE,
    category_id UUID NOT NULL REFERENCES categories(id) ON DELETE CASCADE,
    amount DECIMAL(10,2) NOT NULL DEFAULT 0,
    wallet_id UUID REFERENCES wallets(id) ON DELETE SET NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    UNIQUE(monthly_ingestion_id, category_id)
);

-- 6. TABLA: incomes (Ingresos Mensuales)
CREATE TABLE incomes (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    monthly_ingestion_id UUID NOT NULL REFERENCES monthly_ingestions(id) ON DELETE CASCADE,
    amount DECIMAL(10,2) NOT NULL DEFAULT 0,
    asset_id UUID NOT NULL REFERENCES assets(id) ON DELETE CASCADE,
    description TEXT,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- 7. TABLA: distribution_rules (Reglas de Distribución del Bote Mensual)
CREATE TABLE distribution_rules (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    wallet_id UUID NOT NULL REFERENCES wallets(id) ON DELETE CASCADE,
    type VARCHAR(20) NOT NULL CHECK (type IN ('percentage', 'fixed')),
    value DECIMAL(10,2) NOT NULL DEFAULT 0,
    priority INTEGER NOT NULL DEFAULT 1,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    UNIQUE(wallet_id)
);

-- 8. TABLA: wallet_transactions (Historial de Transacciones de Monederos)
CREATE TABLE wallet_transactions (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    wallet_id UUID NOT NULL REFERENCES wallets(id) ON DELETE CASCADE,
    monthly_ingestion_id UUID REFERENCES monthly_ingestions(id) ON DELETE SET NULL,
    amount DECIMAL(10,2) NOT NULL,
    type VARCHAR(50) NOT NULL CHECK (type IN ('excess', 'surplus', 'accumulative', 'distribution', 'manual')),
    description TEXT,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- 9. TABLA: asset_transactions (Historial de Transacciones de Bienes)
CREATE TABLE asset_transactions (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    asset_id UUID NOT NULL REFERENCES assets(id) ON DELETE CASCADE,
    monthly_ingestion_id UUID REFERENCES monthly_ingestions(id) ON DELETE SET NULL,
    amount DECIMAL(10,2) NOT NULL,
    type VARCHAR(50) NOT NULL CHECK (type IN ('income', 'manual')),
    description TEXT,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- =============================================
-- ÍNDICES PARA OPTIMIZACIÓN
-- =============================================

-- Índices para consultas frecuentes
CREATE INDEX idx_categories_active ON categories(active);
CREATE INDEX idx_categories_type ON categories(type);
CREATE INDEX idx_monthly_ingestions_date ON monthly_ingestions(year, month);
CREATE INDEX idx_category_expenses_ingestion ON category_expenses(monthly_ingestion_id);
CREATE INDEX idx_incomes_ingestion ON incomes(monthly_ingestion_id);
CREATE INDEX idx_wallet_transactions_wallet ON wallet_transactions(wallet_id);
CREATE INDEX idx_asset_transactions_asset ON asset_transactions(asset_id);

-- =============================================
-- TRIGGERS PARA UPDATED_AT
-- =============================================

-- Función para actualizar updated_at
CREATE OR REPLACE FUNCTION update_updated_at_column()
RETURNS TRIGGER AS $$
BEGIN
    NEW.updated_at = NOW();
    RETURN NEW;
END;
$$ language 'plpgsql';

-- Triggers para tablas que necesitan updated_at
CREATE TRIGGER update_categories_updated_at BEFORE UPDATE ON categories FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();
CREATE TRIGGER update_wallets_updated_at BEFORE UPDATE ON wallets FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();
CREATE TRIGGER update_assets_updated_at BEFORE UPDATE ON assets FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();
CREATE TRIGGER update_monthly_ingestions_updated_at BEFORE UPDATE ON monthly_ingestions FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();
CREATE TRIGGER update_distribution_rules_updated_at BEFORE UPDATE ON distribution_rules FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

-- =============================================
-- POLÍTICAS RLS (Row Level Security)
-- =============================================

-- Habilitar RLS en todas las tablas
ALTER TABLE categories ENABLE ROW LEVEL SECURITY;
ALTER TABLE wallets ENABLE ROW LEVEL SECURITY;
ALTER TABLE assets ENABLE ROW LEVEL SECURITY;
ALTER TABLE monthly_ingestions ENABLE ROW LEVEL SECURITY;
ALTER TABLE category_expenses ENABLE ROW LEVEL SECURITY;
ALTER TABLE incomes ENABLE ROW LEVEL SECURITY;
ALTER TABLE distribution_rules ENABLE ROW LEVEL SECURITY;
ALTER TABLE wallet_transactions ENABLE ROW LEVEL SECURITY;
ALTER TABLE asset_transactions ENABLE ROW LEVEL SECURITY;

-- Políticas básicas (ajustar según necesidades de autenticación)
-- Por ahora, acceso completo para usuarios autenticados

CREATE POLICY "Enable all operations for authenticated users" ON categories FOR ALL USING (auth.role() = 'authenticated');
CREATE POLICY "Enable all operations for authenticated users" ON wallets FOR ALL USING (auth.role() = 'authenticated');
CREATE POLICY "Enable all operations for authenticated users" ON assets FOR ALL USING (auth.role() = 'authenticated');
CREATE POLICY "Enable all operations for authenticated users" ON monthly_ingestions FOR ALL USING (auth.role() = 'authenticated');
CREATE POLICY "Enable all operations for authenticated users" ON category_expenses FOR ALL USING (auth.role() = 'authenticated');
CREATE POLICY "Enable all operations for authenticated users" ON incomes FOR ALL USING (auth.role() = 'authenticated');
CREATE POLICY "Enable all operations for authenticated users" ON distribution_rules FOR ALL USING (auth.role() = 'authenticated');
CREATE POLICY "Enable all operations for authenticated users" ON wallet_transactions FOR ALL USING (auth.role() = 'authenticated');
CREATE POLICY "Enable all operations for authenticated users" ON asset_transactions FOR ALL USING (auth.role() = 'authenticated');

-- =============================================
-- DATOS DE EJEMPLO (OPCIONAL)
-- =============================================

-- Insertar algunos datos de ejemplo
INSERT INTO wallets (id, name, current_balance, target_balance) VALUES
    ('550e8400-e29b-41d4-a716-446655440001', 'Emergencias', 1000.00, 5000.00),
    ('550e8400-e29b-41d4-a716-446655440002', 'Vacaciones', 500.00, 2000.00),
    ('550e8400-e29b-41d4-a716-446655440003', 'Coche', 300.00, 1500.00);

INSERT INTO assets (id, name, type, current_balance) VALUES
    ('660e8400-e29b-41d4-a716-446655440001', 'Cuenta Corriente', 'cuenta_bancaria', 2500.00),
    ('660e8400-e29b-41d4-a716-446655440002', 'Cuenta Ahorro', 'cuenta_bancaria', 5000.00),
    ('660e8400-e29b-41d4-a716-446655440003', 'Efectivo', 'efectivo', 200.00);

INSERT INTO categories (id, name, type, monthly_budget, active) VALUES
    ('770e8400-e29b-41d4-a716-446655440001', 'Alimentación', 'gasto', 300.00, true),
    ('770e8400-e29b-41d4-a716-446655440002', 'Transporte', 'gasto', 150.00, true),
    ('770e8400-e29b-41d4-a716-446655440003', 'Ocio', 'gasto', 100.00, true),
    ('770e8400-e29b-41d4-a716-446655440004', 'Reparaciones Coche', 'gasto_acumulativo', 50.00, true),
    ('770e8400-e29b-41d4-a716-446655440005', 'Ropa', 'gasto_mixto', 75.00, true);
```