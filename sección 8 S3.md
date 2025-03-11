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