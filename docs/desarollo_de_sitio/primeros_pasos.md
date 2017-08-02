# Descripción general

ZeroNet le permite publicar sitios estáticos y dinámicos.

Aunque ZeroNet no puede ejecutar lenguajes de scripting como PHP o Ruby, puede crear sitios dinámicos usando la API de ZeroNet (llamada ZeroFrame), JavaScript (o CoffeeScript) y la base de datos SQL incorporada.

## Tutorial de ZeroChat

En este tutorial vamos a construir un sitio de chat P2P, descentralizado, servidor y sin backend en menos de 100 líneas de código.

* [Leer en ZeroBlog] (http://127.0.0.1:43110/Blog.ZeroNetwork.bit/?Post:99:ZeroChat+tutorial)
* [Leer en Medium.com] (https://decentralize.today/decentralized-p2p-chat-in-100-lines-of-code-d6e496034cd4)

## Modo de depuración ZeroNet

ZeroNet viene con un indicador `--debug` que facilitará el desarrollo del sitio.

Para ejecutar ZeroNet en modo de depuración use: `python zeronet.py --debug`

### Características del modo de depuración:

- CoffeeScript automático -> conversión de JavaScript (Todos los ejemplos utilizados en esta documentación y sitios de muestra se escriben en [CoffeeScript] (http://coffeescript.org/))
- Los mensajes de depuración aparecerán en la consola
- Recarga automática de algunos archivos de origen (UiRequest, UiWebsocket, FileRequest) en la modificación para evitar reiniciar (Requiere [PyFilesystem] (http://pyfilesystem.org/) en GNU / Linux)
- `http: //127.0.0.1: 43110 / Debug` Traceback y la consola interactiva de Python en la última posición de error (usando el maravilloso depurador Werkzeug - Requiere [Werkzeug] (http://werkzeug.pocoo.org/))
- `http: //127.0.0.1: 43110 / Console` genera una consola interactiva de Python (Requiere [Werkzeug] (http://werkzeug.pocoo.org/))

### Funciones adicionales (sólo funciona para los sitios de su propiedad)

  - Archivos CSS fusionados: Todos los archivos CSS dentro de la carpeta del sitio se fusionarán en un archivo denominado `all.css`. Puede elegir incluir sólo este archivo en su sitio. Si desea mantener los otros archivos CSS para facilitar el desarrollo, puede agregarlos a la clave de ignorar de su `content.json`. De esta forma, no se publicarán con su sitio. (Js | css) / (?! all. (Js | css)) `` esto ignorará todos los archivos CSS y JS excepto `all.js` Y `all.css`)
  - Archivos JS fusionados: Todos los archivos JS dentro de la carpeta del sitio se fusionarán en un archivo llamado `all.js`. Si hay un compilador CoffeeScript (incluido en Windows), se convertirá `.coffee` a` .js`.
  - Orden en el que los archivos se fusionan en all.css / all.js: Los archivos dentro de los subdirectorios de la carpeta css / js aparecen primero; Los archivos de la carpeta css / js se fusionarán de acuerdo con el orden de nombre de archivo (01_a.css, 02_a.css, etc.)

