# Estructura del dbschema.json


[Ejemplo del archivo dbschema.json original](https://github.com/HelloZeroNet/ZeroTalk/blob/master/dbschema.json)

[Versión original de esta pagina](https://zeronet.readthedocs.io/en/latest/site_development/dbschema_json/)

El siguiente código es una traducción del ejemplo original en ingles, sigue el mismo procedimiento pero con los nombres de las funciones traducidas a excepción de los archivos data.json y content.json, para un mejor entendimiento para aquellos que no dominan bien el ingles.

Al usar este código en ZeroNet necesitas utilizarlo con las funciones en ingles como están en el archivo original, de otro modo estos no funcionaran, así que solo usa este ejemplo como referencia para traducir los términos.

El siguiente código hará lo siguiente:

 - Si se recibe un archivo datos / usuarios / * / data.json actualizado (por ejemplo: un usuario ha publicado algo):

  - Todas las filas de `data [" tópicos "]` se cargaran en la tabla `tópico`
  - Cada clave de `data ['votos_comentario']` se cargaran en la tabla `voto_comentario` como col` hash_comentario` y los valores almacenados en la misma línea que `voto`

 - Si se recibe un archivo datos /usuarios /content.json actualizado (por ejemplo: nuevo usuario creado):
   - La clave "id_usuario", "nombre_usuario", "tamaño_max", "añadido" en el valor de `contenido [" incluidos "]` se cargaran en la tabla `usuario` y la clave se almacenara como` dirección`


```json

{
  "nombre_db": "ZeroTalk", # Nombre de la base de datos (sólo se utiliza para depurar)
  "archivo_db": "data/usuarios/zerotalk.db", # Archivo de base de datos relativo al directorio del sitio
  "versión": 2, # 1 = La tabla Json tiene una columna de ruta de acceso que incluye el directorio y nombre del archivo
                # 2 = La tabla Json tiene una columna separada del directorio y nombre de archivo
                # 3 = Igual que la versión 2, pero también tiene columna del sitio (para sitios merger)
  "mapas": { # Json a asignaciones de base de datos
    ".*/data.json": { # Patrón Regex de archivo en relación con archivo de base de datos
      "a_tabla": [ # Cargar valores en la tabla
        {
          "nodo": "tópicos", # Lectura de datos.json del valor clave [tópicos]
          "tabla": "tópico" # Alimentando los datos a la table de topico
        },
        {
          "nodo": "votos_comentario", # Lectura de datos.json del valor clave [votos_comentario]
          "tabla": "voto_comentario", # Alimentando los datos a la table de voto_comentario
          "clave_col": "hash_comentario",
          # Data.json [votos_comentario] es un dict simple, las claves de la
             # Dict se cargan en la columna voto_comentario de la columna hash_comentario

          "val_col": "voto"
          # Los datos de data.json [votos_comentario] dictados en la columna de voto de la columna voto_comentario

        }
      ],
      "a_valor_clave": ["siguiente_mensaje_id", "siguiente_tópico_id"]
        # Cargar data.json[siguiente_tópico_id] a la tabla de valores clave
        # (clave: siguiente_mensaje_id, valor: data.json [siguiente_mensaje_id])

    },
    "content.json": {
      "a_tabla": [
        {
          "nodo": "incluidos",
          "tabla": "usuario",
          "clave_col": "dirección",
          "importar_cols": ["id_usuario", "nombre_usuario", "tamaño_max", "añadido"],
            # Sólo importar estas columnas a la tabla de usuario
          "reemplaza": {
            "dirección": {"content.json": "data.json"}
            # Reemplazar content.json a data.json en el
            # Valor de la columna de ruta (necesario para unirse)
          }
        }
      ],
      "a_tabla_json": [ "tipo_certificado_auth", "id_cert_usuario" ]  # Guardar tipo_certificado_auth y id_cert_usuario directamente a la tabla json (consultas de datos más fáciles y rápidas)
    }
  },
  "tablas": { # Definiciones de tablas
    "tópico": { # Definir la tabla topico
      "cols": [ # Cols de la table
        ["id_tópico", "INTEGRAL"],
        ["titulo", "TEXTO"],
        ["cuerpo", "TEXTO"],
        ["tipo", "TEXTO"],
        ["has_tópico_padre", "TEXTO"],
        ["añadido", "FECHAHORA"],
        ["id_json", "INTEGRAL REFERENCIAS json (id_json)"]
      ],
      "indices": ["CREAR ÚNICO INDICE clave_tópico EN tópico(id_tópico, json_id)"],
        # Índices creados automáticamente

      "esquema_cambiado": 1426195822
        # La última vez que cambió el esquema, si la versión del cliente es diferente entonces
        # Destruir automáticamente el antiguo, crear la nueva tabla y volver a cargar los datos en él

    },
    "voto_comentario": {
      "cols": [
        ["hash_comentario", "TEXTO"],
        ["voto", "INTEGRAL"],
        ["id_json", "INTEGRAL REFERENCIAS json (id_json)"]
      ],
      "indices": ["CREAR ÚNICO INDICE clave_voto_comentario EN voto_comentario(hash_comentario, id_json)", "CREAR INDICE hash_voto_comentario EN voto_comentario(hash_comentario)"],
      "esquema_cambiado": 1426195826
    },
    "usuario": {
      "cols": [
        ["id_usuario", "INTEGRAl"],
        ["nombre_usuario", "TEXTO"],
        ["tamaño_max", "INTEGRAl"],
        ["dirección", "TEXTO"],
        ["añadido", "INTEGRAl"],
        ["id_json", "INTEGRAl REFERENCIAS json (id_json)"]
      ],
      "indices": ["CREAR ÚNICO INDICE id_usuario EN usuario(id_usuario)", "CREAR ÚNICO INDICE dirección_usuario EN usuario(dirección)"],
      "esquema_cambiado": 1426195825
    },
    "json": {  # El formato de tabla Json sólo es necesario si ha especificado un patrón a_tabla_json en cualquier lugar
        "cols": [
            ["id_json", "INTEGRAL PRIMARIO LLAVE AUTO-INCREMENTAR"],
            ["directorio", "TEXTO"],
            ["nombre_archivo", "TEXTO"],
            ["tipo_certificado_auth", "TEXTO"],
            ["id_cert_usuario", "TEXTO"]
        ],
        "indices": ["CREAR UNICO INDICE dirección EN json(directorio, sitio, nombre_archivo)"],
        "esquema_cambiado": 4
    }
  }
}
```

## Ejemplo de archivo data.json
```json
{
  "id_siguiente_tópico": 2,
  "tópicos": [
    {
      "id_tópico": 1,
      "titulo": "NuevoTópico",
      "cuerpo": "¡Tópico!",
      "añadido": 1426628540,
      "hash_tópico_padre": "5@2"
    }
  ],
  "id_siguiente_mensaje": 19,
  "comentarios": {
    "1@2": [
      {
        "id_comentario": 1,
        "cuerpo": "¡Prueba de nuevo usuario!",
        "añadido": 1423442049
      }
    ],
    "1@13": [
      {
        "id_comentario": 2,
        "cuerpo": "hola",
        "añadido": 1424653288
      },
      {
        "id_comentario": 13,
        "cuerpo": "prueba 123",
        "añadido": 1426463715
      }
    ]
  },
  "votos_tópico": {
    "1@2": 1,
    "4@2": 1,
    "2@2": 1,
    "1@5": 1,
    "1@6": 1,
    "3@2": 1,
    "1@13": 1,
    "4@5": 1
  },
  "votos_comentario": {
    "5@5": 1,
    "2@12": 1,
    "1@12": 1,
    "33@2": 1,
    "45@2": 1,
    "12@5": 1,
    "34@2": 1,
    "46@2": 1
  }
}
```

## Ejemplo de archivo content.json

```json
{
  "archivos": {},
  "ignorar": ".*/.*",
  "incluidos": {
    "13v1FwKcq7dx2UPruFcRcqd8s7VBjvoWJW/content.json": {
      "añadido": 1426683897,
      "archivos_permitidos": "data.json",
      "incluidos_permitidos": false,
      "tamaño_max": 10000,
      "firmantes": [
        "13v1FwKcq7dx2UPruFcRcqd8s7VBjvoWJW"
      ],
      "firmantes_requerido": 1,
      "id_usuario": 15,
      "nombre_usuario": "ejemplousuario#1"
    },
    "15WGMVViswrF13sAKb7je6oX3UhXavBxxQ/content.json": {
      "añadido": 1426687209,
      "archivos_permitidos": "data.json",
      "incluidos_permitidos": false,
      "tamaño_max": 10000,
      "firmantes": [
        "15WGMVViswrF13sAKb7je6oX3UhXavBxxQ"
      ],
      "firmantes_requerido": 1,
      "id_usuario": 15,
      "user_name": "ejemplousuario#2"
    }
  }
}
```
