# Crear un sitio en ZeroNet

## La forma fácil: Usando la interfaz web

 * Haz clic en **⋮** > **"Create new, empty site"** en el menú del sitio ZeroHello.
 * Seras **redireccionado** a un sitio completamente nueva que es solo modificable por ti
 * Puedes encontrar y modificar el contenido de tu sitio en el directorio **data/[ladireccióndetusitio]**
 * Luego de las modificaciones abre tu sitio, arrastra el botón "0" en la parte superior derecha hacia la izquierda, luego presiona en los botones **firmar** y **publicar** en la parte baja del panel

## Forma manual: Usando la linea de comandos

### 1. Crear la estructura del sitio

* Cierra Zeronet si esta corriendo
* Busca en el directorio donde ZeroNet esta instalado y corre:


```bash
$ zeronet.py siteCreate
...
- Site private key: 23DKQpzxhbVBrAtvLEc2uvk7DZweh4qL3fn3jpM3LgHDczMK2TtYUq
- Site address: 13DNDkMUExRf9Xa9ogwPKqp7zyHFEqbhC2
...
- Site created!
$ zeronet.py
...
```

- Esto creara los archivos iniciales en el interior de tu sitio ```data/13DNDkMUExRf9Xa9ogwPKqp7zyHFEqbhC2```.

> __Nota:__
> Los usuarios de Windows usando la versión empaquetada deben buscar en la carpeta ZeroBundle/ZeroNet y correr `"../Python/python.exe" zeronet.py siteCreate`

### 2. Construir/Modificar el sitio

* Actualiza los archivos del sitio localizados en ```data/[ladireccióndetusitio]``` (ej.: 13DNDkMUExRf9Xa9ogwPKqp7zyHFEqbhC2).
* Cuando tu sitio este listo corre:

```bash
$ zeronet.py siteSign 13DNDkMUExRf9Xa9ogwPKqp7zyHFEqbhC2
- Signing site: 13DNDkMUExRf9Xa9ogwPKqp7zyHFEqbhC2...
Private key (input hidden):
```
* Entra la llave privada que obtuviste cuando creaste el sitio. Esto firmara todos los archivos de manera que los pares puedan verificar que el dueño del sitio es quien hizo los cambios.

### 3. Publicar los cambios del sitio

* En orden de informar a los pares acerca de los cambios que hiciste necesitas correr:


```bash
$ zeronet.py sitePublish 13DNDkMUExRf9Xa9ogwPKqp7zyHFEqbhC2
...
Site:13DNDk..bhC2 Publishing to 3/10 peers...
Site:13DNDk..bhC2 Successfuly published to 3 peers
- Serving files....
```

* ¡Eso es todo!, lograste exitosamente firmar y publicar tus modificaciones.
* Tu sitio sera accesible desde : ```http://localhost:43110/13DNDkMUExRf9Xa9ogwPKqp7zyHFEqbhC2```

**Siguientes pasos:** [Documentación de Desarrolladores de ZeroNet](/desarollo_de_sitio/primeros_pasos/)
