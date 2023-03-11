## Identificar las interfaces Ethernet
Muestra la dirección IP de cada interfaz:
```bash
ip a
```


## Obtener la IP pública de nuestro host

Hace consulta a un servidor externo como api.ipify.org:

```bash
echo $(wget -qO - https://api.ipify.org)
```


### Documentación de Ubuntu
Consulta toda la documentación de Ubuntu

[https://ubuntu.com/server/docs/network-configuration](https://ubuntu.com/server/docs/network-configuration)
