# Contribuyendo a ZeroNet

Gracias por usar ZeroNet. ZeroNet es un esfuerzo colaborativo de 67 entusiastas de la descentralización como tú. Agradecemos a todos los usuarios que detecten errores, mejoren la documentación y tengan buenas ideas para diseñar nuevos protocolos. Estas son algunas pautas que le pedimos que siga para empezar a hacer su contribución.

### No tiene que aportar al código fuente

De hecho, la mayoría de los contribuyentes no envían al código fuente. Incluso si te gusta escribir programas, otros tipos de contribución también son bienvenidos.

### ¿Te gusta escribir?

- Escribe sobre ZeroNet.
- Escribe tutoriales para ayudar a las personas a configurar las cosas.
- Ayuda a traducir ZeroNet.
- Mejorar esta documentación. Esta documentación ha sido escrita por muchos miembros de la comunidad en todo el mundo.

### ¿Te gusta ayudar a la gente?

- Suscríbete a nuestro [rastreador de problemas en GitHub](https://github.com/HelloZeroNet/ZeroNet/issues) y ayuda a la gente a resolver problemas.
- Únete en [Gitter](https://gitter.im/HelloZeroNet/ZeroNet) el canal IRC [#zeronet @ freenode](https://kiwiirc.com/client/irc.freenode.net/zeronet) y ayuda a responder las preguntas.
- Configurar una caja de semillas y ayudar a hacer la red más rápida.

### ¿Te gusta hacer paginas web?

- Crea nuevos sitios en ZeroNet. Ve adelante y haz tu propio blog en ZeroNet. [Es fácil y gracias, no cuesta mucho esfuerzo.](../usando_zeronet/crear_nuevo_sitio.md)
- “¡El contenido es lo que importa!” como NoFish dice. La red no vale nada sin contenido, así que necesitamos que lo hagas para tener éxito.

### ¿Te gusta investigar?

- Ayúdanos a investigar nuestros [problemas graves](https://github.com/HelloZeroNet/ZeroNet/labels/help%20wanted).
- Únase a nuestra discusión sobre el diseño de nuevas características y protocolos, como el [soporte I2P](https://github.com/HelloZeroNet/ZeroNet/issues/45) y [protocolo DHT](https://github.com/HelloZeroNet/ZeroNet/issues/57).
- Haz tu propio [Raspberry Pi](https://github.com/HelloZeroNet/ZeroNet#linux-terminal), un [C.H.I.P.](http://127.0.0.1:43110/Blog.ZeroNetwork.bit/?Post:94:Running+ZeroNet+on+a+$9%C2%A0computer) o un [router abierto](https://github.com/HelloZeroNet/ZeroNet/issues/783)? Intente ejecutar ZeroNet en él y díganos cómo funciona ZeroNet en su dispositivo.

### ¿Te gusta escribir códigos?

- Si sabes Python, puedes elegir una tarea de nuestro [rastreador de problemas en GitHub](https://github.com/HelloZeroNet/ZeroNet/issues).
- También son bienvenidos a desarrollar sus propias ideas. Antes de empezar por favor [abre una nueva discusión](https://github.com/HelloZeroNet/ZeroNet/issues/new) para dejar saber a la comunidad, por lo que puede asegurarse de que podemos compartir nuestras ideas para hacer lo mejor de estas.
- Mantenga su estilo de codificación coherente. Le pedimos que siga nuestra convención de codificación a continuación.

### ¿Te gustaría ofrecer apoyo financiero?

- Puedes [donar bitcoins](donar.md) para apoyar ZeroNet.


## Convenciones de codificación

 - Seguir [PEP8](https://www.python.org/dev/peps/pep-0008/)
 - Simple es mejor que complejo
 - Optimización prematura es la raíz de todo el mal

### Nombrado

 - ClassNames: Capitalizado, CamelCased
 - functionNames: empieza con minuscula, camelCased
 - variable_names: minusculas, under_scored

### Variables

 - file_path: Dirección de archivo relativo al directorio de trabajo (data/17ib6teRqdVgjB698T4cD1zDXKgPqpkrMg/css/all.css)
 - inner_path: Archivo relativo a la dirección del sitio (css/all.css)
 - file_name: all.css
 - file: Objeto de archivo Python
 - privatekey: Llave privada para el sitio (sin _)

### Directorios de archivos fuente y nombrado

 - Una clase por archivo es preferido
 - El nombre del directorio y archivo viene del ClassName: WorkerManager class = Worker/WorkerManager.py
