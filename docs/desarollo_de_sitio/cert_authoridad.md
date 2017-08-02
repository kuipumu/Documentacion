# Autoridad certificada

¿Una cuenta sin contraseña? ¿Un certificado para mí? Te das cuenta de que el sistema de identificación de ZeroNet no se ajusta a la convención. En esta sección, aprenderá cómo funciona el certificado de usuario y la autoridad de certificación en ZeroNet.

## ¿Qué hace una autoridad de certificación?

En ZeroNet, todo está firmado por las claves de firma de Bitcoin. Un certificado proporciona un nombre único y memorizable para una dirección de Bitcoin. Una autoridad de certificación (o un proveedor de ID) es responsable de probar la relación entre un nombre descriptivo único y una dirección de Bitcoin.

## Formato del certificado

### Cuerpo

El cuerpo de un certificado contiene una dirección Bitcoin, un tipo de portal y un nombre de usuario memorizable.

```
[Dirección Bitcoin]#[Tipo de Portal]/[Nombre de Usuario]
```

**Ejemplo:**

```
1H28iygiKXe3GUMcD77HiifVqtf3858Aft#web/hellozeronet
```

- Dirección Bitcoin: `1H28iygiKXe3GUMcD77HiifVqtf3858Aft`
- Tipo de Portal: `web`
- Nombre de Usuario: `hellozeronet`

**Reglas generales:**

La dirección de Bitcoin, el tipo de portal y el nombre de usuario ** no deben ** contener el carácter `#`, `@` o `/`

Sólo se permiten 0-9 y a-z en un nombre de usuario. Todas las letras en inglés en un nombre de usuario ** deben estar ** en minúsculas. Los caracteres que no están en el conjunto permitido ** no deben ser ** usados como partes de un nombre de usuario. Un nombre de usuario ** no debería ser ** demasiado largo. Un nombre de usuario ** debe ** ser legible y ** no debe ** interferir con la representación de interfaz de usuario.

Un nombre de usuario ** debe ser único en el grupo de todos los nombres de usuario registrados.

### Firma

Un algoritmo de firma de certificado carga una clave de firma secreta y genera una firma Bitcoin determinista para el cuerpo.

**Desde la fuente del código:**

```python
sign = os.popen("python zeronet.py --debug cryptSign %s#bitmsg/%s %s 2>&1" % (auth_address, user_name, config.site_privatekey)).readlines()[-1].strip()
```

### Certificado

Al mirar el código fuente de ZeroID, sabemos cómo se almacena un certificado en su base de datos pública.
```python
data["users"][user_name] = "bitmsg,%s,%s" % (auth_address, sign)
```

**Ejemplo:**

```
"hellozeronet": "web,1H28iygiKXe3GUMcD77HiifVqtf3858Aft,HA2A+iKekECD3hasrsN8IrR86BnXQ63kPH+9A85JLO9hLUpRJTBn62UfnuuF92B9CIc6+EewAIqzIn9UoVq2LPA="
```

Un certificado se puede almacenar en varios formatos. Sin embargo, todos los formatos deben incluir:

- La dirección Bitcoin: `1H28iygiKXe3GUMcD77HiifVqtf3858Aft`
- El tipo de portal: `web`
- El nombre de usuario: `hellozeronet`
- La firma de la autoridad: `HA2A+iKekECD3hasrsN8IrR86BnXQ63kPH+9A85JLO9hLUpRJTBn62UfnuuF92B9CIc6+EewAIqzIn9UoVq2LPA=`

## El uso en `content.json`

Los propietarios de sitios pueden elegir las autoridades de certificados en las que confiar.

El Blue Hub, por ejemplo, acepta certificados firmados por ZeroID. Esta regla se define en sus ficheros `data / users / content.json`
- El proveedor de ID tiene un nombre descriptivo: `zeroid.bit`
- El resumen de clave pública del proveedor de ID es:: `1iD5ZQJMNXu43w1qLB8sfdHVKppVMduGz`

```json
"user_contents": {
  "cert_signers": {
   "zeroid.bit": [
    "1iD5ZQJMNXu43w1qLB8sfdHVKppVMduGz"
   ]
  }
}
```

Cada usuario presenta su certificado en el archivo de manifiesto en su carpeta Bitcoin. Por ejemplo, `data / users / 1J3rJ8ecnwH2EPYa6MrgZttBNc61ACFiCj / content.json` dice:

```json
{
 "address": "1BLueGvui1GdbtsjcKqCf4F67uKfritG49",
 "cert_auth_type": "web",
 "cert_sign": "HPiZsWEJ5eLnspUj8nQ75WXbSanLz0YhQf5KJDq+4bWe6wNW98Vv9PXNyPDNu2VX4bCEXhRC65pS3CM7cOrjjik=",
 "cert_user_id": "nofish@zeroid.bit",
 "files": {
  "data.json": {
   "sha512": "8e597412a2bc2726ac9a1ee85428fb3a94b09f4e7a3f5f589119973231417b15",
   "size": 21422
  }
 },
 "inner_path": "data/users/1J3rJ8ecnwH2EPYa6MrgZttBNc61ACFiCj/content.json",
 "modified": 1492458379,
 "signs": {
  "1J3rJ8ecnwH2EPYa6MrgZttBNc61ACFiCj": "G8kaZIGAstsiWLVY20e2ogJQi4OO+QuwqJ9GTj3gz7YleST/jst7RQH7hDn0uf8BJMBjFs35H3LPhNHHj4jueh8="
 }
}
```

Sitio específico:

- URL esperada del sitio: `"address": "1BLueGvui1GdbtsjcKqCf4F67uKfritG49"`
- Ruta de archivo esperada:: `"inner_path": "data/users/1J3rJ8ecnwH2EPYa6MrgZttBNc61ACFiCj/content.json"`

Información del certificado:

- Proveedor de ID: `zeroid.bit`
- Nombre de usuario: `nofish`
- Dirección de usuario Bitcoin: `1J3rJ8ecnwH2EPYa6MrgZttBNc61ACFiCj`
- Tipo de portal: `web`
- Firma del proveedor de ID: `HPiZsWEJ5eLnspUj8nQ75WXbSanLz0YhQf5KJDq+4bWe6wNW98Vv9PXNyPDNu2VX4bCEXhRC65pS3CM7cOrjjik=`

### El proceso de verificación

1. El algoritmo de verificación dice `data / users / content.json` para determinar cuál es el sitio esperado para el contenido del usuario.

2. El algoritmo de verificación dice `data / users / content.json` para buscar el resumen de claves públicas del proveedor de ID.

3. Dado un usuario Bitcoin dirección, un tipo de portal y un nombre de usuario, el algoritmo de verificación reconstruye el cuerpo del certificado.

4. El algoritmo de verificación comprueba la firma del proveedor de ID, con la clave pública definida en `data / users / content.json`, para asegurar la autenticidad del cuerpo del certificado.

5. El algoritmo de verificación carga la clave pública del usuario y comprueba la autenticidad del contenido del usuario.

## Características y limitaciones de las autoridades de certificación

- Una autoridad de certificación proporciona nombres memorizables para los resúmenes de clave pública del usuario. También ayuda a mitigar el spam y el contenido no solicitado.

- Un usuario no tiene que dar información secreta como contraseñas. Además, un usuario sólo tiene que autenticar una vez.

- Una autoridad de certificación no tiene que ser aprobada por ningún desarrollador de ZeroNet. El propietario de un sitio puede elegir las autoridades de certificación que deben confiar en calidad de contenido de usuario.

- Una autoridad de certificación es responsable de mantener su grupo de nombres de usuario.

- ZeroID no revoca ni renueva los certificados.

## ¿Puedo vivir sin autoridades de certificación?

Generalmente, se requiere un certificado cuando agrega cosas al sitio de otra persona. No necesita un certificado cuando está modificando su propio sitio.
