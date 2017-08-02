# Protocolo de red ZeroNet

 - Cada mensaje es codificado usando [MessagePack](http://msgpack.org/)

 - Cada petición tiene 3 parámetros:
    * `cmd`: El comando de petición
    * `req_id`: La identificación unica de la petición (simple, nonce incremented), el cliente tiene que incluir esto cuando responda al comando
    * `params`: Parametros para la petición
 - Ejemplo de petición: `{"cmd": "getFile", "req_id": 1, "params:" {"site": "1EU...", "inner_path": "content.json", "location": 0}}`
 - Ejemplo de respuesta: `{"cmd": "response", "to": 1, "body": "content.json content", "location": 1132, "size": 1132}`
 - Ejemplo de error de respuesta: `{"cmd": "response", "to": 1, "error": "Unknown site"}`


# Apretón de manos

Cada conección empieza con un apretón de manos enviando una petición a la dirección de red solicitada:

Parametro            | Descripción
                 --- | ---
**crypt**            | Nulo/Ninguno, solo usado en respuestas
**crypt_supported**  | Una matriz de métodos de cifrado de conexión soportados por el cliente
**fileserver_port**  | El puerto del servidor de archivos del cliente
**onion**            | (Sólo se utiliza en tor) La dirección del onion del cliente
**protocol**         | La versión de protocolo que el cliente utiliza (v1 o v2)
**port_opened**      | El estado de apertura del puerto de cliente del cliente
**peer_id**          | (No utilizado en tor) El peer_id del cliente
**rev**              | Número de revisión del cliente
**version**          | La versión del cliente
**target_ip**        | La dirección de red del servidor

El destino inicializa el cifrado en el socket basado en `crypt_supported`, luego devuelve:

Llave de retorno     | Descripción
                 --- | ---
**crypt**            | La encriptación a usar
**crypt_supported**  | Una matriz de métodos de cifrado de conexión soportados por el servidor
**fileserver_port**  | El puerto del servidor de archivos del servidor
**onion**            | (Sólo se utiliza en tor) La dirección del onion del cliente
**protocol**         | La versión de protocolo que el cliente utiliza (v1 o v2)
**port_opened**      | El estado de apertura del puerto de cliente del servidor
**peer_id**          | (No utilizado en tor) El peer_id del cliente
**rev**              | Número de revisión del servidor
**version**          | La versión del servidor
**target_ip**        | La dirección de red del cliente

> **Nota:** No se utiliza cifrado en las conexiones .onion, ya que la red Tor proporciona la seguridad de transporte de forma predeterminada.
> **Note:** También puede implícitamente inicializár el SSL antes del apretón de manos si puede asumir que es compatible con el cliente remoto.

**Ejemplo**:

Enviar apretón de manos:

```json
{
  "cmd": "handshake",
  "req_id": 0,
  "params": {
    "crypt": None,
    "crypt_supported": ["tls-rsa"],
    "fileserver_port": 15441,
    "onion": "zp2ynpztyxj2kw7x",
    "protocol": "v2",
    "port_opened": True,
    "peer_id": "-ZN0056-DMK3XX30mOrw",
    "rev": 2122,
    "target_ip": "192.168.1.13",
    "version": "0.5.6"
  }
}
```

Respuesta:

```
{
 "protocol": "v2",
 "onion": "boot3rdez4rzn36x",
 "to": 0,
 "crypt": None,
 "cmd": "response",
 "rev": 2092,
 "crypt_supported": [],
 "target_ip": "zp2ynpztyxj2kw7x.onion",
 "version": "0.5.5",
 "fileserver_port": 15441,
 "port_opened": False,
 "peer_id": ""
}
```

# Peticiones de pares

#### getFile _site_, _inner_path_, _location_, _[file_size]_
Solicitar un archivo del cliente

Parametro            | Descripción
                 --- | ---
**site**             | Dirección del sitio (ejemplo: 1EU1tbG9oC1A8jz2ouVwGZyQ5asrNsE4Vr)
**inner_path**       | Ruta del archivo relativa al directorio del sitio
**location**         | Solicitar archivo de este byte (un máximo de 512 bytes se envía por solicitud, por lo que necesita múltiples solicitudes para archivos más grandes)
**file_size**        | Tamaño total del archivo solicitado (opcional)

**Return**:

Llave de respuesta   | Descripción
                 --- | ---
**body**             | El contenido del archivo solicitado
**location**         | La ubicación del último byte enviado
**size**             | Tamaño total del archivo


---


#### ping
Comprueba si el cliente sigue vivo

**Return**:

Llave de respuesta   | Descripción
                 --- | ---
**body**             | Pong


---


#### pex _site_, _peers_, _need_
Intercambia pares con el cliente.
Los pares empacados a 6 bytes (4 byte IP usando inet_ntoa + 2 byte para el puerto)

Parametro            | Descripción
                 --- | ---
**site**             | Dirección del sitio (ejemplo: 1EU1tbG9oC1A8jz2ouVwGZyQ5asrNsE4Vr)
**peers**            | Lista de pares que el solicitante tiene (empacado)
**need**             | Número de pares que el solicitante desea

**Return**:

Llave de respuesta   | Descripción
                 --- | ---
**peers**            | Lista de pares que tiene para el sitio (empacado)


---

#### update _site_, _inner_path_, _body_
Actualizar un archivo del sitio.


Parametro            | Descripción
                 --- | ---
**site**             | Dirección del sitio (ejemplo: 1EU1tbG9oC1A8jz2ouVwGZyQ5asrNsE4Vr)
**inner_path**       | Ruta del archivo relativa al directorio del sitio
**body**             | Contenido completo del archivo actualizado

**Return**:

Llave de respuesta   | Descripción
                 --- | ---
**ok**               | Mensaje de agradecimiento en la actualización correcta :)

---

#### listModified _site_, _since_
Lista los archivos content.json modificados desde el parámetro dado. Se utiliza para obtener el contenido enviado por el usuario del sitio.


Parametro            | Descripción
                 --- | ---
**site**             | Dirección del sitio (ejemplo: 1EU1tbG9oC1A8jz2ouVwGZyQ5asrNsE4Vr)
**since**            | Lista de archivos content.json desde esta fecha y hora.

**Return**:

Llave de respuesta   | Descripción
                 --- | ---
**modified_files**   | Clave: content.json inner_path <br> Valor: fecha de la última modificación

**Ejemplo**:

```json
> zeronet.py --silent peerCmd 127.0.0.1 15441 listModified "{'site': '1BLogC9LN4oPDcruNz3qo1ysa133E9AGg8', 'since': 1497507030}"
{
  "to": 1,
  "cmd": "response",
  "modified_files": {
    "data/users/1NM9k7VJfrb1UWw5agAvyRfSn3ws1wTJ5U/content.json": 1497579272,
    "data/users/1QEfmMwKVxgR4rkREbdJYjgUmF3Zy8pwHt/content.json": 1497565986,
    "data/users/16NS3rBdW9zpLmLSQoD8nLTtNVsRFtVBhd/content.json": 1497575039,
    "data/users/1CjXarXgvcNeCJ2nMQxUi4DRFWp3GEur2W/content.json": 1497513808,
    "data/users/1L5rGDgTs4W2V7gekSvJNhKa7XaHkVwotD/content.json": 1497615798,
    "data/users/1LWuc6JBhUGrKEAh1aPrPU85dEMcKmg3pS/content.json": 1497594716,
    "data/users/1KdnTJVBGzEZrJppFZtzfG9chukuMv8xSb/content.json": 1497584640,
    "data/users/1GMNmr2bDPbT4c8yVnyCoDHke52CNCdqAa/content.json": 1497614188,
    "data/users/1GRm9rED83Tkfi3iWS9m3LWHiRpPZehWLd/content.json": 1497827772,
    "data/users/12Ugp53jiMdvj1Kxa1w7c2LcXUBdGPs1oK/content.json": 1497692901,
    "data/users/1F6BMqittjWUStzUbRXm2kG2GQ3RdBLqFQ/content.json": 1497571485,
    "data/users/1GgNo3CmxPd7n2pMSF3uyqf1XHvgtTUqCe/content.json": 1497560829,
    "data/users/16nArdxrSaNThNp83kL8E6NLL9WD98iUne/content.json": 1497627929,
    "data/users/16CAJkbfNRxNJq4aKdrZ2MSYFfFGvQ8JPi/content.json": 1497664899,
    "data/users/1DrBS2sTD3BX5BBxG8eqYsxXSvGt9kc5HE/content.json": 1497632000,
    "data/users/19sggoAZ4hcorrrfWoFWP9rwfpVsL29cnZ/content.json": 1497928134,
    "data/users/1NYpJupegoTXL4cFpkNdLNJ4XaAhTNhPe1/content.json": 1497535771,
    "data/users/1R67TfYzNkCnh89EFfGmXn5LMb4hXaMRQ/content.json": 1497691787,
    "data/users/1C9HXUYFSVafLxanwkaFPZRcRgCEGsj2Cn/content.json": 1497572833,
    "data/users/1LgoHzNGWeijeZbJ8a1YgGjMCnjaM4BWG/content.json": 1497620232,
    "content.json": 1497623639
  }
}
```

---


#### getHashfield _site_
Obtener el cliente descargado [ids de archivos opcionales] (# optional-file-id).

Parametro            | Descripción
                 --- | ---
**site**             | Dirección del sitio (ejemplo: 1EU1tbG9oC1A8jz2ouVwGZyQ5asrNsE4Vr)

**Return**:

Llave de respuesta   | Descripción
                 --- | ---
**hashfield_raw**    | IDs de archivos opcionales codificados usando `array.array (" H ", [1000, 1001 ..]). Tostring ()`

**Ejemplo**:
```json
> zeronet.py --silent peerCmd 192.168.1.13 15441 getHashfield "{'site': '1Gif7PqWTzVWDQ42Mo7np3zXmGAo3DXc7h'}
{
  'to': 1,
  'hashfield_raw': 'iG\xde\x02\xc6o\r;...',
  'cmd': 'response'
}
```

---


#### setHashfield _site_, _hashfield_raw_
Establezca la lista de [ids de archivos opcionales] (# optional-file-id) que tiene el cliente solicitante.

Parametro            | Descripción
                 --- | ---
**site**             | Dirección del sitio (ejemplo: 1EU1tbG9oC1A8jz2ouVwGZyQ5asrNsE4Vr)
**hashfield_raw**    | IDs de archivos opcionales codificados usando `array.array (" H ", [1000, 1001 ..]). Tostring ()`

**Return**:

Llave de respuesta   | Descripción
                 --- | ---
**ok**               | Actualizado


---


#### findHashIds _site_, _hash_ids_
Consulta si el cliente conoce a cualquier compañero que tenga los hash_ids solicitados

Parametro            | Descripción
                 --- | ---
**site**             | Dirección del sitio (ejemplo: 1EU1tbG9oC1A8jz2ouVwGZyQ5asrNsE4Vr)
**hash_ids**         | Lista de identificadores de archivos opcionales que el cliente está buscando actualmente

**Return**:

Llave de respuesta   | Descripción
                 --- | ---
**peers**            | Clave: ID de archivo opcional <br> Valor: Lista de los pares de ipv4 codificados utilizando `socket.inet_aton (ip) + struct.pack (" H ", puerto)`
**peers_onion**      | Clave: Opcional id de archivo <br> Valor: Lista de pares de cebolla codificada usando `base64.b32decode (onion.replace (" onion "," ") .upper ()) + struct.pack (" H ", puerto)

**Ejemplo**:
```json
> zeronet.py --silent peerCmd 192.168.1.13 15441 findHashIds "{'site': '1Gif7PqWTzVWDQ42Mo7np3zXmGAo3DXc7h', 'hash_ids': [59948, 29811]}"
{
  'to': 1,
  'peers': {
    29811: [
      'S&9\xd3Q<',
      '>f\x94\x98N\xa4',
      'gIB\x90Q<',
      '\xb4\xady\xf7Q<'
    ],
    59948: [
      'x\xcc>\xf6Q<',
      'S\xa1\xddkQ<',
      '\x05\xac\xe8\x8dQ<',
      '\x05\xc4\xe1\x93Q<',
      'Q\x02\xed\nQ<'
    ]
  },
  'cmd': 'response',
  'peers_onion': {
    29811: ['\xc7;A\xce\xbc\xd9O\xe2w<Q<'],
    59948: ['\xc7;A\xce\xbc\xd9O\xe2w<Q<']
  }
}
```

---


# ID de archivo opcional
Representación entera de los primeros 4 caracteres del hash:
```
>>> int("ea2c2acb30bd5e1249021976536574dd3f0fd83340e023bb4e78d0d818adf30a"[0:4], 16)
59948
```
