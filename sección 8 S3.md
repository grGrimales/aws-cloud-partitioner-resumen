# Clase número 73

1. Amazon S3 es un servicio esencial para almacenamiento de archivos, ampliamente utilizado por páginas web y servicios AWS para integración y almacenamiento.
2. Se utiliza para respaldos regionales, permitiendo almacenar copias en distintas regiones para evitar pérdidas por fallos regionales.
3. Permite almacenar datos históricos en forma económica con S3 Glacier para archivado a largo plazo, como hace Nasdaq al guardar 7 años de datos.
4. Amazon S3 facilita almacenamiento híbrido, combinando datos en la nube y locales.
5. Es clave para análisis de datos y extracción de insights empresariales, ejemplo práctico es la empresa Sysco.
6. Útil para alojar contenido estático, incluyendo sitios web y aplicaciones web completas.
7. Amazon S3 organiza archivos en buckets (contenedores) únicos.
7.1. Los buckets tienen nombres globalmente únicos en AWS.
8. El nombre del bucket debe tener entre 3 y 63 caracteres, sin mayúsculas ni guiones bajos, no debe ser una IP, y debe empezar con letra o número.
9. Los buckets se definen en una región específica aunque sus nombres son únicos globalmente en AWS.
10. Todos los archivos almacenados en S3 tienen una estructura de "objetos" con una clave (`key`) única.
11. Las claves representan rutas completas, con prefijo y nombre del objeto.
11. Ejemplo de clave simple:
```plaintext
mi-archivo.txt
```
11. Ejemplo de clave anidada:
```plaintext
carpeta1/subcarpeta/mi-archivo.txt
```
12. Las claves son simples cadenas de texto largas que simulan carpetas mediante el uso del carácter slash (`/`).
13. Un objeto en Amazon S3 se compone de su contenido (cuerpo o `body`) y metadatos.
14. Los metadatos son pares clave-valor que pueden incluir información adicional del archivo.
    ```json
    {
      "Author": "Juan",
      "Project": "AWS"
    }
    ```
15. Si un archivo supera los 5 GB, es obligatorio usar la función `multi-part upload`, dividiendo el archivo en varias partes para su subida.

```javascript
// Ejemplo simplificado de multipart upload con AWS SDK para JS
const AWS = require('aws-sdk');
const s3 = new AWS.S3();

const params = {
  Bucket: 'mi-bucket',
  Key: 'archivo-grande.zip'
};

s3.createMultipartUpload(params, (err, data) => {
  if (err) console.log(err);
  else console.log('Multipart iniciado:', data.UploadId);
});
```

15. Cada parte en un multipart upload puede tener hasta 5 GB, siendo el tamaño máximo de objeto total 5 TB.
16. Se pueden agregar metadatos y activar versionado en objetos para conservar distintas versiones automáticamente.
17. El versionado permite gestionar varias versiones de un mismo objeto, facilitando su recuperación y auditoría.# Clase número 73

## Resumen en formato de viñetas numeradas:

1. Amazon S3 es un servicio esencial para almacenamiento de archivos, ampliamente utilizado por páginas web y servicios AWS para integración y almacenamiento.
2. Se utiliza para respaldos regionales, permitiendo almacenar copias en distintas regiones para evitar pérdidas por fallos regionales.
3. Permite almacenar datos históricos en forma económica con S3 Glacier para archivado a largo plazo, como hace Nasdaq al guardar 7 años de datos.
4. Amazon S3 facilita almacenamiento híbrido, combinando datos en la nube y locales.
5. Es clave para análisis de datos y extracción de insights empresariales, ejemplo práctico es la empresa Sysco.
6. Útil para alojar contenido estático, incluyendo sitios web y aplicaciones web completas.
7. Amazon S3 organiza archivos en buckets (contenedores) únicos.
7.1. Los buckets tienen nombres globalmente únicos en AWS.
8. El nombre del bucket debe tener entre 3 y 63 caracteres, sin mayúsculas ni guiones bajos, no debe ser una IP, y debe empezar con letra o número.
9. Los buckets se definen en una región específica aunque sus nombres son únicos globalmente en AWS.
10. Todos los archivos almacenados en S3 tienen una estructura de "objetos" con una clave (`key`) única.
11. Las claves representan rutas completas, con prefijo y nombre del objeto.
11. Ejemplo de clave simple:
```plaintext
mi-archivo.txt
```
11. Ejemplo de clave anidada:
```plaintext
carpeta1/subcarpeta/mi-archivo.txt
```
12. Las claves son simples cadenas de texto largas que simulan carpetas mediante el uso del carácter slash (`/`).
13. Un objeto en Amazon S3 se compone de su contenido (cuerpo o `body`) y metadatos.
14. Los metadatos son pares clave-valor que pueden incluir información adicional del archivo.
    ```json
    {
      "Author": "Juan",
      "Project": "AWS"
    }
    ```
15. Si un archivo supera los 5 GB, es obligatorio usar la función `multi-part upload`, dividiendo el archivo en varias partes para su subida.

```javascript
// Ejemplo simplificado de multipart upload con AWS SDK para JS
const AWS = require('aws-sdk');
const s3 = new AWS.S3();

const params = {
  Bucket: 'mi-bucket',
  Key: 'archivo-grande.zip'
};

s3.createMultipartUpload(params, (err, data) => {
  if (err) console.log(err);
  else console.log('Multipart iniciado:', data.UploadId);
});
```

15. Cada parte en un multipart upload puede tener hasta 5 GB, siendo el tamaño máximo de objeto total 5 TB.
16. Se pueden agregar metadatos y activar versionado en objetos para conservar distintas versiones automáticamente.
17. El versionado permite gestionar varias versiones de un mismo objeto, facilitando su recuperación y auditoría.

# 74. S3 Hands On

1. Amazon S3 permite crear contenedores llamados buckets para almacenar archivos en la nube, seleccionando una región específica para almacenarlos.

2. Los buckets deben tener nombres únicos a nivel global en AWS, no pueden contener mayúsculas ni guiones bajos, y deben iniciar con número o letra minúscula, por ejemplo:  
`mi-bucket-demo-2024`.

3. Se recomienda mantener máxima seguridad restringiendo el acceso público a los buckets, permitiendo únicamente al dueño cargar y gestionar archivos.

4. El versionado de buckets puede activarse o desactivarse para manejar diferentes versiones de un objeto automáticamente, facilitando su recuperación.

5. Amazon S3 permite utilizar cifrado del lado del servidor, gestionado directamente por Amazon (Server-side Encryption) para proteger automáticamente los archivos subidos.

6. Cada archivo subido se denomina objeto, y cada objeto posee una clave (key), representando la ruta completa dentro del bucket, simulando una estructura de carpetas.

7. Un objeto en S3 tiene contenido (archivo en sí) y metadatos asociados en formato clave-valor, útiles para gestionar y categorizar archivos fácilmente.

8. Ejemplo de una clave simple en un bucket:
```text
foto.png
```

9. Ejemplo de una clave con estructura de carpetas simulada:
```text
fotos/viajes/italia.jpg
```

10. Aunque la interfaz de S3 simula carpetas, realmente se trata de claves largas que incluyen barras `/` como separadores.

11. Amazon S3 admite archivos grandes con un tamaño máximo de hasta 5 terabytes (TB).

10. Archivos superiores a 5 gigabytes (GB) deben subirse mediante multipart upload, dividiendo automáticamente los archivos en partes manejables.

11. Ejemplo simplificado para multipart upload con AWS SDK en JavaScript:
```javascript
const AWS = require('aws-sdk');
const s3 = new AWS.S3();

const params = {
  Bucket: 'mi-bucket',
  Key: 'archivo-grande.iso'
};

s3.createMultipartUpload(params, (error, data) => {
  if (error) console.log('Error:', error);
  else console.log('Multipart upload iniciado:', data.UploadId);
});
```

12. La URL pública de un objeto S3 generalmente no está disponible a menos que explícitamente se habilite el acceso público, por defecto aparece "Access Denied".

13. Para acceder a objetos privados se generan URLs firmadas ("Signed URLs"), que incluyen credenciales del propietario codificadas y permiten acceso temporal seguro.

14. Ejemplo de URL firmada usando AWS SDK para JavaScript:
```javascript
const AWS = require('aws-sdk');
const s3 = new AWS.S3();
const url = s3.getSignedUrl('getObject', {
  Bucket: 'mi-bucket',
  Key: 'documentos/confidencial.pdf',
  Expires: 60 // URL válida por 60 segundos
});
console.log(url);
```

14. Las "carpetas" en S3 no existen físicamente; son prefijos en la clave del objeto, aunque la interfaz de usuario muestra una estructura similar a sistemas como Google Drive o Dropbox.

15. Se puede crear una carpeta virtual simplemente asignando claves con prefijos separados por slashes (`/`), lo que organiza visualmente los objetos.

15. Ejemplo de estructura con prefijos en S3:
```plaintext
facturas/2024/enero/factura-01.pdf
```

15. S3 maneja automáticamente la creación y visualización de estas "carpetas" según los prefijos en las claves de objetos.

16. Es posible eliminar fácilmente objetos individuales o carpetas completas (eliminando todos los objetos que contienen).

17. Ejemplo de código para eliminar un objeto específico con AWS SDK en JavaScript:
```javascript
const params = {
  Bucket: 'mi-bucket',
  Key: 'carpeta/archivo.txt'
};
s3.deleteObject(params, (err, data) => {
  if (err) console.log('Error al eliminar', err);
  else console.log('Objeto eliminado con éxito');
});
```

18. Amazon S3 proporciona opciones de cifrado gestionado por S3 (Server-Side Encryption), recomendables para mantener la seguridad de los datos almacenados.

19. Cada bucket se asocia a una región específica, aunque visualmente aparezcan agrupados en una misma interfaz a nivel global.


# 75. S3 Hands On: Seguridad y Políticas de Bucket

1. Amazon S3 maneja seguridad mediante políticas de bucket (Bucket Policies), aplicadas directamente en la consola para otorgar o restringir accesos.
2. Las políticas de bucket permiten conceder acceso público, acceso cruzado entre cuentas, o forzar el cifrado en la carga de objetos.
3. Las políticas de bucket se escriben en formato JSON, facilitando su lectura y mantenimiento.
4. Un bucket policy típico para permitir acceso público podría ser:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PermitirAccesoPublico",
      "Effect": "Allow",
      "Principal": "*",
      "Action": ["s3:GetObject"],
      "Resource": ["arn:aws:s3:::mi-bucket/*"]
    }
  ]
}
```
5. La propiedad `Principal` indica qué usuarios o cuentas pueden acceder; para acceso público se usa `"Principal": "*"`.
6. Es posible restringir acciones específicas usando "Effect": "Deny", lo que incrementa la seguridad del bucket.
7. Las políticas también permiten especificar exactamente qué acciones están permitidas, como `GetObject`, `PutObject`, o `DeleteObject`.
8. Para otorgar acceso a un usuario interno del mismo AWS account, es mejor utilizar permisos IAM individuales, no bucket policies.
9. Ejemplo de política IAM para permitir que un usuario específico acceda a objetos en S3:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::mi-bucket/*"
    }
  ]
}
```
10. Para acceder a S3 desde una instancia EC2, se debe asignar un IAM role a la instancia con los permisos adecuados:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:ListBucket",
        "s3:GetObject"
      ],
      "Resource": [
        "arn:aws:s3:::mi-bucket",
        "arn:aws:s3:::mi-bucket/*"
      ]
    }
  ]
}
```
11. En el caso de acceso cross-account (acceso desde otra cuenta de AWS), es indispensable usar una política en el bucket que autorice explícitamente dicho acceso:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::123456789012:user/OtraCuentaUsuario"
      },
      "Action": ["s3:GetObject"],
      "Resource": "arn:aws:s3:::mi-bucket/*"
    }
  ]
}
```
12. Amazon S3 incluye configuraciones llamadas "Block Public Access" que impiden filtraciones accidentales de información confidencial.
13. La configuración "Block Public Access" prevalece sobre cualquier política que intente hacer público un bucket, incluso si explícitamente se configura una política pública.
14. Ejemplo de URL firmada (pre-signed URL), válida por 120 segundos, generada con AWS SDK para Node.js:
```javascript
const AWS = require('aws-sdk');
const s3 = new AWS.S3();

const params = {
  Bucket: 'mi-bucket',
  Key: 'archivo-privado.pdf',
  Expires: 120 // URL expira en 2 minutos
};

const urlFirmada = s3.getSignedUrl('getObject', params);
console.log('URL firmada:', urlFirmada);
```
14. Las URLs firmadas permiten acceso temporal y seguro a objetos privados sin necesidad de hacer público el bucket.
15. Para mantener la seguridad y cumplir regulaciones, es recomendable mantener "Block Public Access" activado por defecto a nivel del bucket o incluso a nivel de cuenta.
16. El cifrado server-side (SSE) en Amazon S3 permite que los objetos se almacenen cifrados automáticamente con claves gestionadas por AWS:
```json
{
  "Rules": [
    {
      "ApplyServerSideEncryptionByDefault": {
        "SSEAlgorithm": "AES256"
      }
    }
  ]
}
```
17. Es recomendable activar SSE en todos los buckets nuevos como medida estándar para aumentar la protección de datos sensibles.
18. Al borrar objetos o carpetas completas de un bucket, el borrado es permanente, por lo que debe hacerse con precaución.
19. La combinación de políticas estrictas de acceso, bloqueo de acceso público y cifrado es la mejor práctica para garantizar la seguridad integral en Amazon S3.

# 76. S3 Security: Bucket Policy Hands On

1. Las políticas de bucket en Amazon S3 permiten gestionar el acceso público o restringido a objetos específicos almacenados en un bucket.
2. Para hacer públicos los objetos, es necesario crear una política específica en formato JSON.
3. El primer paso consiste en modificar la configuración del bucket en la pestaña de Permisos, desactivando "Block Public Access" (con precaución debido a la seguridad).
4. Amazon ofrece una herramienta llamada AWS Policy Generator para facilitar la creación de políticas sin necesidad de escribir código directamente.
5. La política generada debe contener información específica como el ARN del bucket al que se aplicará la política y las acciones permitidas.
6. Ejemplo básico para permitir el acceso público de lectura (`GetObject`) a todos los objetos dentro de un bucket:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicRead",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::nombre-de-mi-bucket/*"
    }
  ]
}
```
5. El ARN (Amazon Resource Name) del bucket siempre debe terminar en `/*` para aplicar a todos los objetos dentro del bucket.
6. Tras aplicar correctamente la política, todos los objetos del bucket son visibles públicamente desde su URL directa.
7. Para acceder públicamente a un objeto, basta con copiar y pegar la URL del objeto proporcionada en la interfaz de S3.
8. Las políticas son inmediatamente efectivas tras guardarlas, sin necesidad de reiniciar o reconfigurar el bucket.
9. Las políticas públicas deben aplicarse con mucha cautela para evitar exposición involuntaria de datos sensibles.
10. Si no se habilita la política pública correctamente, la URL directa dará un error de "Access Denied".
11. Es posible utilizar políticas para restringir accesos específicos desde ciertas cuentas AWS, usando el campo `Principal`.
12. Ejemplo de política para permitir únicamente a un usuario específico acceder a objetos de un bucket:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowSpecificUserRead",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::111122223333:user/Juan"
      },
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::mi-bucket/*"
    }
  ]
}
```
13. Para máxima seguridad, se recomienda mantener activada la configuración "Block Public Access" excepto cuando sea absolutamente necesario desactivarla.
14. La configuración "Block Public Access" prevalece sobre cualquier política del bucket, incluso si se especifica una política pública.
15. La combinación de políticas bien definidas y configuraciones de acceso público adecuadas garantiza un almacenamiento seguro y eficiente en Amazon S3.

# 77. Hosting de Sitios Web Estáticos con S3: Práctica

1. Amazon S3 permite alojar páginas web estáticas utilizando archivos HTML, CSS, JavaScript e imágenes.

2. Para habilitar un sitio web estático en Amazon S3 primero es necesario activar la opción de "static website hosting" en las propiedades del bucket.

3. El bucket utilizado debe contener al menos un archivo index.html que será cargado automáticamente al visitar la URL del sitio web.

4. Ejemplo básico del contenido del archivo `index.html`:
```html
<!DOCTYPE html>
<html>
<body>
  <h1>¡Hola Mundo!</h1>
  <img src="coffee.jpg" alt="Café delicioso">
</body>
</html>
```

5. Para activar el hosting estático, en la consola AWS S3, se edita la sección "Static Website Hosting" en las propiedades del bucket, habilitando dicha opción.

6. Tras activar la opción de hosting estático, Amazon S3 proporcionará automáticamente una URL pública para acceder al contenido del bucket:
```
http://mi-bucket.s3-website-us-east-1.amazonaws.com
```

7. Es requisito indispensable que los objetos dentro del bucket (incluyendo archivos HTML, CSS, imágenes) tengan permisos de lectura pública mediante la política del bucket.

8. Al subir archivos adicionales al bucket (por ejemplo imágenes), estos se visualizarán correctamente siempre y cuando estén referenciados correctamente en el HTML:
```html
<img src="beach.jpg" alt="Imagen de playa">
```

8. Amazon S3 no soporta sitios web con código dinámico del lado del servidor, solo estáticos como HTML/CSS puro o JavaScript frontend.

9. La activación del hosting estático en Amazon S3 es inmediata tras habilitar la opción y establecer la política de bucket pública correspondiente.

10. Ejemplo de comando CLI para subir archivos desde local al bucket:
```bash
aws s3 cp index.html s3://mi-bucket/
aws s3 cp coffee.jpg s3://mi-bucket/
```

11. Aunque S3 admite rutas y estructura visual similar a directorios, todas son simplemente claves (`keys`) que definen rutas virtuales.

12. Amazon S3 ofrece una solución económica y escalable para alojar sitios web estáticos con alta disponibilidad sin necesidad de administrar servidores.

# 79. Introducción al Versionado en Amazon S3

1. Amazon S3 ofrece la funcionalidad de versionado, permitiendo almacenar múltiples versiones de un mismo objeto dentro de un bucket.

2. Al activar el versionado, cada vez que se actualiza o sobrescribe un archivo, S3 mantiene las versiones anteriores sin borrarlas.

3. Versionar objetos protege contra eliminaciones accidentales, ya que los objetos borrados pueden restaurarse desde versiones previas fácilmente.

4. Si se intenta eliminar un objeto con el versionado activado, S3 no elimina el objeto, sino que coloca un marcador de eliminación, permitiendo restaurarlo posteriormente.

5. La versión de un archivo que existe antes de habilitar el versionado se etiqueta con el valor `null`.

6. Suspender el versionado en un bucket no elimina versiones anteriores, únicamente detiene la creación de nuevas versiones.

7. Ejemplo de activación del versionado mediante AWS CLI:
```bash
aws s3api put-bucket-versioning \
  --bucket mi-bucket \
  --versioning-configuration Status=Enabled
```

8. Una vez activado, S3 asigna automáticamente un identificador único a cada versión del objeto subido.

9. Ejemplo de cómo subir objetos y versionarlos utilizando AWS CLI:
```bash
aws s3 cp archivo.txt s3://mi-bucket/
aws s3 cp archivo-actualizado.txt s3://mi-bucket/archivo.txt
```

10. Se recomienda mantener siempre activado el versionado para facilitar auditorías, recuperación ante errores humanos, o análisis histórico.

11. Un objeto cargado antes de activar el versionado tendrá su "Version ID" marcado como `null`.

12. Al desactivar temporalmente (suspender) el versionado, se mantiene el historial existente, garantizando que no se pierdan datos ya almacenados.

13. Ejemplo de consulta de todas las versiones de un objeto con AWS CLI:
```bash
aws s3api list-object-versions --bucket mi-bucket --prefix archivo.txt
```

# Vídeo número 78. S3 Website Hands On

1. Amazon S3 permite alojar sitios web estáticos utilizando HTML, CSS, JavaScript y recursos multimedia (imágenes, videos).

2. Para habilitar un bucket como hosting web, se accede a la pestaña "Propiedades" en Amazon S3 y se activa la opción "Static Website Hosting".

3. Es obligatorio especificar un documento índice (por ejemplo, `index.html`), que será la página principal del sitio web.

4. Ejemplo sencillo del archivo `index.html`:
```html
<!DOCTYPE html>
<html>
<body>
  <h1>Bienvenidos a mi web en S3</h1>
  <img src="coffee.jpg" alt="Taza de café">
</body>
</html>
```

5. La URL generada automáticamente por S3 para acceder al sitio tiene este formato:
```
http://nombre-bucket.s3-website-region.amazonaws.com
```

6. Es imprescindible configurar el bucket con acceso público (mediante política de bucket) para que los archivos puedan ser visualizados por los usuarios.

7. Ejemplo sencillo de política para acceso público en un bucket web:
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": "*",
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::mi-bucket-web/*"
  }]
}
```

8. El archivo definido como "índice" es el que se carga automáticamente al visitar la URL principal del sitio (por ejemplo, `index.html`).

9. Es obligatorio subir al bucket el archivo definido como índice, por ejemplo, `index.html`, para evitar errores de visualización.

10. Tras subir el archivo índice y otros recursos al bucket, estos son inmediatamente visibles al acceder mediante la URL del sitio web proporcionada por Amazon.

11. Al habilitar el alojamiento web estático en S3, el contenido debe ser público; de lo contrario, aparecerá el error HTTP 403 ("Acceso denegado").

12. Amazon S3 no admite ejecución de código del lado del servidor (PHP, Python, Node.js); solamente admite sitios web estáticos.

13. Ejemplo de carga de contenido al bucket usando AWS CLI:
```bash
aws s3 cp index.html s3://nombre-bucket-web/
aws s3 cp coffee.jpg s3://mi-bucket-web/
```

13. Amazon S3 ofrece una solución económica y escalable para alojar sitios web estáticos, simplificando la gestión y eliminando la necesidad de administrar servidores web.



# 80. S3 Versioning Hands On

1. La configuración de versionado en S3 permite mantener un historial de cambios en los objetos de un bucket, evitando la pérdida permanente de archivos al sobrescribirlos.

2. Para activar el versionado en un bucket de S3, es necesario acceder a la sección de propiedades, editar la configuración y habilitar la opción de versionado.

3. Una vez habilitado el versionado, cualquier archivo que se suba nuevamente al bucket con el mismo nombre no sobrescribe el original, sino que crea una nueva versión del archivo.

4. Cada objeto en un bucket versionado posee un identificador de versión (Version ID). Los objetos subidos antes de activar el versionado tienen un identificador de versión nulo (null).

5. Al sobrescribir un archivo en S3 con versionado habilitado, se genera una nueva versión y se mantiene la versión anterior, permitiendo restauraciones.

6. Es posible visualizar las versiones de los archivos activando la opción "Show versions" en la interfaz de AWS S3, lo que muestra todas las versiones almacenadas.

7. Para revertir un archivo a una versión anterior, se puede eliminar la versión más reciente del archivo desde la interfaz de S3, restaurando la versión anterior.

8. La eliminación de una versión específica de un archivo es una operación permanente y no reversible, lo que significa que una vez eliminada, no se puede recuperar.

9. Si se deshabilita "Show versions" y se elimina un archivo sin especificar una versión, AWS S3 crea un "delete marker", en lugar de eliminar realmente el archivo.

10. Un delete marker actúa como una versión especial del objeto que oculta todas las versiones anteriores sin eliminarlas físicamente del bucket.

11. Si un archivo con un delete marker es solicitado, el usuario recibirá un error "404 Not Found", ya que la versión visible del objeto ha sido ocultada.

12. Para restaurar un archivo oculto por un delete marker, se debe eliminar el delete marker desde la opción de "Show versions", restaurando así la versión previa del archivo.

13. La eliminación de un delete marker no afecta a las versiones anteriores del archivo, simplemente permite que la versión más reciente vuelva a ser visible en el bucket.

14. Se pueden gestionar múltiples versiones de un archivo en S3 sin afectar su URL pública, ya que cada versión tiene su propio identificador único.

15. Para forzar una actualización en un navegador después de revertir un archivo, se puede utilizar la combinación de teclas `Command + Shift + R` en Mac o `Ctrl + Shift + R` en Windows.

16. La función de versionado permite experimentar con cambios en archivos sin riesgo de perder datos, facilitando la recuperación de versiones anteriores cuando sea necesario.

17. La implementación de versionado en S3 es útil para mantener la integridad de los datos en aplicaciones web y sistemas de almacenamiento, permitiendo revertir cambios rápidamente sin afectar la operación general.

**Ejemplo de código para habilitar versionado en S3 con AWS SDK para Python (boto3):**

```python
import boto3

s3 = boto3.client('s3')
bucket_name = 'mi-bucket-versionado'

# Habilitar versionado en el bucket
s3.put_bucket_versioning(
    Bucket=bucket_name,
    VersioningConfiguration={
        'Status': 'Enabled'
    }
)

print(f'Versionado habilitado en el bucket {bucket_name}')
```

**Ejemplo de código para listar versiones de un objeto en un bucket:**

```python
response = s3.list_object_versions(Bucket=bucket_name, Prefix='index.html')
for version in response.get('Versions', []):
    print(f"Versión ID: {version['VersionId']} - Ultima modificación: {version['LastModified']}")
```

**Ejemplo de eliminación de un delete marker para restaurar un archivo:**

```python
delete_markers = response.get('DeleteMarkers', [])
if delete_markers:
    version_id = delete_markers[0]['VersionId']
    s3.delete_object(Bucket=bucket_name, Key='coffee.jpg', VersionId=version_id)
    print("Delete marker eliminado, archivo restaurado.")
```

 # Video número 81. S3 Replication Overview

1. Amazon S3 Replication permite copiar objetos entre buckets de S3 de manera asincrónica, asegurando redundancia y disponibilidad de los datos en distintas regiones o cuentas.

2. Existen dos tipos de replicación en S3: **Cross-Region Replication (CRR)** y **Same-Region Replication (SRR)**. CRR replica los datos en una región diferente, mientras que SRR mantiene los datos en la misma región.

3. Para habilitar la replicación en S3, es obligatorio activar el **Versioning** tanto en el bucket de origen como en el de destino.

4. En la replicación CRR, los buckets deben estar en diferentes regiones de AWS, mientras que en SRR ambos buckets deben residir en la misma región.

5. Es posible replicar datos entre diferentes cuentas de AWS, lo que permite separar datos en entornos de producción y prueba o realizar backups en otra cuenta de seguridad.

6. La replicación en S3 ocurre de manera asincrónica, es decir, los datos no se copian de inmediato, sino que el proceso ocurre en segundo plano.

7. Para que la replicación funcione correctamente, es necesario asignar los permisos adecuados en IAM. El bucket de destino debe otorgar permisos de escritura al servicio de S3 del bucket de origen.

8. **Casos de uso de CRR:** Se utiliza para cumplir con requisitos de **compliance**, mejorar el acceso de baja latencia a los datos en otra región o replicar datos entre cuentas de AWS.

9. **Casos de uso de SRR:** Es útil para consolidar logs desde múltiples buckets en una sola ubicación, realizar pruebas con datos en un bucket separado o mantener sincronizados entornos de producción y prueba.

10. Al configurar la replicación, se pueden definir reglas para replicar solo ciertos prefijos o etiquetas de objetos, permitiendo un control granular sobre qué se replica.

11. La replicación de objetos no aplica de manera retroactiva. Solo los objetos creados o modificados después de habilitar la replicación serán copiados al bucket de destino.

12. Si se eliminan objetos en el bucket de origen, la eliminación **no se replica** de forma predeterminada. Se puede configurar la replicación para replicar eliminaciones si es necesario.

13. Se pueden monitorear y auditar las operaciones de replicación utilizando **Amazon CloudWatch**, **AWS CloudTrail** y **Amazon S3 Inventory**, que ayudan a verificar el estado y cumplimiento de la replicación.

14. Ejemplo de configuración de replicación en S3 mediante AWS CLI:

```sh
aws s3api put-bucket-replication \
  --bucket mi-bucket-origen \
  --replication-configuration '{
    "Role": "arn:aws:iam::123456789012:role/replication-role",
    "Rules": [
        {
            "Status": "Enabled",
            "Priority": 1,
            "DeleteMarkerReplication": {"Status": "Disabled"},
            "Filter": {},
            "Destination": {
                "Bucket": "arn:aws:s3:::mi-bucket-destino",
                "StorageClass": "STANDARD"
            }
        }
    ]
  }'
```
Este comando configura la replicación desde "mi-bucket-origen" hacia "mi-bucket-destino" utilizando la función de replicación de AWS S3.

# Video número 82. S3 Replication Hands On

1. Para configurar la replicación en Amazon S3, primero se debe crear un bucket de origen y un bucket de destino. El bucket de origen contendrá los datos iniciales y el bucket de destino recibirá las copias replicadas.

2. Es obligatorio habilitar el **Versioning** en ambos buckets para que la replicación funcione correctamente. Sin versionado, S3 no podrá mantener la coherencia de los objetos replicados.

3. Se pueden configurar buckets en la misma región para realizar una **Same-Region Replication (SRR)** o en regiones distintas para una **Cross-Region Replication (CRR)**. Esta última permite redundancia geográfica de los datos.

4. Para comenzar con la configuración, se sube un archivo al bucket de origen. Sin embargo, hasta que no se cree una regla de replicación, el archivo no se replicará automáticamente.

5. La configuración de replicación se realiza desde la pestaña **Management** dentro del bucket de origen. Se deben agregar reglas de replicación para determinar qué datos se replicarán.

6. Al crear una regla de replicación, se debe especificar el **bucket de destino**, que puede estar en la misma cuenta de AWS o en una cuenta diferente.

7. Para que S3 pueda replicar datos entre buckets, es necesario crear una **IAM Role** que otorgue permisos adecuados al servicio S3 para leer y escribir objetos en ambos buckets.

8. La replicación solo aplica a objetos creados después de configurar la regla. Los objetos existentes no se replican a menos que se utilicen **S3 Batch Operations**, una operación separada de la replicación.

9. Una vez creada la regla de replicación, los archivos nuevos que se suban al bucket de origen comenzarán a replicarse automáticamente al bucket de destino después de unos segundos.

10. Cada objeto replicado mantiene el mismo **Version ID** que en el bucket de origen, lo que permite rastrear cambios y restaurar versiones anteriores si es necesario.

11. Para verificar que la replicación está funcionando, se puede cargar un nuevo archivo en el bucket de origen y luego revisar el bucket de destino tras unos segundos para confirmar su presencia.

12. Si un archivo ya existente debe replicarse, es necesario subir una nueva versión del archivo al bucket de origen. Esto generará una nueva versión que se replicará automáticamente al bucket de destino.

13. Se puede comprobar que la replicación está funcionando correctamente comparando los **Version IDs** en ambos buckets, los cuales deben coincidir.

14. Ejemplo de configuración de replicación con AWS CLI:

```sh
aws s3api put-bucket-replication \
  --bucket mi-bucket-origen \
  --replication-configuration '{
    "Role": "arn:aws:iam::123456789012:role/replication-role",
    "Rules": [
        {
            "Status": "Enabled",
            "Priority": 1,
            "DeleteMarkerReplication": {"Status": "Disabled"},
            "Filter": {},
            "Destination": {
                "Bucket": "arn:aws:s3:::mi-bucket-destino",
                "StorageClass": "STANDARD"
            }
        }
    ]
  }'
```
Este comando establece una replicación activa entre "mi-bucket-origen" y "mi-bucket-destino" asegurando la copia automática de objetos nuevos.

# Video número 83. S3 Storage Classes Overview

1. Amazon S3 ofrece diversas clases de almacenamiento, cada una optimizada para diferentes necesidades de acceso, costos y durabilidad.

2. **S3 Standard** es la clase por defecto y está diseñada para datos de acceso frecuente. Tiene alta disponibilidad (99.99%), baja latencia y alto rendimiento, siendo ideal para aplicaciones móviles, análisis de datos y distribución de contenido.

3. **S3 Infrequent Access (S3-IA)** está pensada para datos que se consultan con menor frecuencia pero requieren acceso rápido cuando se necesitan. Su disponibilidad es del 99.9% y se usa en copias de seguridad y recuperación ante desastres.

4. **S3 One Zone-Infrequent Access (S3 One Zone-IA)** almacena los datos en una sola zona de disponibilidad, lo que reduce costos, pero con menor durabilidad. Tiene una disponibilidad del 99.5% y es adecuada para backups secundarios o datos que pueden regenerarse.

5. **S3 Glacier Instant Retrieval** permite recuperar datos en milisegundos y tiene un costo inferior al de S3-IA. Su duración mínima de almacenamiento es de 90 días, siendo ideal para backups de largo plazo con acceso esporádico.

6. **S3 Glacier Flexible Retrieval** (antes conocido como S3 Glacier) ofrece tres opciones de recuperación: Expedited (1-5 minutos), Standard (3-5 horas) y Bulk (5-12 horas). Se usa para archivos almacenados por largos periodos con acceso poco frecuente.

7. **S3 Glacier Deep Archive** es la opción más económica, pero con tiempos de recuperación más largos: 12 horas en modo Standard y 48 horas en modo Bulk. Su duración mínima de almacenamiento es de 180 días.

8. **S3 Intelligent-Tiering** permite mover automáticamente los objetos entre diferentes niveles de acceso según su uso, sin costos de recuperación y con una pequeña tarifa de monitoreo mensual.

9. **Durabilidad vs. Disponibilidad:** Todos los tipos de almacenamiento en S3 tienen una durabilidad de **99.999999999% (11 nueves)**, lo que significa que perder un objeto es casi imposible. La disponibilidad, en cambio, varía según la clase de almacenamiento.

10. **Ejemplo de configuración de clase de almacenamiento al subir un objeto:**

```sh
aws s3 cp archivo.txt s3://mi-bucket --storage-class STANDARD_IA
```

Este comando sube un archivo al bucket "mi-bucket" con la clase de almacenamiento **S3-IA**.

11. Es posible cambiar la clase de almacenamiento de un objeto manualmente utilizando el comando:

```sh
aws s3api copy-object \
    --bucket mi-bucket \
    --copy-source mi-bucket/archivo.txt \
    --key archivo.txt \
    --storage-class GLACIER \
    --metadata-directive COPY
```
Este comando cambia la clase de almacenamiento de un objeto a **S3 Glacier** sin modificar sus metadatos.

12. Se pueden definir **Reglas de Lifecycle** para automatizar la transición de objetos entre diferentes clases de almacenamiento después de un tiempo determinado.

13. Ejemplo de una configuración de **Lifecycle Policy** para mover objetos a Glacier después de 30 días:

```json
{
    "Rules": [
        {
            "ID": "MoveToGlacier",
            "Status": "Enabled",
            "Filter": {},
            "Transitions": [
                {
                    "Days": 30,
                    "StorageClass": "GLACIER"
                }
            ]
        }
    ]
}
```

14. **Costos:** S3 Standard es la opción más cara, mientras que Glacier Deep Archive es la más económica. Las opciones de bajo costo tienen restricciones de recuperación y tiempo mínimo de almacenamiento.

15. **Casos de uso:**
   - **S3 Standard:** Uso frecuente como hosting de sitios web o almacenamiento de datos de aplicaciones.
   - **S3-IA:** Backups con acceso ocasional.
   - **S3 One Zone-IA:** Copias secundarias de datos no críticos.
   - **Glacier:** Archivos que necesitan archivarse por largos periodos sin acceso frecuente.
   - **Intelligent-Tiering:** Datos con patrones de acceso variables.

16. **Recuperación de objetos en Glacier:** Para restaurar un objeto en Glacier, se necesita iniciar un job de recuperación con el siguiente comando:

```sh
aws s3api restore-object \
    --bucket mi-bucket \
    --key archivo.txt \
    --restore-request '{"Days": 7, "GlacierJobParameters": {"Tier": "Expedited"}}'
```
Este comando restaura "archivo.txt" desde Glacier en modo **Expedited** por 7 días.

17. **Comparación de tiempos de acceso:**
   - **Inmediato:** S3 Standard, S3-IA, S3 Instant Retrieval.
   - **Minutos a horas:** Glacier Flexible Retrieval.
   - **Hasta 48 horas:** Glacier Deep Archive.

18. **S3 Intelligent-Tiering es ideal para quienes no desean gestionar manualmente el cambio de clases de almacenamiento**, ya que S3 mueve los objetos de manera automática según su uso.

