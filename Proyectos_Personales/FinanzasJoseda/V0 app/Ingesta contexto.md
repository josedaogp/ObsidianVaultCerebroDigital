A):

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

B) Hay autenticación. Para ello utilizo la propia autenticación de supabase, y tengo esto:

"use client"

import { useState } from "react"

import { useSession, useSupabaseClient } from "@supabase/auth-helpers-react"

export function AuthWidget() {

const supabase = useSupabaseClient()

const [mode, setMode] = useState<"login" | "register" | "forgot">("login")

const [email, setEmail] = useState("")

const [password, setPassword] = useState("")

const [remember, setRemember] = useState(false)

const [loading, setLoading] = useState(false)

const [error, setError] = useState<string | null>(null)

const [success, setSuccess] = useState<string | null>(null)

const resetStates = () => {

setError(null)

setSuccess(null)

setPassword("")

}

const handleLogin = async (e: React.FormEvent) => {

e.preventDefault()

setLoading(true)

setError(null)

setSuccess(null)

const { error } = await supabase.auth.signInWithPassword({ email, password })

if (error) setError("Correo o contraseña incorrectos, o cuenta no confirmada.")

setLoading(false)

}

const handleRegister = async (e: React.FormEvent) => {

e.preventDefault()

setLoading(true)

setError(null)

setSuccess(null)

const { error } = await supabase.auth.signUp({ email, password })

if (error) setError(error.message)

else setSuccess("Revisa tu correo para confirmar tu cuenta.")

setLoading(false)

}

const handleForgot = async (e: React.FormEvent) => {

e.preventDefault()

setLoading(true)

setError(null)

setSuccess(null)

const { error } = await supabase.auth.resetPasswordForEmail(email, {

redirectTo: `${window.location.origin}/reset-password`

})

if (error) setError(error.message)

else setSuccess("Te hemos enviado instrucciones para restaurar la contraseña.")

setLoading(false)

}

// Logo chulo y simple tipo BrightID

const Logo = () => (

<div className="flex items-center gap-2 mb-6">

<svg width="40" height="40" viewBox="0 0 48 48" fill="none">

<rect x="6" y="6" width="36" height="36" rx="10" fill="#2576fd"/>

<rect x="18" y="18" width="12" height="12" rx="3" fill="#fff"/>

</svg>

<span className="text-2xl font-bold text-white select-none">

<span className="text-[#2576fd]">Finanzas</span>Joseda

</span>

</div>

)

return (

<div className="w-full max-w-[400px] bg-[#202939] rounded-2xl shadow-xl px-8 py-10 flex flex-col items-center border border-[#22334a]">

<Logo />

<h1 className="text-xl font-bold mb-4 text-white">

{mode === "login"

? "Iniciar sesión"

: mode === "register"

? "Crear cuenta"

: "¿Olvidaste tu contraseña?"}

</h1>

<form className="w-full flex flex-col gap-4" onSubmit={

mode === "login"

? handleLogin

: mode === "register"

? handleRegister

: handleForgot

}>

<div>

<label className="block text-white text-sm mb-1">Email</label>

<input

type="email"

className="w-full rounded-md px-3 py-2 bg-[#181f2c] text-white border border-[#2a3650] focus:ring-2 focus:ring-[#2576fd] focus:outline-none"

placeholder="you@email.com"

autoComplete="email"

value={email}

onChange={e => setEmail(e.target.value)}

required

autoFocus

/>

</div>

{mode !== "forgot" && (

<div>

<label className="block text-white text-sm mb-1">Contraseña</label>

<input

type="password"

className="w-full rounded-md px-3 py-2 bg-[#181f2c] text-white border border-[#2a3650] focus:ring-2 focus:ring-[#2576fd] focus:outline-none"

placeholder="********"

value={password}

onChange={e => setPassword(e.target.value)}

required={mode !== "forgot"}

/>

</div>

)}

{error && <div className="text-red-400 text-sm">{error}</div>}

{success && <div className="text-green-400 text-sm">{success}</div>}

<button

className="bg-[#2576fd] hover:bg-[#296bdf] transition-colors text-white py-2 rounded-md font-bold text-lg mt-2"

type="submit"

disabled={loading}

>

{loading

? "Procesando..."

: mode === "login"

? "Iniciar sesión"

: mode === "register"

? "Crear cuenta"

: "Enviar email"

}

</button>

</form>

{/* Links para cambiar de modo */}

{mode === "login" && (

<div className="w-full mt-4 flex flex-col items-start gap-2 text-sm">

<button

onClick={() => { setMode("forgot"); resetStates() }}

className="text-[#2576fd] hover:underline transition"

>

¿Olvidaste tu contraseña?

</button>

<span>

¿No tienes cuenta?{" "}

<button

onClick={() => { setMode("register"); resetStates() }}

className="text-[#2576fd] hover:underline font-medium"

>

Regístrate

</button>

</span>

</div>

)}

{mode === "register" && (

<div className="w-full mt-4 flex flex-col items-start gap-2 text-sm">

<span>

¿Ya tienes cuenta?{" "}

<button

onClick={() => { setMode("login"); resetStates() }}

className="text-[#2576fd] hover:underline font-medium"

>

Inicia sesión

</button>

</span>

</div>

)}

{mode === "forgot" && (

<div className="w-full mt-4 flex flex-col items-start gap-2 text-sm">

<button

onClick={() => { setMode("login"); resetStates() }}

className="text-[#2576fd] hover:underline"

>

Volver a iniciar sesión

</button>

</div>

)}

<div className="mt-6 w-full text-xs text-gray-500 flex justify-between items-center">

<a href="/privacy-policy" className="hover:underline">Política de privacidad</a>

</div>

</div>

)

}

Y esto:

// /hooks/useRequireAuth.ts

import { useSession } from "@supabase/auth-helpers-react"

import { useRouter } from "next/navigation"

import { useEffect } from "react"

export function useRequireAuth() {

const session = useSession()

const router = useRouter()

useEffect(() => {

if (session === null) {

router.replace("/login")

}

}, [session, router])

if (session === undefined) return null

if (session === null) return null

return session // <-- esto es la sesión con el user, si todo está bien

}

Que lo uso siempre en una página como la que vamos a diseñar así:

export default function CategoriasPage() {

const session = useRequireAuth()

if (!session) return null

C) Tengo servicios. Te pongo un ejemplo:

import { supabase } from "@/lib/supabaseClient"

import { EnrichedCategory } from "@/types/models"

import { Category, CategoryWithType } from "@/types/category"

export async function getCategories(): Promise<EnrichedCategory[]> {

const { data, error } = await supabase

.from("categories")

.select("*, category_types(name)")

.order("name", { ascending: true })

if (error) {

console.error("Error fetching categories", error)

return []

}

return data as EnrichedCategory[]

}

Pero tendremos que crear nuestros propios servicios por separado.

D)

Ya tengo datos en Supabase, eso no es un problema por ahora.

E) La lógica de negocio es exactamente igual, solo cambia la persistencia.

F) React next.js, mismo kit.

Toda la configuración de supabase ya está hecha, de hecho tengo otras pantallas con CRUD básico implementadas. No hay ninguna lógica de servidor, todo en frontal