
# Referencia de la linea de comandos de ZeroNet



# Wrapper

_Estos comandos son manejados por el marco de contenedor y no se envían a UiServer mediante websocket_


#### wrapperConfirm _message, [button_caption]_

Mostrar una notificación con el botón de confirmación

Parámetro              | Descripción
                  ---  | ---
**message**            | El mensaje que desea mostrar
**button_caption** (opcional) | Leyenda del botón de confirmación (predeterminado: OK)

**Respuesta**: Es cierta si se hace clic en el botón

**Ejemplo:**
```coffeescript
# Delete site
siteDelete: (address) ->
	site = @sites[address]
	title = site.content.title
	if title.length > 40
		title = title.substring(0, 15)+"..."+title.substring(title.length-10)
	@cmd "wrapperConfirm", ["¿Estas seguro? <b>#{title}</b>", "Delete"], (confirmed) =>
		@log "Deleting #{site.address}...", confirmed
		if confirmed
			$(".site-#{site.address}").addClass("deleted")
			@cmd "siteDelete", {"address": address}
```


---


#### wrapperInnerLoaded

Aplica el archivo windows.location.hash a la url de la página. Llame cuando la página esté completamente cargada para saltar al punto de anclaje deseado.


---


#### wrapperGetLocalStorage

**Respuesta**: Almacenamiento local del navegador para el sitio

**Ejemplo:**
```coffeescript
@cmd "wrapperGetLocalStorage", [], (res) =>
	res ?= {}
	@log "Local storage value:", res
```



---

#### wrapperGetState

**Respuesta**: Objeto de estado del historial actual del explorador

---

#### wrapperNotification _type, message, [timeout]_

Muestra una notificación

Parámetro              | Descripción
                  ---  | ---
**type**               | Valores posibles: info, error, done
**message**            | El mensaje que quieres mostrar
**timeout** (optional) | Oculta el mensaje después de este intervalo (mili-segundos)

**Respuesta**: Ninguna

**Ejemplo:**
```coffeescript
@cmd "wrapperNotification", ["done", "¡Tu registración a sido enviada!", 10000]
```


---

#### wrapperOpenWindow _url, [target], [specs]_

Navega o abre una nueva ventana emergente.

Parámetro              | Descripción
                  ---  | ---
**url**                | Url de la pagina abierta
**target** (opcional)  | Nombre de la ventana destinada
**specs** (opcional)   | Propiedades especiales de la ventana (mira: [window.open specs](http://www.w3schools.com/jsref/met_win_open.asp))

**Ejemplo:**
```coffeescript
@cmd "wrapperOpenWindow", ["https://zeronet.io", "_blank", "width=550,height=600,location=no,menubar=no"]
```

---


#### wrapperPermissionAdd _permission_

Solicita un nuevo permiso para el sitio

Parámetro        | Descripción
             --- | ---
**permission**   | Nombre del permiso (ej. Merger:ZeroMe)


---

#### wrapperPrompt _message, [type]_

Muestra la entrada de texto del usuario

Parámetro           | Descripción
               ---  | ---
**message**         | El mensaje que quieres mostrar
**type** (optional) | El tipo de entrada (determinado: text)

**Respuesta**: Texto ingresado en la entrada

**Ejemplo:**
```coffeescript
# Prompt the private key
@cmd "wrapperPrompt", ["Ingresa tu llave privada:", "password"], (privatekey) =>
    $(".publishbar .button").addClass("loading")
    # Enviar firma a content.json y publicar solicitud al servidor
    @cmd "sitePublish", [privatekey], (res) =>
        $(".publishbar .button").removeClass("loading")
        @log "Publish result:", res
```


---

#### wrapperPushState _state, title, url_

Cambia el url y añade una nueva entrada al historial del explorador. Mira:
 [pushState JS method](https://developer.mozilla.org/en-US/docs/Web/API/History_API#The_pushState()_method)_

Parámetro           | Descripción
               ---  | ---
**state**           | Estado del objeto javascript
**title**           | Titulo de la pagina
**url**             | Url de la pagina

**Respuesta**: Ninguna

```coffeescript
@cmd "wrapperPushState", [{"scrollY": 100}, "Pagina de perfil", "Perfil"]
```


---

#### wrapperReplaceState _state, title, url_

Cambia el url sin añadir una nueva entrada al historial del explorador. Mira:
 [replaceState JS method](https://developer.mozilla.org/en-US/docs/Web/API/History_API#The_replaceState()_method)_

Parámetro           | Descripción
               ---  | ---
**state**           | Estado del objeto javascript
**title**           | Titulo de la pagina
**url**             | Url de la pagina

**Respuesta**: Ninguna

```coffeescript
@cmd "wrapperReplaceState", [{"scrollY": 100}, "Pagina de Perfil", "Perfil"]
```


---

#### wrapperSetLocalStorage _data_

Asigna el alojamiento local de datos del explorador para el sitio

Parámetro              | Descripción
                  ---  | ---
**data**               | Cualquier estructura de datos que quieres almacenar para el sitio

**Respuesta**: Ninguna

**Ejemplo:**
```coffeescript
Page.local_storage["topic.#{@topic_id}_#{@topic_user_id}.visited"] = Time.timestamp()
Page.cmd "wrapperSetLocalStorage", Page.local_storage
```


---

#### wrapperSetTitle _title_

Asigna el titulo del sitio

Parámetro              | Descripción
                  ---  | ---
**title**              | Nuevo titulo de pestaña del explorador

**Respuesta**: Ninguna

**Ejemplo:**
```coffeescript
Page.cmd "wrapperSetTitle", "newtitle"
```

---


#### wrapperSetViewport _viewport_

Asigna la etiqueta de meta datos del viewport del contenido (requerido para sitios móviles)


Parámetro           | Descripción
               ---  | ---
**viewport**        | La etiqueta de meta datos del viewport del contenido

**Respuesta**: Ninguna

**Ejemplo:**
```coffeescript
# Solicitar la clave privada
@cmd "wrapperSetViewport", "width=device-width, initial-scale=1.0"
```


---



# UiServer

El UiServer es para ZeroNet lo que para las paginas web normales es la configuración LAMP.

El UiServer hará todo el trabajo 'backend' (por ejemplo: consultar el DB, acceder a archivos, etc.). Estas son las llamadas de la API que necesitará para dinamizar su sitio.



#### certAdd _domain, auth_type, auth_user_name, cert_

Añade un nuevo certificado al usuario actual.

Parámetro            | Descripción
                 --- | ---
**domain**           | Dominio emisor del certificado
**auth_type**        | Tipo de autenticación utilizado en el registro
**auth_user_name**   | Nombre de usuario utilizado en el registro
**cert**             | El cert en sí: `auth_address#auth_type/auth_user_name` Cadena firmada por el propietario del sitio cert

**Respuesta**: "ok", "Not changed" o {"error": error_message}

**Ejemplo:**
```coffeescript
@cmd "certAdd", ["zeroid.bit", auth_type, user_name, cert_sign], (res) =>
	$(".ui").removeClass("flipped")
	if res.error
		@cmd "wrapperNotification", ["error", "#{res.error}"]
```


---


#### certSelect _accepted_domains_

Muestra el selector de certificado.

Parámetro            | Descripción
                 --- | ---
**accepted_domains** | Lista de dominios que son aceptados por el sitio como proveedor de autorización

**Respuesta**: Ninguna

**Ejemplo:**
```coffeescript
@cmd "certSelect", {"accepted_domains": ["zeroid.bit"]}
```


---


#### channelJoin _channel_

Solicita notificaciones acerca de los eventos del sitio.

Parámetro   | Descripción
        --- | ---
**channel** | Canal a unirse

**Respuesta**: Ninguna

**Channels**:

 - **siteChanged** (joined by default)<br>Events: peers_added, file_started, file_done, file_failed

**Ejemplo**:
```coffeescript
# Wrapper websocket conexión lista
onOpenWebsocket: (e) =>
	@cmd "channelJoinAllsite", {"channel": "siteChanged"}

# Ruta de solicitudes y mensajes entrantes
route: (cmd, data) ->
	if cmd == "setSiteInfo"
		@log "Site changed", data
	else
		@log "Unknown command", cmd, data
```

**Ejemplo de dato de evento**
```json
{
	"tasks":0,
	"size_limit":10,
	"address":"1RivERqttrjFqwp9YH1FviduBosQPtdBN",
	"next_size_limit":10,
	"event":[ "file_done", "index.html" ],
	[...] # Lo mismo que siteInfo return dict
}

```


---


#### dbQuery _query_

Ejecutar una consulta en la caché de SQL

Parámetro            | Descripción
                 --- | ---
**query**            | Comando de consulta de SQL

**Respuesta**: <list> Resultado de la consulta

**Ejemplo:**
```coffeescript
@log "Updating user info...", @my_address
Page.cmd "dbQuery", ["SELECT user.*, json.json_id AS data_json_id FROM user LEFT JOIN json USING(path) WHERE path='#{@my_address}/data.json'"], (res) =>
	if res.error or res.length == 0 # Db no está listo todavía o Ningún usuario encontrado
		$(".head-user.visitor").css("display", "")
		$(".user_name-my").text("Visitor")
		if cb then cb()
		return

	@my_row = res[0]
	@my_id = @my_row["user_id"]
	@my_name = @my_row["user_name"]
	@my_max_size = @my_row["max_size"]
```



---


#### fileDelete _inner_path_

Borrar un archivo

Parámetro        | Descripción
             --- | ---
**inner_path**   | El archivo que quieres borrar

**Respuesta**: "ok" si se logro, de lo contrario un mensaje de error.


---


#### fileGet _inner_path, [required], [format], [timeout]_

Obtener el contenido del archivo

Parámetro               | Descripción
                    --- | ---
**inner_path**          | El archivo que quieres obtener
**required** (optional) | Trata y espera por el archivo si este no existe. (determinado: True)
**format** (optional)   | Codificado de los datos de la respuesta. (texto o base64) (determinado: text)
**timeout** (optional)  | Tiempo máximo de espera para que los datos lleguen (determinado: 300)


**Respuesta**: <string> El contenido del archivo


**Ejemplo:**
```coffeescript
# Voto positivo en un tópico de ZeroTalk
submitTopicVote: (e) =>
	if not Users.my_name # No registrado
		Page.cmd "wrapperNotification", ["info", "Por favor, pide acceso antes de publicar."]
		return false

	elem = $(e.currentTarget)
	elem.toggleClass("active").addClass("loading")
	inner_path = "data/users/#{Users.my_address}/data.json"

	Page.cmd "fileGet", [inner_path], (data) =>
		data = JSON.parse(data)
		data.topic_votes ?= {} # Crear si no existe
		topic_address = elem.parents(".topic").data("topic_address")

		if elem.hasClass("active") # Añade voto positivo al tópico
			data.topic_votes[topic_address] = 1
		else # Remueve el voto positivo del tópico
			delete data.topic_votes[topic_address]

		# Escribir el archivo y publicarlo a otros pares
		Page.writePublish inner_path, Page.jsonEncode(data), (res) =>
			elem.removeClass("loading")
			if res == true
				@log "File written"
			else # Fallo
				elem.toggleClass("active") # Cambiar de vuelta

	return false
```


---


#### fileList _inner_path_

Lista de archivos en el directorio

Parámetro        | Descripción
             --- | ---
**inner_path**   | El directorio que quieres listar

**Respuesta**: Lista de archivos en el directorio (recursivo)


---


#### fileQuery _dir_inner_path, query_

Comando simple de consulta de archivos json

Parámetro            | Descripción
                 --- | ---
**dir_inner_path**   | Patrón de archivos consultados
**query**            | Comando de consulta (opcional)

**Respuesta**: <list> Contenido coincidente

**Query examples:**

 - `["data/users/*/data.json", "topics"]`:Devuelve todos los temas nodos de todos los archivos de usuario
 - `["data/users/*/data.json", "comments.1@2"]`: Respuestas `user_data["comments"]["1@2"]` Valor de todos los archivos de usuario
 - `["data/users/*/data.json", ""]`: Devuelve todos los datos de los archivos de los usuarios
 - `["data/users/*/data.json"]`: Devuelve todos los datos de los archivos de los usuarios (igual que anteriormente)

**Ejemplo:**
```coffeescript
@cmd "fileQuery", ["data/users/*/data.json", "topics"], (topics) =>
	topics.sort (a, b) -> # Ordenar por fecha
		return a.added - b.added
	for topic in topics
		@log topic.topic_id, topic.inner_path, topic.title
```


---


#### fileRules _inner_path_

Devolver las reglas para el archivo

Parámetro            | Descripción
                 --- | ---
**inner_path**       | Directorio interno del archivo

**Return**: <list> Contenido que encaja

**Example result:**

```json
{
	"current_size": 2485,
	"cert_signers": {"zeroid.bit": ["1iD5ZQJMNXu43w1qLB8sfdHVKppVMduGz"]},
	"files_allowed": "data.json",
	"signers": ["1J3rJ8ecnwH2EPYa6MrgZttBNc61ACFiCj"],
	"user_address": "1J3rJ8ecnwH2EPYa6MrgZttBNc61ACFiCj",
	"max_size": 100000
}
```

**Ejemplo:**
```coffeescript
@cmd "fileRules", "data/users/1J3rJ8ecnwH2EPYa6MrgZttBNc61ACFiCj/content.json", (rules) =>
	@log rules
```


---


#### fileWrite _inner_path, content_

Escribir archivo en el contenido


Parámetro          | Descripción
               --- | ---
**inner_path**     | Directorio interno del archivo que quieres escribir
**content_base64** | Contenido que quieres escribir al archivo (base64 codificado)

**Respuesta**: "ok" si se logro, de lo contrario un mensaje de error.

**Ejemplo:**
```coffeescript
writeData: (cb=null) ->
	# Codificar a json, codificar utf8
	json_raw = unescape(encodeURIComponent(JSON.stringify({"hello": "ZeroNet"}, undefined, '\t')))
	# Convertir a base64 y enviar
	@cmd "fileWrite", ["data.json", btoa(json_raw)], (res) =>
		if res == "ok"
			if cb then cb(true)
		else
			@cmd "wrapperNotification", ["error", "File write error: #{res}"]
			if cb then cb(false)
```

_Nota:_ para escribir archivos que no estén todavía en content.json, debes tener `"own": true` en `data/sites.json` en el sitio que quieres escribir


---



#### serverInfo

**Respuesta:** <dict> Toda la información acerca del servidor

**Ejemplo:**
```coffeescript
@cmd "serverInfo", {}, (server_info) =>
	@log "Server info:", server_info
```

**Example return value:**
```json
{
	"debug": true, # Corriendo en modo de depuración
	"fileserver_ip": "*", # Servidor de archivos enlazar a
	"fileserver_port": 15441, # Puerto de servidor de archivos
	"ip_external": true, # Activo del modo pasivo
	"platform": "win32", # Sistema Operativo
	"ui_ip": "127.0.0.1", # UiServer vinculado a
	"ui_port": 43110, # Puerto UiServer (Web)
	"version": "0.2.5" # Versión
}
```




---


#### siteInfo

**Respuesta**: <dict> Toda la información acerca del sitio

**Ejemplo:**
```coffeescript
@cmd "siteInfo", {}, (site_info) =>
	@log "Site info:", site_info
```

**Example return value:**
```json
{
	"tasks": 0, # Número de archivos actualmente en descarga
	"size_limit": 10, # Límite de tamaño del sitio actual en MB
	"address": "1EU1tbG9oC1A8jz2ouVwGZyQ5asrNsE4Vr", # Dirección del sitio
	"next_size_limit": 10, # Límite de tamaño requerido por la suma de archivos del sitio
	"auth_address": "2D6xXUmCVAXGrbVUGRRJdS4j1hif1EMfae", # Dirección de Bitcoin del usuario actual
	"auth_key_sha512": "269a0f4c1e0c697b9d56ccffd9a9748098e51acc5d2807adc15a587779be13cf", # Obsoleta, no la utilice
	"peers": 14, # Pares del sitio
	"auth_key": "pOBdl00EJ29Ad8OmVIc763k4", # Obsoleta, no la utilice
	"settings":  {
		"peers": 13, # Saved peers num for sorting
		"serving": true, # Sitio habilitado
		"modified": 1425344149.365, # Ultima fecha de modificación de todos los archivos del sitio
		"own": true, # Own site
		"permissions": ["ADMIN"], # Permiso del sitio
		"size": 342165 # Tamaño total del sitio en bytes
	},
	"bad_files": 0, # Los archivos que necesitan ser descargados
	"workers": 0, # Descargas concurrentes actuales
	"content": { # Raiz de content.json
		"files": 12, # Numero de archivo, información detallada del archivo removida para reducir la transferencia de datos y parse time
		"description": "Este sitio",
		"title": "ZeroHello",
		"signs_required": 1,
		"modified": 1425344149.365,
		"ignore": "(js|css)/(?!all.(js|css))",
		"signers_sign": null,
		"address": "1EU1tbG9oC1A8jz2ouVwGZyQ5asrNsE4Vr",
		"zeronet_version": "0.2.5",
		"includes": 0
	},
	"cert_user_id": "zeronetuser@zeroid.bit", # Certificado actualmente seleccionado para el sitio
	"started_task_num": 1, # Ultimo número de archivos descargados
	"content_updated": 1426008289.71 # Tiempo de actualización de Content.json
}
```


---


#### sitePublish _[privatekey], [inner_path], [sign]_

Publicar un content.json del sitio

Parámetro                 | Descripción
                      --- | ---
**privatekey** (opcional) | Llave privada usada para el firmado (determinado: llave privada actual del usuario)
**inner_path** (opcional) | Directorio interno del content.json que quieres publicar (determinado: content.json)
**sign** (opcional)       | Si es cierto entonces firma el content.json antes de publicarlo (determinado: Cierto)

**Respuesta**: "ok" si se logro, de lo contrario un mensaje de error.

**Ejemplo:**
```coffeescript
# Prompt the private key
@cmd "wrapperPrompt", ["Entra tu llave privada:", "password"], (privatekey) =>
	$(".publishbar .button").addClass("loading")
	# Send sign content.json and publish request to server
	@cmd "sitePublish", [privatekey], (res) =>
		$(".publishbar .button").removeClass("loading")
		@log "Publish result:", res
```


---


#### siteSign _[privatekey], [inner_path]_

Firma un content.json del sitio

Parámetro                 | Descripción
                      --- | ---
**privatekey** (opcional) | Llave privada usada para el firmado (determinado: llave privada actual del usuario)
**inner_path** (opcional) | Directorio interno del content.json que quieres publicar (determinado: content.json)

**Respuesta**: "ok" si se logro, de lo contrario un mensaje de error.

> __Nota:__
> Usa "stored" como "privatekey" si este esta definido en users.json (ej. sitios clonados)

**Ejemplo:**
```coffeescript
if @site_info["privatekey"] # Llave privada almacenada en users.json
	@cmd "siteSign", ["stored", "content.json"], (res) =>
		@log "Sign result", res
```


---



#### siteUpdate _address_

Fuerza el chequeo y descarga del contenido cambiado de otros pares (solo es necesario si el usuario esta en el modo pasivo y esta usando una versión antigua de ZeroNet)

Parámetro     | Descripción
          --- | ---
**address**   | Dirección del sitio que quieres actualizar (solo se permite en el sitio actual sin el permiso del ADMIN)

**Respuesta:** Ninguna

**Ejemplo:**
```coffeescript
# Actualización manual del sitio para conexiones pasivas
updateSite: =>
	$("#passive_error a").addClass("loading").removeClassLater("loading", 1000)
	@log "Actualizando sitio..."
	@cmd "siteUpdate", {"address": @site_info.address}
```


---


# Complemento: Encriptar Mensaje


#### userPublickey _[index]_

Obtener la llave publica especifica del usuario en el sitio

Parámetro            | Descripción
                 --- | ---
**index** (opcional) | Sub-publickey within site (default: 0)


**Respuesta**: base64 llave publica codificada

---

#### eciesEncrypt _text, [publickey], [return_aes_key]_

Encrípta un texto usando una llave publicar

Parámetro                      | Descripción
                           --- | ---
**text**                       | Texto a encríptar
**publickey** (opcional)       | Indice de llave publica del usuario (int) o llave publica codificada en base64 (determinado: 0)
**return_aes_key** (opcional)  | Obtén la llave AES usada en la encriptación (determinado: falso)


**Respuesta**: Texto encriptado en el formato base64 o [Texto encriptado en el formato base64, llave AES en el formato base64]

---

#### eciesDecrypt _params, [privatekey]_

Trata de desencriptar la lista de textos

Parámetro                      | Descripción
                           --- | ---
**params**                     | Un texto o lista de textos encriptados
**privatekey** (opcional)      | Indice de llave privada de usuario (int) o llave privada codificada en base64 (determinado: 0)


**Respuesta**: Texto desencriptado o lista de textos desencriptados (nulo para decodificaciones fallidas)


---

#### aesEncrypt _text, [key], [iv]_

Encripta un texto usando la llave y el iv

Parámetro                      | Descripción
                           --- | ---
**text**                       | Un texto encriptado usando AES
**key** (opcional)             | Contraseña codificada en base64 (determinado: generar nueva)
**iv** (opcional)              | Iv codificado en base64 (determinado: generar nueva)


**Respuesta**: [llave codificada en base64, iv codificado en base64, texto encriptado codificado en base64]


---

#### aesDecrypt _iv, encrypted_text, key_
#### aesDecrypt _encrypted_texts, keys_

Desencriptar el texto usando la llave IV y AES

Parámetro                      | Descripción
                           --- | ---
**iv**                         | Iv en el formato base64
**encrypted_text**             | Texto encriptado en el formato Base64
**encrypted_texts**            | Lista de [iv codificado en base64, texto encriptado codificado en base64] pares
**key**                        | Contraseña codificada en base64 para el texto
**keys**                       | Llaves para decodificar (Intenta cada uno por cada pareja)


**Respuesta**: Texto decodificado o lista de textos descodificados


---


# Complemento: Noticias


#### feedFollow _feeds_

Establecer consultas SQL seguidas.

La consulta SQL debería resultar en filas con columnas:

Field          | Descripción
           --- | ---
**type**       | Tipo: publicación, artículo, comentario, mención
**date_added** | Hora del evento
**title**      | Se mostrará la primera línea del evento
**body**       | Segunda y tercera línea del evento
**url**        | Enlace a la página de eventos

Parámetro      | Descripción
           --- | ---
**feeds**      | Formato: {"nombre de la consulta": [consulta SQL, [param1, param2, ...], ...}, los parámetros se escaparán, unidos por `,` insertados en lugar de `: params` en la consulta Sql.

**Respuesta**: ok

**Ejemplo:**
```coffeescript
# Sigue las publicaciones de ZeroBlog
query = "
	SELECT
	 post_id AS event_uri,
	 'post' AS type,
	 date_published AS date_added,
	 title AS title,
	 body AS body,
	 '?Post:' || post_id AS url
	FROM post
"
params = [""]
Page.cmd feedFollow [{"Posts": [query, params]}]
```

---

#### feedListFollow

Respuesta de los feeds de paginas seguidas


**Respuesta**: Los feeds actualmente seguidos en el mismo formato que en los comandos feedFollow


---

#### feedQuery _[limit], [day_limit]_

Ejecutar todo seguido consulta sql


**Respuesta**: El resultado de las consultas Sql siguientes

Parámetro            | Descripción
                 --- | ---
**limit**            | Límite de resultados por sitio seguido (valor predeterminado: 10)
**day_limit**        | Devolver no más antiguo que el número de estos días (predeterminado: 3)


---


# Plugin: MergerSite


#### mergerSiteAdd _addresses_

Iniciar la descarga de un nuevo sitio(s) combinado

Parámetro            | Descripción
                 --- | ---
**addresses**        | Dirección del sitio o lista de direcciones del sitio


---

#### mergerSiteDelete _address_

Detener la siembra y eliminar un sitio combinado

Parámetro            | Descripción
                 --- | ---
**address**           | Dirección del sitio


---

#### mergerSiteList _[query_site_info]_

Devolver sitios combinados.

Parámetro            | Descripción
                 --- | ---
**query_site_info**  | Si es cierto, devuelve información detallada del sitio para sitios fusionados


---


# Complemento: Silenciar

#### muteAdd _auth_address_, _cert_user_id_, _reason_

Agregar nuevo usuario a la lista de silencio. (Requiere confirmación para sitios que no son ADMIN)

Parámetro            | Descripción
                 --- | ---
**auth_address**     | Nombre del directorio de los datos del usuario.
**cert_user_id**     | Cert nombre de usuario del usuario
**reason**           | Motivo del silenciamiento

**Respuesta**: Ok si está confirmado

**Ejemplo:**
```coffeescript
Page.cmd("muteAdd", ['1GJUaZMjTfeETdYUhchSkDijv6LVhjekHz','helloworld@kaffie.bit','Spammer'])
```

---

#### muteRemove _auth_address_

Quitar un usuario de la lista de silencio. (Requiere confirmación para sitios que no son ADMIN)

Parámetro            | Descripción
                 --- | ---
**auth_address**     | Nombre del directorio de los datos del usuario.

**Respuesta**: Ok si está confirmado

**Ejemplo:**
```coffeescript
Page.cmd("muteRemove", '1GJUaZMjTfeETdYUhchSkDijv6LVhjekHz')
```

---

#### muteList

Lista de usuarios silenciados. (Requiere permiso de ADMIN en el sitio)

**Respuesta**: Lista de usuarios silenciados


---


# Plugin: OptionalManager

#### optionalFileList _[address]_, _[orderby]_, _[limit]_

Devolver la lista de archivos opcionales

Parámetro            | Descripción
                 --- | ---
**address**          | La dirección del sitio que desea listar los archivos opcionales (predeterminado: sitio actual)
**orderby**          | Orden de los archivos opcionales devueltos (predeterminado: time_downloaded DESC)
**limit**            | Número máximo de archivos opcionales devueltos (predeterminado: 10)

**Respuesta**: Fila de la base de datos de los archivos opcionales: file_id, site_id, inner_path, hash_id, size, peer, uploaded, is_downloaded, is_pinned, time_added, time_downlaoded, time_accessed

---

#### optionalFileInfo _inner_path_

Consulta de información de archivo opcional de la base de datos

Parámetro            | Descripción
                 --- | ---
**inner_path**       | La ruta del archivo

**Respuesta**: Fila de la base de datos de los archivos opcionales: file_id, site_id, inner_path, hash_id, size, peer, uploaded, is_downloaded, is_pinned, time_added, time_downlaoded, time_accessed

---

#### optionalFilePin _inner_path_, _[address]_

Marcado (excluir de la limpieza automática de archivos automatizada) del archivo opcional descargado

Parámetro            | Descripción
                 --- | ---
**inner_path**       | La ruta del archivo
**address**          | Dirección del archivo (predeterminado: sitio actual)

---

#### optionalFileUnpin _inner_path_, _[address]_

Quitar marcado (incluir de la limpieza automática de archivos automatizada) del archivo opcional descargado

Parámetro            | Descripción
                 --- | ---
**inner_path**       | La ruta del archivo
**address**          | Dirección del archivo (predeterminado: sitio actual)

---

#### optionalFileDelete _inner_path_, _[address]_

Consultar un archivo opcional descargado

Parámetro            | Descripción
                 --- | ---
**inner_path**       | La ruta del archivo
**address**          | irección del archivo (predeterminado: sitio actual)

---

#### optionalLimitStats

Devolver el espacio en disco actualmente utilizado por los archivos opcionales

**Respuesta**: Límite, estadística del espacio usado y libre

---


#### actionOptionalLimitSet _limit_

Establecer el límite de archivos opcional

Parámetro            | Descripción
                 --- | ---
**limit**            | Ax espacio utilizado por los archivos opcionales en gb o por ciento del espacio utilizado

---

#### actionOptionalHelpList _[address]_

Lista de los directorios descargados automáticamente de los archivos opcionales

Parámetro            | Descripción
                 --- | ---
**address**          | Dirección del sitio que desea listar directorios ayudados (predeterminado: sitio actual)

**Respuesta**: Dict de directorios y descripciones auto-descargadas

---


#### actionOptionalHelp directory, title, _[address]_

Añadir directorio a la lista de descarga automática

Parámetro            | Descripción
                 --- | ---
**directory**        | Directorio que desea agregar a la lista de descarga automática
**title**            | Título de la entrada (se muestra en ZeroHello)
**address**          | Dirección del sitio al que desea agregar el directorio de descarga automática (predeterminado: sitio actual)

---

#### actionOptionalHelpRemove directory, _[address]_

Eliminar una entrada de descarga automática

Parámetro            | Descripción
                 --- | ---
**directory**        | Directorio que desea eliminar de la lista de descarga automática
**address**          | Dirección del sitio afectado (predeterminado: sitio actual)

---

#### actionOptionalHelpAll value, _[address]_

Ayuda a descargar cada nuevo archivo opcional cargado en el sitio

Parámetro            | Descripción
                 --- | ---
**value**            | Activar o desactivar la descarga automática
**address**          | Dirección del sitio afectado (predeterminado: sitio actual)


---


# Comandos del administrador
_(Requiere permiso de ADMIN en data / sites.json)_



---


#### configSet _key, value_

Cree o actualice una entrada en el archivo de configuración de ZeroNet. (zeronet.conf de forma predeterminada)


Parámetro            | Descripción
                 --- | ---
**key**              | Nombre de entrada de la configuración
**value**            | Entrada de configuración del nuevo valor


**Respuesta**: Ok


---



#### certSet _domain_

Establezca el certificado usado para el sitio actual.

Parámetro            | Descripción
                 --- | ---
**domain**           | Dominio del emisor del certificado

**Respuesta**: Ninguna


---


#### channelJoinAllsite _channel_

Solicite notificaciones sobre los eventos de cada sitio.

Parámetro           | Descripción
               ---  | ---
**channel**         | Canal para unirse (ver channelJoin)

**Respuesta**: Ninguna




---


#### serverPortcheck

Comenzar a comprobar si el puerto está abierto

**Respuesta**: True (puerto abierto) o False (puerto cerrado)


---


#### serverShutdown

Deje de ejecutar el cliente ZeroNet.

**Respuesta**: Ninguna



---


#### serverUpdate

Vuelva a descargar ZeroNet de Github.

**Respuesta**: Ninguna


---


#### siteClone _address_, _[root_inner_path]_

Copie los archivos del sitio en uno nuevo.

Cada archivo y directorio se omitirá si tiene una versión predeterminada `-default` y la versión subfijada se copiará en su lugar.

Ej. Si tiene un directorio `data` y un directorio` data-default`: El directorio `data` no será copiado y el directorio` data-default` será renombrado a datos.

Parámetro           | Descripción
               ---  | ---
**address**         | Dirección del sitio desea clonar
**root_inner_path** | El directorio fuente del nuevo sitio

**Respuesta**: Ninguna, redirecciona automáticamente a nuevo sitio en la terminación


---


#### siteList

**Respuesta**: <list> Lista SiteInfo de todos los sitios descargados


---


#### sitePause _address_

Pausa la publicación del sitio

Parámetro           | Descripción
               ---  | ---
**address**         | La dirección del sitio desea pausar

**Respuesta**: Ninguna


---


#### siteResume _address_

Reanudar la publicación del sitio

Parámetro           | Descripción
               ---  | ---
**address**         | Dirección del sitio que desea reanudar


**Respuesta**: Ninguna



