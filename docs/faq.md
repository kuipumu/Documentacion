# Preguntas frecuentes


#### ¿Necesito tener un puerto abierto?

Esto es __opcional__, puedes explorar y usar sitios ZeroNet sin un puerto abierto. Si quieres crear un nuevo sitio es altamente recomendado tener un puerto abierto.

Al inicio ZeroNet intenta abrir un puerto para ti en tu router usando [UPnP](https://es.wikipedia.org/wiki/Universal_Plug_and_Play), si esto falla tienes que hacerlo manualmente:


- Intenta acceder a la interfaz de tu router usando [http://192.168.1.1](http://192.168.1.1) o [http://192.168.0.1](http://192.168.0.1)
- Busca por una opción "Habilitar soporte UPnP" o algo similar y luego reinicia ZeroNet.

Si aun haciendo esto no funciona entonces intenta encontrar una sección de 'Reenvío de puertos'. Esto es diferente en cada router. [Aqui hay un articulo que lo explica brevemente.](https://www.adslzone.net/tutorial-36.3.html) El puerto para reenviar es 15441.

---


#### ¿ZeroNet es anónimo?

No es mas anónimo que BitTorrent, pero la privacidad (la posibilidad de encontrar quien es el dueño del comentario/sitio) sera incrementado en la medida que la red y los sitios ganen mas semillas.

ZeroNet esta hecho para trabajar con redes de anonimato: puedes fácilmente ocultar tu IP usando la red Tor.

---


#### ¿ Como usar ZeroNet en el explorador Tor?

En el modo Tor es recomendable usar ZeroNet en el explorador Tor:

-Inicia el explorador Tor
- Ve a la dirrección 'about:preferences#advanced'
- Clic en 'Configuración...'
- Coloca '127.0.0.1' en el campo de **No usar proxy para**
- Abre http://127.0.0.1:43110 en el explorador

Si todavía ves una pagina en blanco:
- Clic en el botón de NoScript's (el primero en la barra de herramientas)
- Elegir "Temporalmente permitir toda esta pagina"
- Recarga la pagina

---


#### ¿Como usar ZeroNet con Tor?

Si quieres ocultar tu IP instala la ultima versión de ZeroNet luego haz clic en Tor > Habilitar Tor para cada conección en ZeroHello.

En Windows Tor esta empacado con ZeroNet para otros sistemas operativos [sigue las instrucciones de instalación de Tor](https://es.gizmodo.com/como-empezar-a-utilizar-el-navegador-anonimo-tor-paso-1680401465), edita tu archivo de configuración torrc removiendo el '#' de la linea '# ControlPort 9051' luego reinicia tu servicio de Tor y ZeroNet.

> __Tip:__ Puedes verificar tu dirección IP usando las la pagina de [estadísticas](http://127.0.0.1:43110/Stats) de ZeroNet.

> __Tip:__ Si tienes errores de conección asegurate que tienes la ultima versión de Tor intalada (se requiere una versión superior a la 0.2.7.5)


---


#### ¿Como hacer funcionar ZeroNet con Tor bajo Linux?
Actualiza a la ultima versión de Tor (necesitamos una versión superior a la 0.2.7.5), sigue estas instrucciones ej. para Debian:

 - `echo 'deb http://deb.torproject.org/torproject.org jessie main'>> /etc/apt/sources.list.d/tor.list`
 - `gpg --keyserver keys.gnupg.net --recv 886DDD89`
 - `gpg --export A3C4F0F979CAA22CDBA8F512EE8CBC9E886DDD89 | apt-key add -`
 - `apt-get update`
 - `apt-get install tor`

Edita la configuración para habilitar el protocolo de control:

 - `mcedit /etc/tor/torrc`
 - Remuve el caracter `#` de las lineas `ControlPort 9051` y `CookieAuthentication 1` (linea ~57)
 - `/etc/init.d/tor restart`
 - Añade permiso para ti mismo para leer el cookie de autorizaciṕn con 'usermod -a -G debian-tor [tuusuariodelinux]' <br>(if no estas en Debian mira el archivo de grupo de usuario por 'ls -al /var/run/tor/control.authcookie')
- Cierra/Inicia con tu usuario para aplicar los cambios de grupo

> __Tip:__ Puedes verificar si tu Tor esta corriendo correctamente usando 'echo 'PROTOCOLINFO' | nc 127.0.0.1 9501

> __Tip:__ También es posible usar Tor sin modificar torrc (o de usar versiones mas antiguas de clientes Tor), corriendo 'zeronet.py --tor disable --proxy 127.0.0.1:9050 --disable_udp', pero entonces perderás la habilidad de hablar con otras direcciones .onion.

---


#### ¿Como hacer que Tor funcione si mi ISP o gobierno lo bloquea?

ZeroNet no incluye [transportes conectables de Tor](https://tb-manual.torproject.org/es-ES/circumvention.html) todavía. La manera mas sencilla de hacer Tor funcionar en una red censurada es iniciar el explorador Tor, ajustarlo para conectar con la red Tor con los transportes conectables que funcionen, y modificar la configuración de ZeroNet para usar el proxy del explorador Tor y controlar el puerto iniciando ZeroNet con --tor_controller 127.0.0.1:9151 --tor_proxy 127.0.0.1:9150 o añadiendo estos parámetros al archivo zeronet.conf

```
[global]
tor_controller = 127.0.0.1:9151
tor_proxy = 127.0.0.1:9150
```


---

#### ¿Puedo usar el mismo nombre de usuario en multiples maquinas?

Si, tienes que copiar el archivo 'data/users.json' a tu nueva maquina.

---

#### ¿Como crear una dirección agradable de la pagina?

Usa [vanitygen](https://bitcointalk.org/index.php?topic=25804.0) para generar uno. Una ves que tengas las llaves, crea el directorio 'data/1TuLlavePublica...tCkBzAXTKvJk4uj8'. Coloca algunos archivos ahí.
Luego navega a [http://127.0.0.1:43110/1TuLlavePublica...tCkBzAXTKvJk4uj8/]. Arrastra el botón "0" a la izquierda y usa la barra lateral para iniciar sesión en tu sitio.


---

#### ¿Como puedo registrar un dominio .bit?

Puedes registrar dominios .bit usando [Namecoin](https://namecoin.info/). Maneja tus dominios usando la interfaz visual del cliente o por la [interfaz de linea de comandos](https://flossystems.com/blog/2014/04/registro-de-dominios-bit-con-namecoin).

Luego de que la registración sea realizada tienes que editar el registro del dominio añadiendo la sección Zeronet a este, ej.:

```
{
...
    "zeronet": {
        "": "1EU1tbG9oC1A8jz2ouVwGZyQ5asrNsE4Vr",
        "blog": "1BLogC9LN4oPDcruNz3qo1ysa133E9AGg8",
        "talk": "1TaLk3zM7ZRskJvrh3ZNCDVGXvkJusPKQ"
    },
...
}
```
"" significa el dominio principal y cualquier otro que sea un sub-dominio.

> __Tip:__ Puedes comprar Namecoin para Bitcoin u otras cripto-currencias usando [shapeshift.io](https://shapeshift.io/).

> __Tip:__ Otras posibilidades de registrar dominios .bit: [domaincoin.net](https://domaincoin.net/), [peername.com](https://peername.com/), [dotbit.me](https://dotbit.me/)

> __Tip:__ Puedes verificar tu dominio en [namecha.in](http://namecha.in/), por ejemplo: [zeroid.bit](http://namecha.in/name/d/zeroid)

> __Tip:__ Deberías usar solamente [letras minusculas, numeros y - en tu dominio](http://wiki.namecoin.info/?title=Domain_Name_Specification_2.0#Valid_Domains).

> __Tip:__ Para hacer ZeroHello mostrar el nombre de tu dominio en ves de la dirección Bitcoin de tu sitio, añade una llave de dominio a tu content.json. ([Example](https://github.com/HelloZeroNet/ZeroBlog/blob/master/content.json#L6))

---

#### ¿Puedo usar la dirección/llave privada del sitio generado para aceptar pagos en Bitcoin?

Si, es una dirección Bitcoin estándar. La llave privada esta en formato WIF, así que tu puedes importarla en la mayoría de los clientes.

> __Tip:__ No esta recomendado mantener una gran cantidad de dinero en la dirección de tu sitio, por que tienes que entrar tu llave privada cada ves que modifiques tu sitio.

---

#### ¿Que pasa cuando alguien aloja contenido malicioso?

Los sitios de ZeroNet están en un recinto de seguridad, tienen los mismos privilegios como cualquier otro sitio que visitas por el Internet. Estas en total control con lo que almacenas. Si encuentras contenido sospechoso puedes para de alojar el sitio en cualquier momento.

---

#### ¿Es posible instalar ZeroNet a una maquina remota?

Si, tienes que habilitar el complemento UiPassword renombrando el directorio __plugins/disabled-UiPassword__ a __plugins/UiPassword__,
luego inicia ZeroNet en la maquina remota usando <br>'zeronet.py --ui_ip "X.X.X.X:XXXX" --ui_password cualquiercontraseña'.</br>
Esto va a enlazar la interfaz web gráfica de ZeroNet a todas las interfaces, pero para mantenerlo seguro solo puedes acceder a este entrando la contraseña que se le dio.

> __Tip:__ También puedes restringir la interfaz basada en la dirección ip usando `--ui_restrict ip1 ip2`.

> __Tip:__ Puedes especificar la contraseña en el archivo de configuración creando un archivo 'zeronet.conf' y añadir la linea  '[global]', 'ui_password = cualquiercontraseña' a este.


---


#### ¿Hay alguna forma de rastrear el ancho de banda que ZeroNet esta usando?

Los bytes de información enviados/recibidos son mostrados en la barra lateral de ZeroNet. <br>(abrela arrastrando el botón '0' de la parte superior derecha a la izquierda)

> __Tip:__  Pagina de estadística por conección: [http://127.0.0.1:43110/Stats](http://127.0.0.1:43110/Stats)


---


#### ¿Que pasa si dos personas usan la mismas llaves para modificar el sitio?

Cada archivo content.json tiene una marca de tiempo, los clientes siempre aceptan las mas nuevas.


---


#### ¿Zeronet usa la cadena de bloqueo de Bitcoin?

No, ZeroNet solo usa la criptografía del Bitcoin para las direcciones de sitio y la firma/verificación del contenido.
La autentificación de los usuarios también esta basada en el formato Bitcoin de [BIP32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki) (Documento solo disponible en ingles).

La cadena de bloqueo de Namecoin esta siendo usada para la registración de dominios.


---


#### ¿ZeroNet solo soporta sitios HTML y CSS?

ZeroNet esta construida para sitios actualizados en tiempo real, dinámicos, pero solo puedes servir cualquier tipo de archivo en su uso. (repositorios VCS, tu propio thin-cllient, base de datos, etc.)


---


#### ¿Como puedo crear un nuevo sitio ZeroNet?

[Sigue estas instrucciones.](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki) (Documento solo disponible en ingles).

---

#### ¿Como funciona?

- Cuando quieres abrir un nuevo sitio este pregunta por las direcciones IP de la red BitTorrent de los visitantes
- Primero descarga un archivo llamado __content.json__, el cual mantiene todos los otros nombres de archivo,
  __hashes__ y la firma criptográfica del dueño del sitio
- __Verifica__ el archivo content.json descargado usando la __dirección__ del sitio y la __firma__ del dueño del sitio del archivo
- __Descarga otro archivo__ (html, css, js...) y los verifica usando el hash SHA512 del archivo content.json
- Cada sitio visitado empieza a __tambien ser servido por ti__.
- Si el dueño del sitio (el cual tiene la llave privada para la dirección del sitio) __modifica__ el sitio, entonces el o ella firmara el nuevo content.json y __lo publicara a sus semillas__. Luego de que las semillas hayan verificado la integridad del content.json (usando la firma), estos __descargan los archivos modificados__ y publican el nuevo contenido a otras semillas.

Mas Información
 [Descripción de los sitios de prueba de ZeroNet](/usando_zeronet/sitios_de_muestra/),
 [Diapositivas acerca de como funciona ZeroNet](https://docs.google.com/presentation/d/1_2qK1IuOKJ51pgBvllZ9Yu7Au2l551t3XBgyTSvilew/pub)
