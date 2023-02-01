# MariaDB 

## Instalar MariaDB en Ubuntu 20.04 LTS
```bash
sudo apt update
sudo apt install mariadb-server
sudo mysql_secure_installation
```

## Permitir acceso a la BBDD desde cualquier host
Editar `/etc/mysql/mariadb.conf.d/50-server.cnf` y modificar `bind-address` a:
```
...

bind-address = 0.0.0.0

...
```
## Crear un usuario con permisos de administración
1. Crear un nuevo usuario `useradmin` en el host `localhost` con una `contraseña`:
```mysql
CREATE USER 'useradmin'@'localhost' IDENTIFIED BY 'contraseña';
```

2. Otorgar todos los permisos al nuevo usuario
```mysql
GRANT ALL PRIVILEGES ON * . * TO 'useradmin'@'localhost';
``` 

3. Actualizar los permisos
```mysql
FLUSH PRIVILEGES;
```