## script bash
### Leer un número entero por teclado y devolver un mensaje si es positivo

```bash
#!/bin/bash
 echo "Introduce un número"
 read n
 if [ $n -ge "0" ]; then
    echo "El número $n es positivo"
 else
    echo "El número $n es negativo"
 fi

```
