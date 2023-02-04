# Creación de un par de claves de usuario en Linux (Ubuntu)
Creación de un par de claves de usuario como método de autenticación en el protocolo SSH.


## Paso 1 - Creación del par de claves
```bash
# almacenaremos la clave privada en ~/.ssh
cd ~/.ssh
```
```bash 
# creamos una clave de 4096 bits utilizando el algoritmo rsa y utilizando nuestra direccion email como etiqueta 
ssh-keygen -t rsa -b 4096 -C "usuario@dominio"
```
El comando anterior nos solicitará el nombre del archivo. Por defecto, nos sugiere **~/.ssh/id_rsa**  
Al finalizar nos muestra el [hash](#nota-1-huella-digital-de-una-clave) de la clave generada y su visualización mediante una imagen ascii ([*random art*](#nota-2-ramdom-art)).

**Ejemplo:** 
```
ssh-keygen -t rsa -b 4096 -C "usuario@ejemplo.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/carlos/.ssh/id_rsa): ejemplo
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in ejemplo
Your public key has been saved in ejemplo.pub
The key fingerprint is:
SHA256:6x9B5ufHIyPwx1Y7ldpRi3albamSKEvNMBNzNPkPhAs usuario@ejemplo.com
The key's randomart image is:
+---[RSA 4096]----+
|         oo      |
|       E.o..     |
|       o..*     o|
|        += o  .o*|
|       +S o +oo*+|
|        *+.=o+=+.|
|       o.++oBo*. |
|      ..o  =.+ o |
|       ....      |
+----[SHA256]-----+
```


## Paso 2 - Copiar la clave pública en el servidor

La forma más rápida de copiar la clave pública en el host es usar la utilidad `ssh-copy-id`. 
Debido a su simplicidad, este método es muy recomendable si está disponible.   
Si ssh-copy-id no está disponible en la máquina local (cliente), se puede usar uno de los dos métodos alternativos indicadosa continuación.

### Método principal: copiar la clave utilizando ssh-copy-id
Requisito: para que este método funcione, se debe tener acceso SSH basado en contraseña el su servidor.

La sintaxis es:
```
ssh-copy-id -i <clave>.pub usuario@host 
```

Ejemplo:

```
ssh-copy-id -i ejemplo.pub ubuntu@213.0.121.1  
```

El resultado será un mensaje como el siguiente:

>The authenticity of host '213.0.121.1 (213.0.121.1)' can't be established.  
>RSA key fingerprint is 6x9B5ufHIyPwx1Y7ldpRi3albamSKEvNMBNzNPkPhAs. 
>Are you sure you want to continue connecting (yes/no)? yes

Esto significa que tu sistema local no reconoce el host remoto. Esto sucederá la primera vez que te conectes a un nuevo host. Escribe "sí" y pulsa `ENTER` para continuar.

A continuación, la utilidad ssh-copy-id buscará el archivo local <clave>.pub creado anteriormente y te pedirá la contraseña de la cuenta del usuario remoto:


>/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
>/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
>ubuntu@213.0.121.1's password:

Tras escribir la contraseña y pulsar `ENTER` se copiará la clave pública en el servidor en el archivo `~/.ssh/authorized_keys`.
El mensaje obtenido será similar al siguiente:
>Number of key(s) added: 1
>Now try logging into the machine, with:   "ssh 'ubuntu@213.0.121.1'"
>and check to make sure that only the key(s) you wanted were added.
  
  

### Nota 1: huella digital de una clave
La huella digital o *fingerprint* de un una clave pública se utiliza para verificar la identidad del *host* al que nos conectamos.
Del mismo modo, el *host* puede verificar nuestra identidad mediante la huella de nuestra clave pública.

La longitud de una huella digital depende del algoritmo criptográfico utilizado. 
En el caso de SHA256 la huella tiene una longitud de 256 bits y para visualizarla se representa mediante una cadena Base64:
En el ejemplo anterior, la huella es 
>6x9B5ufHIyPwx1Y7ldpRi3albamSKEvNMBNzNPkPhAs 

Podemos obtener la huella digital de una clave ssh mediante el siguiente comando:
```
ssh-keygen -lf <nombre_archivo_clave>
```
Ejemplo
```
ssh-keygen -lf id_rsa
4096 SHA256:6x9B5ufHIyPwx1Y7ldpRi3albamSKEvNMBNzNPkPhAs usuario@ejemplo.com (RSA)
```

### Nota 2: *random art*

El *random art* de una huella digital es una forma de visualizar dicho *hash* para poder reconocer fácilmente y de forma visual una huella. 
Podemos visualizar el *random art* de la huella digital de una clave ssh añadiendo la opción -v al comando ssh-keygen:

```
ssh-keygen -lvf <nombre_archivo_clave>
```
Ejemplo:

```
% ssh-keygen -lvf ejemplo
4096 SHA256:6x9B5ufHIyPwx1Y7ldpRi3albamSKEvNMBNzNPkPhAs usuario@ejemplo.com (RSA)
+---[RSA 4096]----+
|         oo      |
|       E.o..     |
|       o..*     o|
|        += o  .o*|
|       +S o +oo*+|
|        *+.=o+=+.|
|       o.++oBo*. |
|      ..o  =.+ o |
|       ....      |
+----[SHA256]-----+

```

Normalmente es usado para visualizar las claves del *host* al que nos conectamos. En este caso, debemos configurar nuestro cliente SSHS añadiendo al fichero local **/etc/ssh/ssh_config** la opción **VisualHostKey**:

>VisualHostKey yes

### Recomendaciones de seguridad
1. Protege la clave privada con una **contraseña segura**, de un mínimo de 12 caracteres que contenga mayúsculas, minúsculas, números y símbolos.
2. No uses claves SSH para autenticarte como root, sino para cuentas de usuario.
3. Deshabilita la autenticación con contraseñas en el servidor. La autenticación mediante un par de claves es mucho más segura.
4. Limita el acceso a la clave pública en el servidor. El archivo ~/.ssh/authorized_keys solo debe tener permiso de R+W para la cuenta propietaria
5. Actualiza las claves periódicamente, al menos una vez al año. Al añadirla a authorized_keys y, tras verificar su funcionamiento, recuerda eliminar las claves antiguas.
6. Utiliza ssh-agent o un **gestor de claves** que facilite la administración de todas tus claves
7. Considera la posibilidad de utilizar una **llave hardware** tipo YubiKey para almacenar tus claves privadas y evitar almacenarlas en tu ordenador.
