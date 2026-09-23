Nombre: JDOG
Server name: jdogserver
nombre usuario: jdogs
contraseña: lucyortizcanina89
constarseña bbswitch secure boot: lucyortizcanina

**User y pass ROUTER DIGI**
User: user
Pass: userDigi1!
Si no, probar user user.

## Pila que usa:
CR1220W 3.0V, con dos cables. Le cambié la pila el 10-3-2026, y le puse con cinta aislante los cables.
## Comandos básicos
### WIFI
- Añadir una nueva wifi:
```ssh
cd /etc/netplan
sudo nano 50-cloud-init.yaml
```
Ahora, a nivel derecha de access-points (donde ya debe haber alguno), poner:
"NOMBRE-RED"
	password: "contraseña"
Guardar y luego:
```ssh
sudo netplan apply
```
Después ip a para ver si se ha conectado ya.
### Discos
- Listar los discos y sus unidades
```ssh
lsblk -f
```

### Powertop (Optimizador energía)
- Optimización automática:
```ssh
sudo powertop --auto-tune
```

Para que se ejecutara siempre, creé un servicio así:
```ssh
sudo nano /etc/systemd/system/powertop.service
```
Y lo activé así:
```ssh
sudo systemctl enable powertop.service
```

### SSL
**Cómo activar SSL**
sudo certbot --nginx -d jdogserver.ddns.net
**Para que se renueve solo:**
sudo certbot renew --dry-run


### Gráfica
NOTA: FINALMENTE DEJÉ LA GRÁFICA ACTIVA (AUNQUE SIN USO EN PRINCIPIO) PORQUE NO ARRANCABA.
- Revisar si está activada la gráfica NVIDIA o no. Se hace con bbswitch, y previamente ha habido que poner el driver nouveau que controlaba la gráfica en la blacklist para que el kernel lo ignorase. Ahora, al meter este comando, debería aparecer un OFF diciendo que está desactivada.
```ssh
cat /proc/acpi/bbswitch
```
#### Pasos para volver a habilitar la NVIDIA

1. **Quitar el blacklist de `nouveau`**
    
    `sudo rm /etc/modprobe.d/blacklist-nouveau.conf sudo update-initramfs -u`
    
2. **(Opcional) Instalar drivers propietarios NVIDIA**
    
    - Ver qué recomiendan:
        
        `ubuntu-drivers devices`
        
    - Instalar automáticamente:
        
        `sudo ubuntu-drivers autoinstall`
        
3. **Desactivar bbswitch si molesta**
    
    - Borra su configuración:
        
        `sudo rm /etc/modprobe.d/bbswitch.conf`
        
    - Quita el módulo de arranque:
        
        `sudo sed -i '/bbswitch/d' /etc/modules`
        
4. **Reinicia**
    
    `sudo reboot`
    
5. **Verifica que está activa**
    
    `lspci | grep -i nvidia lsmod | grep nvidia`
    