# Estructura del content.json

Cada sitio de ZeroNet tendrá un archivo `content.json`. ([Ejemplo de archivo content.json] (https://github.com/HelloZeroNet/ZeroTalk/blob/master/content.json))

Este archivo llevará, entre otras cosas, una lista de todos los archivos de su sitio y una firma creada con su clave privada. Esto se hace para evitar la manipulación de archivos (es decir, sólo usted, o personas de su confianza, puede actualizar el contenido del sitio).

Aquí hay una lista de claves admitidas:


# Generado automáticamente


---

### address

La dirección de su sitio

**Ejemplo**: 1TaLk3zM7ZRskJvrh3ZNCDVGXvkJusPKQ


---


### address_index

La dirección del sitio BIP32 índice de sub-clave de su semilla BIP32. Agregado automáticamente cuando clones un sitio. Permite recuperar la clave privada del sitio de tu semilla BIP32.

**Ejemplo**: 30926910

---


### cloned_from

La dirección del sitio donde se clona el sitio.

**Ejemplo**: 1BLogC9LN4oPDcruNz3qo1ysa133E9AGg8

---


### clone_root

El sub-directorio en el sitio de origen del cual se clonó este sitio.

**Ejemplo**: plantilla-nueva


---


### files

Tamaño y sha512 hash de archivos descargados automáticamente contenidos en su sitio. Se agregó automáticamente mediante el comando `zeronet.py siteSign siteaddress privatekey`.

**Ejemplo**:
```json
    "css/all.css": {
      "sha512": "869b09328f07bac538c313c4702baa5276544346418378199fa5cef644c139e8",
      "size": 148208
```


---


### files_optional

Tamaño y sha512 hash de archivos opcionales contenidos en su sitio. Se agregó automáticamente mediante el comando `zeronet.py siteSign siteaddress privatekey`.

**Ejemplo**:
```json
    "data/mivideo.mp4": {
      "sha512": "538c09328aa52765443464135cef644c144346418378199fa5cef61837819538",
      "size": 832103
```



---


### modified

Hora en que se generó el archivo content.json.

**Ejemplo**: 1425857522.076


---


### sign (obsoleto)

ECDSA signo del contenido del archivo content.json. (Claves ordenadas, sin espacios en blanco y los nodos `sign` y` signers_sign`). Para compatibilidad con versiones anteriores, se eliminará pronto.

**Ejemplo**:
```json
  "sign": [
    43117356513690007125104018825100786623580298637039067305407092800990252156956,
    94139380599940414070721501960181245022427741524702752954181461080408625270000
  ],
```


---


### signers_sign

Posibles direcciones de firmantes para el contenido raíz.json firmado usando la clave privada de la dirección del sitio. (Posibilidad de Multisig)

**Formato de la cadena firmada**: [numero_de_firmantes_requeridos]:[dirección de firmante],[dirección de firmante]

**Ejemplo**: <small>HKNDz9IUHcBc/l2Jm2Bl70XQDL9HYHhJ2hUdg8AMyunACLgxyXBr7EW1/ME4hGkaFZSFmIxlInmxH+BrMVXbnLw=</small>

*Otro ejemplo*:
```
signs_required: 1:1PcxwuHYxuJEmM4ydtB1vbiAY6WkNgsz9G,1CK6KHY6MHgYvmRQ4PAafKYDrg1ejbH1cE
signed message: MEUCIQDuz+CzOVvFkv1P2ra9i5E1p1G0/1cOGecm7GpLpMLhuwIgBIbCL0YHXD1S2+x48QS5VO/rISrkdLiUR+o+x1X0y1A=
```
El mensaje firmado arriba se firma usando la dirección, "1PcxwuHYxuJEmM4ydtB1vbiAY6WkNgsz9G"

---


### signs

ECDSA firma el contenido del archivo content.json. (Claves ordenadas, sin espacios en blanco y los nodos `sign` y` signers_sign`).

**Ejemplo**:
```json
  "signs": {
    "1TaLk3zM7ZRskJvrh3ZNCDVGXvkJusPKQ": "G6/QXFKvACPQ7LhoZG4fgqmeOSK99vGM2arVWkm9pV/WPCfc2ulv6iuQnuzw4v5z82qWswcRq907VPdBsdb9VRo="
  },
```


----


### zeronet_version

Versión de ZeroNet utilizada para generar el archivo content.json.viewport

**Ejemplo**: 0.2.5


---


# Ajustes


### background-color (color de fondo)

Color de fondo del envoltorio

**Example**: #F5F5F5


---


### cloneable

Permitir a clonar el sitio si ** true **.

Para hacer su sitio correctamente clonable usted tiene que agregar los archivos de datos para el comienzo limpio (por ejemplo, sin ninguna entrada del blog).
Para ello, tiene que agregar ** - default ** postfix a sus archivos de datos y directorios.
En el proceso de clonación, se omite cada archivo y directorio si tiene ** - default ** postfixed y entonces el postfix ** - default ** se eliminará de los archivos y directorios afectados.


---


### description

Descripción de su sitio, que se muestra en el título del sitio en ZeroHello.

**Ejemplo**: Demo del foro descentralizado


---


### domain

Nombre de dominio Namecoin de su sitio. ZeroHello enlazará a esto si el usuario tiene el complemento Zeroname habilitado.

**Ejemplo**: Blog.ZeroNetwork.bit




---


### ignore

Ignore los archivos de la firma que coinciden con este patrón preg

**Ejemplo**: `((js|css)/(?!all.(js|css))|data/users/.*)` (ignorar todos los archivos js y css excepto all.js y all.css, ademas de no añadir nada del directorio data/users/)


---


### includes

Incluir otro content.json

**Ejemplo**:

```json
{
  "data/users/content.json": {
    "signers": [ # Posibles direcciones de firmantes para el archivo
      "1LSxsKfC9S9TVXGGNSM3vPHjyW82jgCX5f"
    ],
    "signers_required": 1 # El * número * de Signos válidos necesarios para aceptar el archivo (posibilidad de Multisig)
    "files_allowed": "data.json", # Patrón Preg de los archivos permitidos en el archivo de inclusión
    "includes_allowed": false, # Anidados incluye permitido o no
    "max_size": 10000, # Tamaño de sum máximo permitido en el include (en bytes)
  }
}
```


---


### merged_type

Fuente de datos para el tipo de sitio merger especificado

**Ejemplo**: `ZeroMe`


---


### optional

Patrón Preg de archivos opcionales

**Ejemplo**: `(data/mp4/.*|updater/.*)` (Todo en data / mp4 y el directorio del actualizador es opcional)


---


### signs_required

El ** número ** de firmas válidas requeridas para aceptar el archivo (posibilidad de Multisig)

**Ejemplo**: 1


---


### title

Título del sitio, visible en el título del navegador y en ZeroHello.

**Ejemplo**: ZeroTalk


----


### translate

Los archivos necesitan ser traducidos. (Utilizar el lenguaje json en el diccionario `languages`)

**Ejemplo**: ["index.html", "js/all.js"]


----


### favicon

Favicon del sitio. Reemplace el favicon predeterminado de ZeroNet con un favicon específico del sitio.

**Ejemplo**: favicon.ico


----


### user_contents

Reglas del contenido de usuario permitido del directorio actual.

Nodo                   | Descripción
                  ---  | ---
**cert_signers**       | Dominios aceptados y sus direcciones de firmante válidas
**permission_rules**   | Nombres de archivos permitidos y tamaño total del directorio basado en el dominio cert o el método de autorización
**permissions**        | Permisos por usuario. (false = banned user)

**Ejemplo**:
```json
  "user_contents": {
    "cert_signers": {
      "zeroid.bit": [ "1iD5ZQJMNXu43w1qLB8sfdHVKppVMduGz" ]
    },
    "permission_rules": {
      ".*": {
        "files_allowed": "data.json",
        "max_size": 10000
      },
      "bitid/.*@zeroid.bit": { "max_size": 40000 },
      "bitmsg/.*@zeroid.bit": { "max_size": 15000 }
    },
    "permissions": {
      "bad@zeroid.bit": false,
      "nofish@zeroid.bit": { "max_size": 100000 }
    }
  }
```

----


### viewport

Contenido de la meta etiqueta de la ventana de visualización. (Utilizado para páginas amigables para móviles)

**Ejemplo**: width=device-width, initial-scale=1.0
