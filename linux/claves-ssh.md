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
Al finalizar nos muestra el hash de la clave generada y su visualización mediante una imagen ascii (*random art*).

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

### Nota 2: *ramdom art*

El *random art* de una huella digital es una forma de visualizar dicho *hash* para poder reconocer fácilmente y de forma visual una huella. 
Podemos visualizar el *random art* de la huella digital de una clave ssh añadiendo la opción -v al comando ssh-keygen:

```
ssh-keygen -lvf <nombre_archivo_clave>
```
Ejemplo
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
