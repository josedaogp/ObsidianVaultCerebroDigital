## Pasos de despliegue (DDNS + Nginx + PM2)

1. Crear carpeta apps y entrar: `sudo mkdir -p /opt/apps && sudo chown $USER:$USER /opt/apps && cd /opt/apps`
    
2. Clonar repo: `git clone <URL_DEL_REPO.git> mi-app && cd mi-app`
    
3. (Opcional) Crear `.env.production` con tus variables (ej. `NEXT_PUBLIC_SUPABASE_URL=...`).
    
4. Instalar dependencias: `npm ci || npm install`
    
5. Compilar producción: `npm run build`
    
6. Probar local (puerto 3001): `npx next start -p 3001` (Ctrl+C para salir)
    
7. Instalar PM2: `sudo npm i -g pm2`
    
8. Levantar en 3001 con PM2: `pm2 start "next start -p 3001" --name mi-app --update-env`
    
9. Guardar estado PM2: `pm2 save && pm2 status`
    
10. UFW mínimo: `sudo ufw allow OpenSSH && sudo ufw allow 'Nginx Full'`
    
11. Crear host Nginx:
    

`sudo tee /etc/nginx/sites-available/mi-app <<'EOF' server {   listen 80;   server_name miapp.tu-ddns.net;    location / {     proxy_pass http://127.0.0.1:3001;     proxy_http_version 1.1;     proxy_set_header Host $host;     proxy_set_header X-Real-IP $remote_addr;     proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;     proxy_set_header X-Forwarded-Proto $scheme;     proxy_set_header Upgrade $http_upgrade;     proxy_set_header Connection "upgrade";   } } EOF`

12. Activar sitio y recargar Nginx:  
    `sudo ln -s /etc/nginx/sites-available/mi-app /etc/nginx/sites-enabled/ && sudo nginx -t && sudo systemctl reload nginx`
    
13. DDNS: apunta `miapp.tu-ddns.net` a tu IP pública (en tu proveedor DDNS).
    
14. Router: Port Forwarding **80/443 → IP LAN del servidor**.
    
15. (HTTPS) Certificado: `sudo apt install -y certbot python3-certbot-nginx && sudo certbot --nginx -d miapp.tu-ddns.net`
    

> Deploys posteriores: `git pull && npm ci||npm i && npm run build && pm2 restart mi-app && pm2 save`

## Explicación breve de parámetros Nginx

- `listen 80;` puerto público HTTP (443 para HTTPS).
    
- `server_name miapp.tu-ddns.net;` dominio/DDNS que atenderá este bloque.
    
- `location / { ... }` reglas por ruta (aquí todo el tráfico raíz).
    
- `proxy_pass http://127.0.0.1:3001;` a qué servicio interno se reenvía.
    
- `proxy_http_version 1.1;` necesario para WebSockets y keep-alive modernos.
    
- `proxy_set_header Host $host;` conserva el host original (importante para Next).
    
- `X-Real-IP`, `X-Forwarded-*` preservan IP y esquema del cliente.
    
- `Upgrade` / `Connection "upgrade"` habilita WebSockets (útil en tiempo real).

## ANOTACIONES IMPORTANTES
Si queremos que la app esté en el location/finanzas/ por ejemplo, es importante poner el fichero de configuración tal que así:
```
  GNU nano 7.2                                                  finanzas
server {
  listen 80;
  server_name jdogserver.ddns.net;

  location /finanzas/ {
    proxy_pass http://127.0.0.1:3001;
    proxy_http_version 1.1;

    proxy_set_header Host $host;
    proxy_set_header X-Forwarded-Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header X-Forwarded-Prefix /finanzas;

    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";

    proxy_redirect off;
  }
}

```
Además, en el proyecto clonado de git de React (/opt/apps/proyecto), en el fichero *next.config.mjs*, hay que añadir que su basePath es /finanzas y otro parámetro. Habría que añadir estas dos líneas:
```mjs
  basePath: '/finanzas',
  trailingSlash: true,
```
Quedaría algo así:
```mjs
/** @type {import('next').NextConfig} */
const nextConfig = {
  eslint: {
    ignoreDuringBuilds: true,
  },
  typescript: {
    ignoreBuildErrors: true,
  },
  images: {
    unoptimized: true,
  },
  basePath: '/finanzas',
  trailingSlash: true,
}

export default nextConfig

```

Es importante el trailingSlash porque si no redirecciona en bucle.

## Comandos más usados
**Cuando cambias la configuración del nginx (/etc/nginx/sites-availables)**
```cmd
sudo nginx -t && sudo systemctl reload nginx
```

**Cuando cambias algo del repo React** (/opt/apps/finanzasJosedaV0)
```cmd
npm run build
pm2 restart finanzas
```
