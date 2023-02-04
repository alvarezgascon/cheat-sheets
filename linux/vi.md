# Editor de texto vi / vim


### Ir a una línea 
1. Pulsar la tecla `ESC` para entrar al modo comando
2. Escribir `:` y en el prompt escribir el número de línea

Ejemplo para ir a la línea 35:
>:35

### Mostrar número de línea
1. Pulsar la tecla `ESC` para entrar al modo comando
2. Escribir `:` y en el prompt escribir el comando siguiente: `set number`
3. Para desactivar los números de línea, escribir el comando `set nonumber`

Si queremos que por defecto `vi` tenga activada la visualización del número de línea, editaremos el archivo ~/.vimrc:
```bash
vi ~/.vimrc
```
Añadimos la línea:
>set number



### Abrir un archivo directamente en una linea en particular

Usaremos el comando `vi +numerodelinea archivo` indicando el número de línea con la opción +

Ejemplo:
```bash
vi +37 archivo.txt
```

### Abrir un archivo en la primera línea que contenga una cadena de texto en particular

Usaremos el comando `vi +/cadena_a_buscar archivo` indicando el número de línea con la opción +/

Ejemplo:
```bash
vi +/error /var/log/syslog
```

### Mostrar la configuración actual de vi
En el prompt `:` escribe lo siguiente:
```
set all
```
Para ver una lista de todo lo que se ha configurado en el archivo de configuración o tiempo de ejecución de vim
escribe el siguiente comando en el símbolo del sistema `:`
```
set
```
