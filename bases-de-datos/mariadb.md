# MariaDB 

## Instalar MariaDB en Ubuntu 20.04 LTS
```bash
# actualizar repositorios
sudo apt update && sudo upgrade -y

# instalar servidor mariadb
sudo apt install mariadb-server

# asegurar la instalacion
# la contraseña predeterminada de Mariadb está en blanco.
sudo mysql_secure_installation
```

## Permitir acceso a la BBDD desde cualquier host
Editar `/etc/mysql/mariadb.conf.d/50-server.cnf` y modificar `bind-address` a:
```
...

bind-address = 0.0.0.0
# Ojo: se recomienda no dejarlo abierto a cualquier IP sino únicamente a la subred del host cliente

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
