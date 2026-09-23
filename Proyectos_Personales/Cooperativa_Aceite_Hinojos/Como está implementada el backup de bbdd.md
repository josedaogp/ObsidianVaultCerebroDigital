Lanza este comando:
```
docker exec -e PGPASSWORD=cooperativaAHpass cooperativa_aceite_postgres_db pg_dump -U cooperativaAH -d cooperativaHinojosBBDD -F c -f /tmp/backup_test.sql


```
Y lo genera aquí (comprobar con este comando):
```
docker exec -it cooperativa_aceite_postgres_db ls /tmp/backup_test.sql

```
Está incluido con el rotado de logs, y su configuración está en services/config.py