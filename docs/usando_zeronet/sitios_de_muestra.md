# Sitios de muestra ZeroNet

## ZeroHello

La pagina principal de ZeroNet

 - Listas de todos los sitios añadidos: titulo, nro. de pares, fecha de modificación
 - Acciones del sitio: actualizar, pausar, resumir, borrar
 - Clona sitios para tener tu propio blog, foro, etc.
 - Si hay una nueva versión disponible actualiza ZeroNet con un clic

![ZeroHello](/img/zerohello.jpg)

Dirección: [1HeLLo4uzjaLetFx6NH3PMwFP3qbRbTf3D](http://127.0.0.1:43110/1HeLLo4uzjaLetFx6NH3PMwFP3qbRbTf3D)

[Código fuente](https://github.com/HelloZeroNet/ZeroHello)

---

## ZeroBoard

Simple demo de tablero de mensajes para la distribución de contenido dinámico

 - Avatares dinámicos generados por la auth_key del usuario
 - Actualizaciones de mensajes en tiempo real

¿Como funciona?

 - Envías tu mensaje al bot dueño de la llave privada del sitio
 - El bot modifica el archivo `message.json`, lo firmas usando la llave privada y publicas a otros pared
 - Si la modificación del sitio alcanza tu cliente el mensaje aparecerá en tu explorador

![ZeroBoard](/img/zeroboard.jpg)

Dirección: [1Gfey7wVXXg1rxk751TBTxLJwhddDNfcdp](http://127.0.0.1:43110/1Gfey7wVXXg1rxk751TBTxLJwhddDNfcdp)

[Código fuente](https://github.com/HelloZeroNet/ZeroBoard)


---

## ZeroBlog

Demo de blog auto publicáble

 - Editor de contenido en linea
 - Sintaxis de Markdown
 - Sintaxis de código resaltados
 - firmado de sitio y publicado usando solo la interfaz web

¿Como funciona?

 - Puedes editar el archivo `data.json` usando la interfaz web
 - Presionando el botón `Firmar y Publicar contenido nuevo contenido` te pregunta por la llave privada del sitio (la cual se muestra cuando tu [creas un nuevo sitio usando el comando zeronet.py siteCreate](create_new_site/))
 - Tu cliente ZeroNet firmara los nuevos archivos modificados y lo publicara directamente a otros pares
 - Tu sitio sera accesible mientras al menos 1 computador del par (visitante) este activo

![ZeroBlog](/img/zeroblog.jpg)

Dirección: [1BLogC9LN4oPDcruNz3qo1ysa133E9AGg8](http://127.0.0.1:43110/1BLogC9LN4oPDcruNz3qo1ysa133E9AGg8) or [blog.zeronetwork.bit](http://127.0.0.1:43110/blog.zeronetwork.bit)

[Código fuente](https://github.com/HelloZeroNet/ZeroBlog)


---

## ZeroTalk

Demo de foro P2P descentralizado

 - Crear, modificar, borrar tópico y mensaje
 - Votar por tópico y mensaje
 - Solo tienes que contactar el dueño del sitio una ves, cuando se requiere para los permisos de modificación del sitio
 - Comentario y modificación de contenido enviado directamente a otros pares
 - Solo tu puedes firmar y modificar tus propios archivos
 - Muestra en tiempo real de nuevos comentarios.

¿Como funciona?

 - Para interactuar con el sitio tienes que requerir un certificado de registración (una firma criptografica) de un proovedor ZeroID
 - Luego de que tengas el certificado puedes publicar tu contenido (mensajes, topicos, votos de agrado) directamente a otros pares

![ZeroTalk](/img/zerotalk.jpg)

Dirección: [1TaLkFrMwvbNsooF4ioKAY9EuxTBTjipT](http://127.0.0.1:43110/1TaLkFrMwvbNsooF4ioKAY9EuxTBTjipT) or [talk.zeronetwork.bit](http://127.0.0.1:43110/talk.zeronetwork.bit)

[Código fuente](https://github.com/HelloZeroNet/ZeroTalk)

---

## ZeroMail

Sitio de mensajes P2P, distribuidos y encriptádo de par a par. Para mejorar la privacidad utiliza una solución como BitMessage y no expondrá el recipiente del mensaje.

 - Usando ECIES para intercambio secreto, AES256 para codificado del mensaje.
 - Cuando visitas por primera ves la pagina esta añadi tu llave publica a el archivo de tus datos y luego de eso cualquiera puede enviarte un mensaje
 - Todos tratan de desencriptár cada mensaje, esto mejora la privacidad haciendo imposible de encontrar el recipiente del mensaje
 - Para reducir la sobrecarga por mensaje e incrementar la velocidad de desencriptádo nosotros re usamos la llave AES, pero un nuevo IV es generado cada ves.



![ZeroTalk](/img/zeromail.jpg)

Dirección: [1MaiL5gfBM1cyb4a8e3iiL8L5gXmoAJu27](http://127.0.0.1:43110/1MaiL5gfBM1cyb4a8e3iiL8L5gXmoAJu27) or [mail.zeronetwork.bit](http://127.0.0.1:43110/mail.zeronetwork.bit)

[Código fuente](https://github.com/HelloZeroNet/ZeroMail)

---

## ZeroChat

El sitio finalizado para el tutorial de creación de una aplicación de chat P2P actualizada en tiempo real, con base de datos SQL y sin servidor, usando ZeroNet en menos de 100 lineas de código

 - Seleccionando el certificado ZeroID
 - Almacenando mensajes en una base de datos SQL
 - Creando tus propios mensajes y distribuyéndolos directamente a otros usuarios en tiempo real.
 - Actualiza en tiempo real los mensajes como vayan llegando


![ZeroChat](/img/zerochat.jpg)

Dirección del sitio finalizado: [1AvF5TpcaamRNtqvN1cnDEWzNmUtD47Npg](http://127.0.0.1:43110/1AvF5TpcaamRNtqvN1cnDEWzNmUtD47Npg)

Tutorial en ZeroBlog:
 [Parte 1](http://127.0.0.1:43110/Blog.ZeroNetwork.bit/?Post:43:ZeroNet+site+development+tutorial+1),
 [Parte 2](http://127.0.0.1:43110/Blog.ZeroNetwork.bit/?Post:46:ZeroNet+site+development+tutorial+2)

---

## ZeroMe

Red social P2P parecida a Twitter, descentralizada.

 - Almacena la información de usuario en el registro de usuario ZeroMe
 - Almacena las publicaciones y comenta en el MergeSite llamado Hub
 - Sube imágenes como archivos opcionales
 - Muestra en tiempo real de la actividad del muro


![ZeroMe](/img/zerome.jpg)

Dirección: [1MeFqFfFFGQfa1J3gJyYYUvb5Lksczq7nH](http://127.0.0.1:43110/1MeFqFfFFGQfa1J3gJyYYUvb5Lksczq7nH)

[Codigo fuente](https://github.com/HelloZeroNet/ZeroMe)

---

## ReactionGIFs

Demo para archivos opcional, los archivos de video solo se descargan si tu explorador los pide.

![ReactionGIFs](/img/reactiongifs.jpg)

Dirección: [1Gif7PqWTzVWDQ42Mo7np3zXmGAo3DXc7h](http://127.0.0.1:43110/1Gif7PqWTzVWDQ42Mo7np3zXmGAo3DXc7h)

[Codigo fuente](https://github.com/HelloZeroNet/ReactionGIFs)
