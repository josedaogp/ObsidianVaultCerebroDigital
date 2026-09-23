## Descripción del vídeo

Para tener funcional n8n tenemos 3 maneras

- Abrirnos una cuenta de n8n oficial [aquí](https://app.n8n.cloud/register): Tendremos que pagar por uso y por desbloquear ciertas características. Pero a la hora de tener un uso muy intensivo de la plataforma nos puede rentar
- Montar un contenedor de Docker en nuestro ordenador: Requiere de una maquina disponible, configurar proxies revertidos y conocimiento mas técnico.
- Instalar en Railway una plantilla ya hecha: Es importante que uséis [N8N (w/ workers)](https://railway.app/template/r2SNX_) ya que tiene la última versión y por tanto las implementaciones con servicios como OpenAI están actualizadas.

En la instalación es necesario que para que otros servicios como Telegram, puedan acceder a nuestro flujo de trabajo (workflow), establezcamos una variable de entorno en el contenedor Primary de Railway.

**WEBHOOK_URL**
```
https://primary-production-9b88.up.railway.app
```

[![](https://media1-production-mightynetworks.imgix.net/asset/8ff5de2e-f57a-4d4c-8320-030c337a2a77/1718980408236.png?ixlib=rails-4.2.0&fm=jpg&q=75&auto=format&w=1400&h=1400&fit=max&impolicy=ResizeCrop&constraint=downsize&aspect=fit)](https://media1-production-mightynetworks.imgix.net/asset/8ff5de2e-f57a-4d4c-8320-030c337a2a77/1718980408236.png?ixlib=rails-4.2.0&fm=jpg&q=75&auto=format)  
 el valor de esa variable es la URL pública que usamos para acceder a nuestro panel de n8n.

---

Transcripción del vídeo:

[instalacion.zip](https://media2-production.mightynetworks.com/asset/f91fd549-8ca8-4eda-a657-9ca7e0b1f96f/instalacion.zip "instalacion.zip")

## Mis notas
Ha explicado las tres vías de instalación de n8n:
- Con el propio hosting de n8n (hay que pagar)
- En local con docker (habría que hacer un proxy y demás para que las peticiones de terceros se puedan conectar con nuestro contenedor de docker)
- Railway. En este caso tendremos que pagar una pequeña mensualidad. Habrá que cambiar también una variable de entorno (la que aparece arriba de webhook_url) para que lleguen las peticiones.