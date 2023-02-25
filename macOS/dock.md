## Crear un separador en el Dock de macOS
1. Abrir terminal
2. Ejecutar el siguiente código
```
write com.apple.dock persistent-apps -array-add '{"tile-type"="spacer-tile";}'; killall Dock
```
3. Arrastar el separador para colocarlo entre las apps que queramos

## Eliminar un separador en el Dock de macOS
Botón derecho > Eliminar del Dock

## Reestablecer la configuración del Dock a los ajustes por defecto
```
defaults delete com.apple.dock; killall Dock
```
