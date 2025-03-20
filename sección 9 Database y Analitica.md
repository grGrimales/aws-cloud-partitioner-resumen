# 93. Introducción a las Bases de Datos  

1. **Las bases de datos permiten almacenar información de manera estructurada**, facilitando la consulta y gestión eficiente de los datos mediante índices y relaciones entre registros.  

2. **Los sistemas de almacenamiento como EBS, EFS y S3 permiten gestionar archivos individualmente**, pero cuando se requiere una organización estructurada de datos, se utilizan bases de datos.  

3. **Las bases de datos pueden clasificarse en dos grandes grupos: relacionales y NoSQL**, cada una con características específicas para distintos casos de uso.  

4. **Las bases de datos relacionales organizan la información en tablas interconectadas mediante relaciones**, permitiendo estructurar los datos en columnas y filas, similar a una hoja de cálculo.  

5. **El lenguaje de consulta estructurado (SQL) es el estándar en bases de datos relacionales**, permitiendo realizar consultas, inserciones y modificaciones de datos con comandos como:  
   ```sql
   SELECT * FROM estudiantes WHERE departamento_id = 3;
   ```  

6. **Ejemplo de relaciones en bases de datos relacionales:**  
   - Una tabla de `Estudiantes` con columnas `id`, `nombre`, `email`, `departamento_id`.  
   - Una tabla de `Departamentos` con columnas `id`, `nombre`.  
   - Se establece una relación entre `departamento_id` de la tabla `Estudiantes` y `id` de la tabla `Departamentos`.  

7. **Las bases de datos NoSQL (No Structured Query Language) tienen una estructura más flexible**, permitiendo almacenar datos en formatos como documentos JSON en lugar de tablas fijas.  

8. **Los principales beneficios de NoSQL incluyen flexibilidad, escalabilidad horizontal y alto rendimiento**, permitiendo distribuir los datos en múltiples servidores sin afectar la velocidad de consulta.  

9. **Ejemplo de un documento JSON en una base de datos NoSQL:**  
   ```json
   {
     "nombre": "Juan Pérez",
     "edad": 30,
     "direccion": {
       "calle": "Av. Siempre Viva",
       "ciudad": "Springfield"
     },
     "autos": ["Ford", "BMW", "Fiat"]
   }
   ```  
   Este formato permite anidar información y almacenar datos de manera más dinámica.  

10. **Las bases de datos NoSQL se dividen en varias categorías:**  
   - **Key-Value:** Como DynamoDB, almacenan datos como pares clave-valor.  
   - **Documentos:** Como MongoDB, utilizan JSON o BSON.  
   - **Grafos:** Como Neo4j, modelan relaciones complejas entre datos.  
   - **En memoria:** Como Redis, optimizadas para velocidad.  
   - **Motores de búsqueda:** Como Elasticsearch, diseñadas para indexación y consultas rápidas.  

11. **AWS ofrece bases de datos administradas que reducen la carga operativa del usuario**, proporcionando alta disponibilidad, escalabilidad automática y backups automatizados.  

12. **Los beneficios de utilizar bases de datos administradas en AWS incluyen:**  
   - Rápida implementación.  
   - Alta disponibilidad y tolerancia a fallos.  
   - Escalabilidad vertical y horizontal.  
   - Backups y restauración automatizados.  
   - Parches y actualizaciones gestionados por AWS.  

13. **Ejemplo de creación de una base de datos administrada en AWS con RDS:**  
   ```bash
   aws rds create-db-instance --db-instance-identifier mydb --db-instance-class db.t3.micro --engine mysql --allocated-storage 20
   ```  
   Esto crea una instancia de MySQL con 20 GB de almacenamiento.  

14. **AWS se encarga del mantenimiento de las bases de datos administradas**, incluyendo actualizaciones de seguridad, monitoreo y generación de alertas en caso de fallos.  

15. **Las bases de datos autoadministradas pueden ejecutarse en instancias EC2**, pero requieren gestionar manualmente la disponibilidad, backups y parches de seguridad.  

16. **El modelo de responsabilidad compartida en bases de datos administradas establece que AWS se encarga de la infraestructura**, mientras que el usuario gestiona la configuración, optimización y seguridad de los datos.  

17. **AWS ofrece una variedad de bases de datos administradas para distintos casos de uso:**  
   - RDS para bases de datos relacionales como MySQL, PostgreSQL y SQL Server.  
   - DynamoDB para almacenamiento NoSQL altamente escalable.  
   - ElastiCache para bases de datos en memoria como Redis y Memcached.  
   - Neptune para bases de datos de grafos.  
   - DocumentDB para bases de datos de documentos similares a MongoDB.  

18. **El uso de bases de datos administradas facilita la escalabilidad y reduce la complejidad operativa**, permitiendo a las empresas centrarse en el desarrollo sin preocuparse por la infraestructura subyacente.  


# 94. RDS & Aurora Overview  

1. **RDS (Relational Database Service) es el servicio administrado de bases de datos relacionales en AWS**, diseñado para facilitar la gestión y escalabilidad sin necesidad de administrar la infraestructura subyacente.  

2. **RDS soporta varios motores de bases de datos populares**, incluyendo PostgreSQL, MySQL, MariaDB, Oracle, Microsoft SQL Server e IBM DB2, ofreciendo flexibilidad en la elección del motor adecuado.  

3. **Aurora es una base de datos relacional propietaria de AWS**, compatible con PostgreSQL y MySQL, optimizada para la nube y diseñada para ofrecer mejor rendimiento y escalabilidad en comparación con RDS estándar.  

4. **Las ventajas de usar RDS sobre una base de datos en EC2 incluyen automatización de provisión, parches, respaldos y monitoreo**, lo que reduce la carga operativa de los administradores.  

5. **RDS permite realizar restauraciones a un punto en el tiempo (Point-in-Time Restore)**, facilitando la recuperación ante fallos con backups automáticos y snapshots manuales.  

6. **Para mejorar el rendimiento de lectura, RDS permite configurar Read Replicas**, lo que ayuda a distribuir la carga de consultas de lectura y mejorar la escalabilidad de la base de datos.  

7. **RDS admite despliegue Multi-AZ**, lo que significa que mantiene una réplica en una segunda zona de disponibilidad para garantizar alta disponibilidad y recuperación ante fallos.  

8. **Ejemplo de creación de una base de datos en RDS con AWS CLI:**  
   ```bash
   aws rds create-db-instance --db-instance-identifier mydb --db-instance-class db.t3.micro --engine mysql --allocated-storage 20
   ```  
   Este comando crea una instancia de MySQL con 20 GB de almacenamiento.  

9. **No es posible acceder a RDS mediante SSH**, ya que AWS gestiona completamente la infraestructura subyacente, permitiendo a los usuarios interactuar únicamente a través de herramientas de administración de bases de datos.  

10. **Aurora tiene un rendimiento superior a RDS estándar**, con hasta 5 veces más velocidad que MySQL y 3 veces más que PostgreSQL en RDS, gracias a su arquitectura optimizada para la nube.  

11. **Aurora escala automáticamente su almacenamiento en bloques de 10 GB hasta 128 TB**, eliminando la necesidad de planificación manual de capacidad.  

12. **Aurora es un 20% más costoso que RDS, pero ofrece mayor eficiencia y escalabilidad**, lo que lo convierte en una opción más rentable para cargas de trabajo críticas y de alto rendimiento.  

13. **Aurora Serverless permite la ejecución automática de bases de datos sin necesidad de gestión manual de capacidad**, ajustando dinámicamente los recursos según la demanda.  

14. **Aurora Serverless es ideal para cargas de trabajo intermitentes o impredecibles**, ya que permite pagar solo por el tiempo de uso en lugar de mantener instancias activas constantemente.  

15. **Ejemplo de creación de una base de datos en Aurora Serverless con AWS CLI:**  
   ```bash
   aws rds create-db-cluster --engine aurora-mysql --db-cluster-identifier my-aurora-serverless --scaling-configuration MinCapacity=1,MaxCapacity=4
   ```  
   Esto configura una base de datos Aurora Serverless con capacidad de escalado automático.  

16. **Aurora Serverless utiliza un proxy administrado para manejar conexiones a la base de datos**, permitiendo la activación y desactivación de instancias según la carga de trabajo.  

17. **Para el examen de certificación AWS, es importante recordar que Aurora es la opción más optimizada y escalable para bases de datos relacionales en la nube**, mientras que RDS ofrece una solución administrada basada en motores de bases de datos tradicionales.  


# 95. RDS Hands On  

1. **RDS permite la creación y administración de bases de datos relacionales en la nube**, eliminando la necesidad de gestionar infraestructura y facilitando la escalabilidad y el mantenimiento.  

2. **AWS RDS ofrece dos modos de creación de bases de datos: Standard Create y Easy Create**, donde Easy Create configura la base de datos con las mejores prácticas recomendadas, mientras que Standard Create permite una personalización más avanzada.  

3. **RDS soporta múltiples motores de bases de datos**, incluyendo MySQL, PostgreSQL, MariaDB, Oracle y Microsoft SQL Server, permitiendo elegir el más adecuado para cada caso de uso.  

4. **El Free Tier de RDS permite la creación de bases de datos sin costo por 12 meses**, ofreciendo instancias `db.t2.micro` con hasta 20 GB de almacenamiento SSD gratuito.  

5. **Ejemplo de creación de una base de datos en RDS con AWS CLI:**  
   ```bash
   aws rds create-db-instance --db-instance-identifier mydb --db-instance-class db.t2.micro --engine mysql --allocated-storage 20
   ```  
   Este comando configura una base de datos MySQL con 20 GB de almacenamiento.  

6. **Los parámetros de configuración incluyen la selección del usuario administrador y contraseña**, los cuales deben ser establecidos al momento de la creación de la base de datos.  

7. **RDS permite habilitar el escalado automático de almacenamiento**, asegurando que la capacidad de la base de datos se expanda automáticamente si se necesita más espacio.  

8. **Es posible definir si la base de datos tendrá acceso público**, lo que permite conectarse desde cualquier parte de internet o restringir el acceso solo a instancias dentro de la misma VPC.  

9. **Para permitir conexiones seguras, RDS utiliza grupos de seguridad (Security Groups)**, los cuales definen las reglas de acceso a la base de datos según direcciones IP y puertos.  

10. **Ejemplo de configuración de un grupo de seguridad para permitir conexiones en el puerto 3306 (MySQL):**  
    ```bash
    aws ec2 authorize-security-group-ingress --group-id sg-12345678 --protocol tcp --port 3306 --cidr 0.0.0.0/0
    ```  
    Este comando permite conexiones desde cualquier IP, lo cual no es recomendable en entornos de producción.  

11. **RDS proporciona monitoreo en tiempo real del uso de la CPU, memoria y conexiones**, lo que facilita la administración y optimización del rendimiento.  

12. **Se pueden tomar snapshots (copias de seguridad) manuales de la base de datos**, permitiendo restaurarla en cualquier momento o replicarla en otra región.  

13. **Ejemplo de creación de un snapshot de una base de datos en RDS:**  
    ```bash
    aws rds create-db-snapshot --db-instance-identifier mydb --db-snapshot-identifier mydb-snapshot
    ```  

14. **Las snapshots pueden ser restauradas para crear nuevas bases de datos con la misma configuración y datos**, lo que facilita la recuperación ante fallos o la clonación de entornos.  

15. **AWS permite copiar snapshots entre regiones**, lo que es útil para implementar estrategias de recuperación ante desastres y garantizar la disponibilidad en múltiples ubicaciones.  

16. **Las snapshots también pueden ser compartidas con otras cuentas de AWS**, permitiendo a diferentes usuarios acceder a los datos sin necesidad de crear copias manualmente.  

17. **RDS permite eliminar bases de datos de manera sencilla a través de la consola o la CLI**, asegurando la eliminación completa de la instancia y sus backups si así se configura.  

18. **Ejemplo de eliminación de una base de datos en RDS:**  
    ```bash
    aws rds delete-db-instance --db-instance-identifier mydb --skip-final-snapshot
    ```  
    Este comando elimina la base de datos sin crear un snapshot final.  

19. **El uso de RDS facilita la gestión de bases de datos en la nube, reduciendo la carga operativa y mejorando la disponibilidad y seguridad**, lo que lo convierte en una opción ideal para aplicaciones empresariales.  

# 95. RDS Hands On  

1. **RDS permite la creación y administración de bases de datos relacionales en la nube**, eliminando la necesidad de gestionar infraestructura y facilitando la escalabilidad y el mantenimiento.  

2. **AWS RDS ofrece dos modos de creación de bases de datos: Standard Create y Easy Create**, donde Easy Create configura la base de datos con las mejores prácticas recomendadas, mientras que Standard Create permite una personalización más avanzada.  

3. **RDS soporta múltiples motores de bases de datos**, incluyendo MySQL, PostgreSQL, MariaDB, Oracle y Microsoft SQL Server, permitiendo elegir el más adecuado para cada caso de uso.  

4. **El Free Tier de RDS permite la creación de bases de datos sin costo por 12 meses**, ofreciendo instancias `db.t2.micro` con hasta 20 GB de almacenamiento SSD gratuito.  

5. **Ejemplo de creación de una base de datos en RDS con AWS CLI:**  
   ```bash
   aws rds create-db-instance --db-instance-identifier mydb --db-instance-class db.t2.micro --engine mysql --allocated-storage 20
   ```  
   Este comando configura una base de datos MySQL con 20 GB de almacenamiento.  

6. **Los parámetros de configuración incluyen la selección del usuario administrador y contraseña**, los cuales deben ser establecidos al momento de la creación de la base de datos.  

7. **RDS permite habilitar el escalado automático de almacenamiento**, asegurando que la capacidad de la base de datos se expanda automáticamente si se necesita más espacio.  

8. **Es posible definir si la base de datos tendrá acceso público**, lo que permite conectarse desde cualquier parte de internet o restringir el acceso solo a instancias dentro de la misma VPC.  

9. **Para permitir conexiones seguras, RDS utiliza grupos de seguridad (Security Groups)**, los cuales definen las reglas de acceso a la base de datos según direcciones IP y puertos.  

10. **Ejemplo de configuración de un grupo de seguridad para permitir conexiones en el puerto 3306 (MySQL):**  
    ```bash
    aws ec2 authorize-security-group-ingress --group-id sg-12345678 --protocol tcp --port 3306 --cidr 0.0.0.0/0
    ```  
    Este comando permite conexiones desde cualquier IP, lo cual no es recomendable en entornos de producción.  

11. **RDS proporciona monitoreo en tiempo real del uso de la CPU, memoria y conexiones**, lo que facilita la administración y optimización del rendimiento.  

12. **Se pueden tomar snapshots (copias de seguridad) manuales de la base de datos**, permitiendo restaurarla en cualquier momento o replicarla en otra región.  

13. **Ejemplo de creación de un snapshot de una base de datos en RDS:**  
    ```bash
    aws rds create-db-snapshot --db-instance-identifier mydb --db-snapshot-identifier mydb-snapshot
    ```  

14. **Las snapshots pueden ser restauradas para crear nuevas bases de datos con la misma configuración y datos**, lo que facilita la recuperación ante fallos o la clonación de entornos.  

15. **AWS permite copiar snapshots entre regiones**, lo que es útil para implementar estrategias de recuperación ante desastres y garantizar la disponibilidad en múltiples ubicaciones.  

16. **Las snapshots también pueden ser compartidas con otras cuentas de AWS**, permitiendo a diferentes usuarios acceder a los datos sin necesidad de crear copias manualmente.  

17. **RDS permite eliminar bases de datos de manera sencilla a través de la consola o la CLI**, asegurando la eliminación completa de la instancia y sus backups si así se configura.  

18. **Ejemplo de eliminación de una base de datos en RDS:**  
    ```bash
    aws rds delete-db-instance --db-instance-identifier mydb --skip-final-snapshot
    ```  
    Este comando elimina la base de datos sin crear un snapshot final.  

19. **El uso de RDS facilita la gestión de bases de datos en la nube, reduciendo la carga operativa y mejorando la disponibilidad y seguridad**, lo que lo convierte en una opción ideal para aplicaciones empresariales.  

# 96. RDS Deployment Options  

1. **RDS ofrece varias opciones de despliegue para mejorar el rendimiento, la disponibilidad y la recuperación ante desastres**, permitiendo elegir la arquitectura más adecuada según las necesidades del sistema.  

2. **Las Read Replicas permiten escalar la carga de lectura**, creando copias de solo lectura de la base de datos principal para distribuir el tráfico de consultas entre múltiples instancias.  

3. **Es posible crear hasta 15 Read Replicas de una base de datos RDS**, lo que permite aumentar significativamente la capacidad de lectura sin afectar el rendimiento de la base de datos principal.  

4. **Ejemplo de creación de una Read Replica en RDS con AWS CLI:**  
   ```bash
   aws rds create-db-instance-read-replica --db-instance-identifier mydb-replica --source-db-instance-identifier mydb
   ```  
   Esto genera una réplica de solo lectura de la base de datos `mydb`.  

5. **Las Read Replicas solo permiten operaciones de lectura**, por lo que todas las escrituras deben seguir ocurriendo en la base de datos principal.  

6. **El despliegue Multi-AZ mejora la alta disponibilidad al replicar automáticamente la base de datos en una zona de disponibilidad secundaria**, lo que permite la recuperación inmediata ante fallos.  

7. **En una configuración Multi-AZ, la base de datos de respaldo es pasiva y solo se activa en caso de fallo de la instancia principal**, asegurando continuidad operativa sin intervención manual.  

8. **Ejemplo de habilitación de Multi-AZ en RDS con AWS CLI:**  
   ```bash
   aws rds modify-db-instance --db-instance-identifier mydb --multi-az
   ```  
   Esto configura la base de datos para operar en modo Multi-AZ.  

9. **El proceso de failover en Multi-AZ es automático**, lo que significa que si la base de datos principal falla, RDS conmuta a la base de datos secundaria sin interrupciones notables en la aplicación.  

10. **Las Read Replicas pueden ser promovidas a bases de datos independientes en caso de falla de la base de datos principal**, lo que permite recuperar rápidamente un sistema en caso de desastre.  

11. **La opción de Multi-Region permite crear Read Replicas en diferentes regiones de AWS**, reduciendo la latencia para usuarios distribuidos globalmente y mejorando la recuperación ante desastres.  

12. **Ejemplo de creación de una Read Replica en otra región:**  
   ```bash
   aws rds create-db-instance-read-replica --db-instance-identifier mydb-replica --source-db-instance-identifier mydb --region us-east-2
   ```  
   Esto replica la base de datos en la región `us-east-2`.  

13. **La replicación Multi-Region es ideal para estrategias de disaster recovery**, permitiendo que una aplicación siga operando incluso si toda una región de AWS experimenta una interrupción.  

14. **Las Read Replicas en múltiples regiones mejoran el rendimiento global de las aplicaciones distribuidas**, permitiendo que las consultas de lectura sean atendidas desde una base de datos más cercana al usuario final.  

15. **El uso de replicación entre regiones tiene costos adicionales**, ya que la transferencia de datos entre regiones genera cargos por ancho de banda.  

16. **Al elegir un modelo de despliegue en RDS, es importante considerar los requisitos de escalabilidad, disponibilidad y recuperación ante desastres**, asegurando que la arquitectura seleccionada cumpla con las necesidades del sistema.  

# 97. ElastiCache Overview  

1. **Amazon ElastiCache es un servicio de bases de datos en memoria administrado por AWS**, diseñado para mejorar el rendimiento de aplicaciones al reducir la carga sobre bases de datos relacionales y NoSQL.  

2. **ElastiCache soporta dos motores de almacenamiento en memoria: Redis y Memcached**, ambos optimizados para proporcionar acceso ultrarrápido a los datos almacenados en caché.  

3. **El uso de ElastiCache permite reducir la latencia y mejorar el rendimiento de aplicaciones con alta demanda de lectura**, evitando consultas repetitivas a bases de datos más lentas como RDS o DynamoDB.  

4. **Ejemplo de creación de una instancia de ElastiCache con AWS CLI:**  
   ```bash
   aws elasticache create-cache-cluster --cache-cluster-id my-cache --engine redis --cache-node-type cache.t3.micro --num-cache-nodes 1
   ```  
   Esto crea un clúster de ElastiCache con Redis utilizando una instancia `t3.micro`.  

5. **Las bases de datos en memoria permiten almacenar datos temporalmente con acceso extremadamente rápido**, ideal para sesiones de usuario, caché de consultas y almacenamiento temporal de datos procesados.  

6. **ElastiCache puede utilizarse para almacenar resultados de consultas frecuentes en RDS**, evitando que la base de datos principal maneje todas las solicitudes y mejorando la escalabilidad.  

7. **Ejemplo de almacenamiento de datos en Redis con Python:**  
   ```python
   import redis

   r = redis.Redis(host='my-cache.abcdef.0001.use1.cache.amazonaws.com', port=6379, decode_responses=True)
   r.set('usuario_123', 'Grediana')
   print(r.get('usuario_123'))  # Output: Grediana
   ```  
   Esto almacena y recupera un valor en Redis.  

8. **Memcached es una opción ligera y distribuida para aplicaciones que necesitan almacenamiento en memoria sin persistencia de datos**, mientras que Redis soporta estructuras avanzadas y persistencia opcional.  

9. **ElastiCache es ideal para aplicaciones con tráfico intensivo de lectura y baja variabilidad en los datos**, como páginas web dinámicas, dashboards en tiempo real y sistemas de recomendaciones.  

10. **El modelo de arquitectura recomendado incluye un balanceador de carga (ELB), instancias EC2 y RDS con ElastiCache como capa de caché intermedia**, mejorando la eficiencia del sistema.  

11. **ElastiCache es completamente administrado por AWS**, lo que significa que AWS maneja la configuración, escalabilidad, parches, monitoreo y recuperación ante fallos.  

12. **Ejemplo de configuración de una clave con tiempo de expiración en Redis:**  
   ```python
   r.setex('token_sesion', 3600, 'abcdef123456')  # Expira en 1 hora
   ```  
   Esto permite manejar sesiones de usuario sin necesidad de consultas recurrentes a la base de datos.  

13. **El uso de ElastiCache permite aliviar la presión sobre bases de datos relacionales**, disminuyendo costos operativos y evitando la necesidad de escalar instancias RDS constantemente.  

14. **Para el examen de certificación de AWS, es importante recordar que ElastiCache es la solución recomendada para bases de datos en memoria y reducción de latencia en aplicaciones escalables**.  

# 98. DynamoDB Overview  

1. **DynamoDB es una base de datos NoSQL completamente administrada por AWS**, diseñada para ofrecer alta disponibilidad y escalabilidad sin necesidad de gestionar servidores.  

2. **La base de datos se replica automáticamente en tres zonas de disponibilidad (AZs)**, garantizando resiliencia y tolerancia a fallos sin intervención del usuario.  

3. **DynamoDB es una base de datos serverless, lo que significa que no se necesita aprovisionar instancias manualmente**, a diferencia de RDS o ElastiCache, donde se requiere definir el tamaño de las instancias.  

4. **Está diseñada para manejar cargas masivas, soportando millones de solicitudes por segundo**, con capacidad de almacenamiento para trillones de filas y cientos de terabytes de datos.  

5. **El rendimiento es extremadamente rápido, con tiempos de respuesta en milisegundos de un solo dígito**, lo que la hace ideal para aplicaciones que requieren baja latencia.  

6. **DynamoDB se integra con IAM para controlar permisos de acceso, autorización y administración de usuarios**, permitiendo políticas de seguridad avanzadas.  

7. **Ofrece una opción de autoescalado que ajusta automáticamente la capacidad de lectura y escritura**, permitiendo que la base de datos se adapte dinámicamente a la carga de trabajo.  

8. **El modelo de datos de DynamoDB se basa en un esquema clave-valor**, donde cada elemento de la tabla tiene una clave primaria obligatoria y atributos adicionales opcionales.  

9. **Ejemplo de creación de una tabla en DynamoDB usando AWS CLI:**  
   ```bash
   aws dynamodb create-table --table-name Usuarios \
     --attribute-definitions AttributeName=UserId,AttributeType=S \
     --key-schema AttributeName=UserId,KeyType=HASH \
     --provisioned-throughput ReadCapacityUnits=5,WriteCapacityUnits=5
   ```  
   Esto crea una tabla llamada `Usuarios` con un campo `UserId` como clave primaria.  

10. **El esquema de clave primaria puede ser solo una partición key o una combinación de partición key y sort key**, lo que permite una organización flexible de los datos.  

11. **DynamoDB soporta dos clases de tablas para optimización de costos:**  
   - **Standard:** Para datos con accesos frecuentes.  
   - **Infrequent Access (IA):** Para datos menos usados, reduciendo costos de almacenamiento.  

12. **DynamoDB Accelerator (DAX) es un servicio de caché en memoria diseñado específicamente para DynamoDB**, proporcionando tiempos de respuesta en microsegundos.  

13. **Ejemplo de almacenamiento de datos en DynamoDB usando Python con boto3:**  
   ```python
   import boto3

   dynamodb = boto3.resource('dynamodb')
   table = dynamodb.Table('Usuarios')

   table.put_item(Item={'UserId': '123', 'Nombre': 'Grediana', 'Edad': 30})
   response = table.get_item(Key={'UserId': '123'})
   print(response['Item'])
   ```  
   Esto almacena y recupera un usuario en la tabla `Usuarios`.  

14. **DAX mejora el rendimiento hasta 10 veces en comparación con DynamoDB sin caché**, evitando consultas redundantes y mejorando la escalabilidad.  

15. **ElastiCache y DAX no son intercambiables**, ya que DAX es una solución específica para DynamoDB, mientras que ElastiCache puede utilizarse con otras bases de datos.  

Esta arquitectura hace que DynamoDB sea ideal para aplicaciones web escalables, comercio electrónico, juegos en línea y cualquier sistema que requiera acceso rápido y confiable a grandes volúmenes de datos.

# 99. DynamoDB Hands On  

1. **DynamoDB es un servicio de base de datos NoSQL completamente administrado y sin servidor que permite la creación de tablas sin necesidad de aprovisionar infraestructura.**  

2. **Al crear una tabla en DynamoDB, es obligatorio definir una clave de partición (partition key), que sirve como identificador único de los datos almacenados.**  

3. **Opcionalmente, se puede definir una clave de ordenamiento (sort key), que permite estructurar los datos de forma jerárquica dentro de la tabla.**  

4. **La flexibilidad de DynamoDB permite que los elementos dentro de la misma tabla no necesiten seguir un esquema fijo, lo que significa que diferentes ítems pueden contener atributos distintos.**  

5. **Los atributos en una tabla de DynamoDB pueden ser de diferentes tipos, como cadenas de texto (string), números (number) y listas, entre otros.**  

6. **Para insertar datos en una tabla de DynamoDB, se pueden definir los valores de los atributos sin necesidad de declarar una estructura previa, como ocurre en bases de datos relacionales.**  

7. **Ejemplo de inserción de un ítem en DynamoDB utilizando AWS SDK para Python (Boto3):**  
   ```python
   import boto3

   dynamodb = boto3.resource('dynamodb')
   table = dynamodb.Table('DemoTable')

   table.put_item(
       Item={
           'user_id': '1234',
           'first_name': 'Stephane',
           'last_name': 'Maarek',
           'favorite_number': 42
       }
   )
   print("Elemento insertado correctamente")
   ```  

8. **DynamoDB permite insertar elementos con atributos personalizados, sin requerir que todas las filas tengan los mismos atributos.**  

9. **A diferencia de las bases de datos relacionales como RDS, en DynamoDB no es posible realizar consultas con JOINs entre múltiples tablas.**  

10. **El enfoque NoSQL de DynamoDB implica que todos los datos relevantes deben estar estructurados dentro de la misma tabla para optimizar la consulta y evitar la necesidad de relaciones complejas.**  

11. **Se pueden eliminar tablas de DynamoDB fácilmente a través de la consola de AWS o utilizando la API de AWS SDK.**  

12. **DynamoDB se enfoca en ofrecer alta disponibilidad y escalabilidad, lo que lo hace ideal para aplicaciones de alto tráfico y baja latencia.**  

13. **Al ser una base de datos sin servidor, AWS maneja automáticamente el escalado y mantenimiento, permitiendo a los desarrolladores centrarse en la lógica de negocio.**  

14. **DynamoDB es ampliamente utilizado en aplicaciones móviles, juegos en línea, análisis en tiempo real y cualquier caso de uso que requiera un acceso rápido y flexible a los datos.**  

15. **Para eliminar una tabla utilizando Boto3 en Python, se puede ejecutar el siguiente código:**  
   ```python
   table.delete()
   print("Tabla eliminada correctamente")
   ```  


   # 100. DynamoDB Global Tables  

1. **DynamoDB Global Tables es una funcionalidad que permite replicar tablas en múltiples regiones de AWS**, asegurando baja latencia y disponibilidad global.  

2. **El objetivo principal de las Global Tables es mejorar el acceso a los datos para usuarios distribuidos geográficamente**, evitando la necesidad de hacer consultas a una única región remota.  

3. **Los datos almacenados en una tabla global se replican automáticamente entre regiones**, permitiendo que cualquier usuario acceda a la versión más actualizada de la información sin importar su ubicación.  

4. **Ejemplo de creación de una Global Table en AWS CLI:**  
   ```bash
   aws dynamodb create-global-table \
     --global-table-name MiTablaGlobal \
     --replication-group RegionName=us-east-1 RegionName=eu-west-3
   ```  
   Esto configura una tabla replicada entre `us-east-1` (Virginia) y `eu-west-3` (París).  

5. **Las Global Tables permiten hasta 10 regiones de replicación**, lo que garantiza la escalabilidad y redundancia de los datos en distintas ubicaciones.  

6. **El modelo de replicación es activo-activo**, lo que significa que cualquier instancia de la tabla en cualquier región puede aceptar escrituras y estas se sincronizarán automáticamente con las demás.  

7. **La replicación entre regiones es bidireccional**, asegurando que los cambios realizados en cualquier región sean reflejados en todas las demás.  

8. **Ejemplo de escritura y lectura en una Global Table con Python y boto3:**  
   ```python
   import boto3

   dynamodb = boto3.resource('dynamodb', region_name='us-east-1')
   table = dynamodb.Table('MiTablaGlobal')

   # Insertar un nuevo item
   table.put_item(Item={'UserId': '123', 'Nombre': 'Grediana', 'Edad': 30})

   # Leer el item desde otra región
   dynamodb_eu = boto3.resource('dynamodb', region_name='eu-west-3')
   table_eu = dynamodb_eu.Table('MiTablaGlobal')
   response = table_eu.get_item(Key={'UserId': '123'})
   print(response['Item'])
   ```  
   Esto demuestra cómo un dato insertado en una región se puede recuperar desde otra.  

9. **Las Global Tables eliminan la necesidad de configurar manualmente la replicación entre regiones**, ya que AWS maneja automáticamente la sincronización de datos.  

10. **Son ideales para aplicaciones globales que requieren acceso rápido a datos desde diferentes continentes**, como redes sociales, sistemas de comercio electrónico y plataformas de streaming.  

11. **El uso de Global Tables implica costos adicionales por la replicación de datos entre regiones**, incluyendo cargos por transferencia de datos y solicitudes de escritura y lectura en cada región.  

12. **La latencia de acceso a los datos se reduce significativamente, ya que los usuarios acceden a una réplica de la tabla en la región más cercana**, evitando demoras por solicitudes a regiones remotas.  

13. **Para el examen de certificación AWS, es importante recordar que DynamoDB Global Tables es la solución recomendada cuando se necesita una base de datos NoSQL distribuida globalmente con replicación automática**.  

# 101. Redshift Overview  

1. **Amazon Redshift es un servicio de almacenamiento de datos (data warehouse) basado en PostgreSQL**, diseñado para ejecutar consultas analíticas y de procesamiento de datos a gran escala.  

2. **A diferencia de RDS, Redshift no está diseñado para transacciones en línea (OLTP), sino para procesamiento analítico en línea (OLAP)**, lo que lo hace ideal para análisis de grandes volúmenes de datos.  

3. **Las cargas de datos en Redshift no son continuas, sino que suelen realizarse en lotes**, por ejemplo, cada hora o diariamente, para optimizar el rendimiento en consultas analíticas.  

4. **Redshift ofrece un rendimiento 10 veces superior a otros data warehouses tradicionales**, permitiendo escalar el almacenamiento y procesamiento hasta niveles de petabytes de datos.  

5. **El almacenamiento en Redshift es columnar en lugar de basado en filas**, lo que significa que los datos se almacenan y procesan por columnas en lugar de filas, mejorando la velocidad de análisis.  

6. **Ejemplo de consulta en Redshift usando SQL para analizar datos de ventas:**  
   ```sql
   SELECT categoria, SUM(ventas) AS total_ventas
   FROM ventas
   GROUP BY categoria
   ORDER BY total_ventas DESC;
   ```  
   Esta consulta permite obtener el total de ventas agrupado por categoría.  

7. **Redshift usa un motor de ejecución de consultas llamado MPP (Massively Parallel Processing)**, que distribuye la carga de trabajo en múltiples nodos para mejorar la velocidad de procesamiento.  

8. **El modelo de pago de Redshift es "pay-as-you-go"**, basado en la cantidad de instancias aprovisionadas y el almacenamiento utilizado, lo que permite controlar costos según el uso.  

9. **Se integra con herramientas de inteligencia de negocios (BI) como Amazon QuickSight y Tableau**, permitiendo la creación de dashboards y reportes en tiempo real sobre los datos almacenados.  

10. **Redshift Serverless es una nueva opción que permite ejecutar cargas de trabajo analíticas sin administrar servidores**, escalando automáticamente según la demanda de consultas.  

11. **Con Redshift Serverless, no es necesario aprovisionar instancias manualmente**, ya que AWS administra la infraestructura y solo se paga por el procesamiento y almacenamiento utilizado.  

12. **Ejemplo de habilitación de Redshift Serverless en AWS CLI:**  
   ```bash
   aws redshift-serverless create-workgroup --workgroup-name mi-redshift-serverless
   ```  
   Esto configura un nuevo entorno de Redshift sin necesidad de definir instancias.  

13. **Las principales aplicaciones de Redshift incluyen generación de reportes, dashboarding, análisis en tiempo real y procesamiento de grandes volúmenes de datos históricos**.  

14. **Al conectarse a Redshift, se pueden utilizar herramientas como el Query Editor de AWS o clientes SQL estándar para ejecutar consultas y analizar datos**.  

15. **Para el examen de certificación AWS, es importante recordar que Redshift es la mejor opción para almacenamiento y análisis de datos en grandes volúmenes**, especialmente en escenarios donde se requiere procesamiento eficiente de consultas complejas.  

# 102. EMR Overview  

1. **Amazon EMR (Elastic MapReduce) es un servicio administrado para ejecutar frameworks de Big Data en AWS**, permitiendo procesar grandes volúmenes de datos de manera distribuida.  

2. **EMR no es una base de datos tradicional, sino una plataforma basada en Hadoop** que permite crear clústeres de servidores para el análisis masivo de datos.  

3. **El servicio se basa en tecnologías de código abierto como Apache Hadoop, Apache Spark, HBase, Presto y Flink**, que se ejecutan sobre un clúster de múltiples instancias de EC2.  

4. **Un clúster de EMR está compuesto por un conjunto de nodos**, incluyendo un nodo maestro que coordina las tareas y múltiples nodos trabajadores que ejecutan los cálculos.  

5. **Ejemplo de creación de un clúster de EMR en AWS CLI:**  
   ```bash
   aws emr create-cluster --name "MiClusterEMR" --release-label emr-6.4.0 \
     --applications Name=Hadoop Name=Spark \
     --instance-type m5.xlarge --instance-count 3
   ```  
   Este comando crea un clúster de EMR con tres instancias EC2 y soporte para Hadoop y Spark.  

6. **EMR se integra con instancias Spot para reducir costos**, lo que permite ejecutar trabajos de Big Data de manera más eficiente aprovechando capacidad de cómputo no utilizada.  

7. **El servicio incluye autoescalado**, lo que permite agregar o eliminar nodos según la demanda de procesamiento sin necesidad de intervención manual.  

8. **Las principales aplicaciones de EMR incluyen procesamiento de datos, aprendizaje automático, indexación web y análisis de grandes volúmenes de información en tiempo real.**  

9. **Ejemplo de ejecución de un trabajo en Spark sobre EMR:**  
   ```python
   from pyspark.sql import SparkSession

   spark = SparkSession.builder.appName("EjemploEMR").getOrCreate()
   df = spark.read.csv("s3://mi-bucket/datos.csv", header=True, inferSchema=True)
   df.groupBy("categoria").count().show()
   ```  
   Este código procesa un archivo CSV almacenado en S3 y agrupa los datos por categoría.  

10. **Los datos en EMR pueden almacenarse en S3, HDFS (Hadoop Distributed File System) o en bases de datos como DynamoDB y Redshift**, facilitando la integración con otros servicios de AWS.  

11. **EMR puede utilizarse para ejecutar consultas analíticas complejas en grandes volúmenes de datos**, reduciendo significativamente el tiempo de procesamiento en comparación con enfoques tradicionales.  

12. **Los costos de EMR se calculan según el número de nodos y el tiempo de ejecución del clúster**, lo que permite controlar el gasto optimizando los recursos utilizados.  

13. **EMR es ideal para empresas que necesitan analizar grandes volúmenes de datos sin administrar su propia infraestructura de Big Data**, aprovechando la escalabilidad y flexibilidad de AWS.  

14. **Para el examen de certificación AWS, es importante recordar que cuando se menciona Hadoop o procesamiento de Big Data distribuido, la opción correcta es Amazon EMR.**  

# 103. Athena Overview  

1. **Amazon Athena es un servicio de consultas SQL serverless**, diseñado para analizar datos almacenados en S3 sin necesidad de infraestructura o bases de datos tradicionales.  

2. **Athena permite ejecutar consultas SQL directamente sobre archivos almacenados en S3**, sin requerir carga previa de datos en un motor de base de datos.  

3. **Los formatos de archivo soportados incluyen CSV, JSON, ORC, Avro y Parquet**, lo que permite flexibilidad en la organización y almacenamiento de datos en S3.  

4. **Ejemplo de consulta SQL en Athena para analizar registros almacenados en S3:**  
   ```sql
   SELECT region, COUNT(*) AS total_transacciones
   FROM s3_sales_data
   WHERE fecha >= '2024-01-01'
   GROUP BY region
   ORDER BY total_transacciones DESC;
   ```  
   Esta consulta cuenta el número de transacciones por región desde el inicio de 2024.  

5. **Athena está basado en el motor Presto**, una tecnología de código abierto optimizada para la ejecución de consultas distribuidas sobre grandes volúmenes de datos.  

6. **El servicio se integra con herramientas de visualización como Amazon QuickSight**, permitiendo la creación de dashboards y reportes sobre los resultados de las consultas.  

7. **El costo de Athena se basa en la cantidad de datos escaneados por consulta, con una tarifa aproximada de $5 por terabyte analizado**, lo que incentiva la optimización del almacenamiento.  

8. **El uso de formatos de almacenamiento comprimidos y columnar, como Parquet o ORC, reduce el costo de consulta**, ya que minimiza la cantidad de datos que deben ser escaneados.  

9. **Athena es ideal para casos de uso de inteligencia de negocios, análisis de logs y auditorías**, permitiendo analizar datos en S3 sin necesidad de moverlos o preprocesarlos.  

10. **Ejemplo de consulta para analizar logs de acceso de AWS CloudTrail en S3:**  
   ```sql
   SELECT eventName, COUNT(*) AS cantidad_eventos
   FROM cloudtrail_logs
   WHERE eventSource = 's3.amazonaws.com'
   GROUP BY eventName
   ORDER BY cantidad_eventos DESC;
   ```  
   Esta consulta muestra cuántos eventos de cada tipo ocurrieron en S3.  

11. **Athena es altamente útil para analizar registros de redes como VPC Flow Logs, ELB Logs y CloudTrail**, facilitando la auditoría y monitoreo de la actividad en AWS.  

12. **Athena es completamente serverless**, lo que significa que no requiere aprovisionamiento de instancias ni administración de infraestructura, reduciendo la complejidad operativa.  

13. **Desde una perspectiva de certificación AWS, Athena es la mejor opción cuando se requiere análisis de datos en S3 sin necesidad de configurar bases de datos adicionales**.  

14. **Cada consulta en Athena se ejecuta en paralelo utilizando cómputo distribuido**, lo que permite obtener resultados rápidamente incluso en grandes volúmenes de datos.  

# 104. QuickSight Overview  

1. **Amazon QuickSight es un servicio serverless de inteligencia de negocios (BI) impulsado por machine learning**, diseñado para crear dashboards interactivos sin necesidad de administrar infraestructura.  

2. **QuickSight permite transformar datos en gráficos, tablas y reportes visuales**, facilitando la toma de decisiones a partir de información estructurada y procesada.  

3. **El servicio se adapta automáticamente a la escala de los datos y la cantidad de usuarios**, lo que lo hace adecuado tanto para pequeñas empresas como para grandes organizaciones.  

4. **Ejemplo de consulta en Athena para extraer datos y analizarlos en QuickSight:**  
   ```sql
   SELECT fecha, total_ventas, region  
   FROM s3_ventas_data  
   WHERE fecha >= '2024-01-01';
   ```  
   Esta consulta obtiene información de ventas para ser visualizada en un dashboard de QuickSight.  

5. **QuickSight soporta integración con múltiples fuentes de datos**, incluyendo RDS, Aurora, Athena, Redshift, S3 y bases de datos externas conectadas mediante ODBC o JDBC.  

6. **El modelo de precios de QuickSight se basa en sesiones de usuario**, lo que significa que se paga solo por las sesiones activas sin necesidad de licencias fijas.  

7. **Incluye capacidades de machine learning integradas para detectar patrones en los datos, identificar anomalías y generar predicciones automáticas**, sin requerir conocimientos avanzados en ciencia de datos.  

8. **Los dashboards creados en QuickSight pueden ser embebidos en aplicaciones web y móviles**, permitiendo la visualización de datos en entornos personalizados.  

9. **Ejemplo de cómo conectar QuickSight a un bucket de S3 para análisis:**  
   - Cargar archivos CSV o JSON en un bucket de S3.  
   - Crear una tabla en Athena basada en los datos de S3.  
   - Conectar QuickSight a la fuente de datos en Athena.  
   - Crear visualizaciones interactivas a partir de los datos extraídos.  

10. **QuickSight permite realizar análisis ad-hoc**, es decir, consultas dinámicas sin la necesidad de definir estructuras de datos fijas previamente.  

11. **El servicio ofrece seguridad avanzada mediante integración con AWS IAM y control de acceso basado en roles**, lo que garantiza que cada usuario solo vea la información autorizada.  

12. **Las empresas pueden utilizar QuickSight para generar informes automatizados y compartir visualizaciones interactivas con diferentes equipos dentro de la organización.**  

13. **Desde una perspectiva de certificación AWS, QuickSight es la mejor opción cuando se menciona la necesidad de visualización de datos dentro del ecosistema de AWS.**  

# 105. DocumentDB Overview  

1. **Amazon DocumentDB es un servicio de base de datos NoSQL totalmente administrado**, diseñado específicamente para trabajar con datos en formato JSON y compatible con MongoDB.  

2. **DocumentDB es similar a Aurora, pero optimizado para bases de datos MongoDB**, proporcionando una solución escalable y altamente disponible en la nube de AWS.  

3. **El almacenamiento en DocumentDB se expande automáticamente en incrementos de 10 GB**, eliminando la necesidad de aprovisionamiento manual y facilitando la escalabilidad.  

4. **Las instancias de DocumentDB pueden escalarse para manejar millones de solicitudes por segundo**, lo que lo hace ideal para aplicaciones que requieren alto rendimiento y disponibilidad.  

5. **Ejemplo de conexión a DocumentDB desde una aplicación Node.js usando Mongoose:**  
   ```javascript
   const mongoose = require('mongoose');

   mongoose.connect('mongodb://usuario:contraseña@docdb-instance.endpoint:27017/miBase', {
       useNewUrlParser: true,
       useUnifiedTopology: true
   }).then(() => console.log('Conectado a DocumentDB'))
     .catch(err => console.error('Error de conexión', err));
   ```  

6. **Al ser un servicio completamente administrado, AWS se encarga de las actualizaciones, parches, monitoreo y copias de seguridad**, permitiendo a los desarrolladores enfocarse en la aplicación.  

7. **DocumentDB replica los datos automáticamente en tres zonas de disponibilidad (AZs)**, garantizando tolerancia a fallos y recuperación rápida en caso de incidentes.  

8. **El servicio es compatible con las API de MongoDB**, lo que permite migraciones sencillas desde bases de datos MongoDB on-premise o en otras nubes.  

9. **Se pueden crear réplicas de lectura en DocumentDB**, lo que ayuda a distribuir la carga de trabajo y mejorar el rendimiento de consultas intensivas.  

10. **Ejemplo de creación de una colección en DocumentDB utilizando la shell de MongoDB:**  
   ```javascript
   use miBase;
   db.usuarios.insertOne({ nombre: "Juan", edad: 30, ciudad: "Madrid" });
   ```  

11. **Al comparar DocumentDB con DynamoDB, ambos son NoSQL, pero DocumentDB es más adecuado para aplicaciones que requieren esquemas flexibles con datos estructurados en JSON**, mientras que DynamoDB es ideal para almacenamiento de alto rendimiento con clave-valor.  

12. **Desde una perspectiva de certificación de AWS, si en un escenario se menciona MongoDB o JSON como modelo de datos, la mejor respuesta será Amazon DocumentDB.**  

13. **DocumentDB es una excelente opción para aplicaciones web, catálogos de productos, sistemas de gestión de contenido y cualquier otra aplicación que dependa de datos en JSON altamente escalables.**  

# 106. Neptune Overview  

1. **Amazon Neptune es una base de datos gráfica totalmente administrada**, diseñada para almacenar y consultar relaciones complejas entre datos con baja latencia.  

2. **El modelo de datos de Neptune se basa en grafos**, lo que lo hace ideal para aplicaciones donde los datos están interconectados, como redes sociales, motores de recomendación y detección de fraudes.  

3. **Ejemplo de una estructura de grafo en Neptune:**  
   ```javascript
   {
       "usuarios": [
           { "id": 1, "nombre": "Ana" },
           { "id": 2, "nombre": "Luis" }
       ],
       "amistades": [
           { "origen": 1, "destino": 2, "relación": "amigo" }
       ]
   }
   ```  

4. **Neptune admite dos lenguajes de consulta especializados en grafos: Gremlin (Apache TinkerPop) y SPARQL**, lo que permite realizar búsquedas eficientes en bases de datos complejas.  

5. **Ejemplo de consulta en Gremlin para obtener los amigos de un usuario:**  
   ```gremlin
   g.V().has("nombre", "Ana").out("amistades").values("nombre")
   ```  

6. **Las bases de datos gráficas son diferentes a las relacionales porque almacenan relaciones directamente en los nodos y aristas**, lo que mejora la eficiencia de consultas que requieren navegar por conexiones complejas.  

7. **Neptune permite almacenar y consultar miles de millones de relaciones con latencias en milisegundos**, lo que lo hace altamente eficiente para grandes volúmenes de datos conectados.  

8. **Cuenta con replicación automática en tres zonas de disponibilidad (AZs), garantizando alta disponibilidad y tolerancia a fallos.**  

9. **Puede escalar hasta 15 réplicas de lectura**, permitiendo la distribución de cargas para mejorar el rendimiento en entornos de alto tráfico.  

10. **Casos de uso de Neptune incluyen redes sociales, sistemas de gestión de conocimiento, detección de fraudes, motores de recomendación y análisis de relaciones en datos científicos.**  

11. **Ejemplo de una consulta en SPARQL para encontrar todas las relaciones de un usuario:**  
   ```sparql
   SELECT ?relacion ?amigo WHERE {
       ?usuario <http://example.com/nombre> "Ana" .
       ?usuario ?relacion ?amigo .
   }
   ```  

12. **Desde el punto de vista del examen de certificación AWS, si se menciona análisis de relaciones complejas o consultas optimizadas en datos interconectados, Neptune es la opción correcta.**  

13. **Neptune se integra con servicios de AWS como IAM para seguridad, CloudWatch para monitoreo y S3 para backups automáticos.**  


# 107. Timestream Overview  

1. **Amazon Timestream es una base de datos totalmente administrada, diseñada para manejar datos de series temporales**, ideal para aplicaciones que registran eventos o mediciones a lo largo del tiempo.  

2. **Los datos de series temporales son aquellos que cambian con el tiempo**, como métricas de rendimiento de sistemas, datos de sensores IoT o registros de actividad financiera.  

3. **Ejemplo de datos de series temporales:**  
   ```json
   {
       "timestamp": "2025-03-15T12:00:00Z",
       "sensor_id": "12345",
       "temperatura": 22.5,
       "humedad": 60
   }
   ```  

4. **Timestream es una base de datos serverless**, lo que significa que no requiere administración de infraestructura y escala automáticamente según la demanda.  

5. **Es hasta 1000 veces más rápida y 10 veces más barata que las bases de datos relacionales tradicionales** para el manejo de datos de series temporales.  

6. **Timestream permite almacenar y analizar billones de eventos diarios**, proporcionando consultas optimizadas para tendencias, patrones y análisis predictivos.  

7. **Ofrece funciones avanzadas de análisis de series temporales**, como agregaciones, interpolaciones y detección de tendencias en tiempo real.  

8. **Ejemplo de consulta en SQL para obtener el promedio de temperatura por hora:**  
   ```sql
   SELECT 
       BIN(time, 1h) AS hora, 
       AVG(temperatura) AS temp_promedio
   FROM sensores
   WHERE time > ago(7d)
   GROUP BY BIN(time, 1h)
   ```  

9. **Se integra con servicios de AWS como Amazon Kinesis, AWS IoT Core y Amazon QuickSight**, permitiendo la recolección, almacenamiento y visualización de datos en tiempo real.  

10. **La estructura de almacenamiento de Timestream se divide en dos capas:**  
    - **Memoria caliente:** para datos recientes y de acceso rápido.  
    - **Memoria fría:** para datos históricos optimizados para almacenamiento a largo plazo.  

11. **El almacenamiento y la consulta de datos en Timestream están optimizados mediante la compresión automática y el ordenamiento basado en el tiempo**, reduciendo los costos de almacenamiento y mejorando la eficiencia de las consultas.  

12. **Ejemplo de consulta para detectar picos de temperatura superiores a 30°C:**  
   ```sql
   SELECT * FROM sensores 
   WHERE temperatura > 30 
   ORDER BY time DESC 
   LIMIT 10
   ```  

13. **Para el examen de certificación AWS, si se menciona el análisis de datos históricos o en tiempo real basados en una línea de tiempo, la respuesta correcta será Amazon Timestream.**  


# 108. QLDB Overview  

1. **Amazon QLDB (Quantum Ledger Database) es una base de datos de libro mayor totalmente gestionada y serverless**, diseñada para registrar y rastrear el historial de transacciones de manera inmutable.  

2. **Un libro mayor (ledger) es un registro que almacena todas las transacciones realizadas en un sistema**, asegurando que cada cambio sea verificable y no pueda ser eliminado ni modificado.  

3. **QLDB está optimizado para escenarios financieros y de auditoría**, donde es fundamental garantizar la integridad de los datos, como registros bancarios, sistemas de facturación y rastreo de activos.  

4. **Es una base de datos centralizada, pero con características de inmutabilidad y verificación criptográfica**, a diferencia de las blockchains descentralizadas.  

5. **Ejemplo de un registro en QLDB representado en JSON:**  
   ```json
   {
       "transactionId": "123456",
       "account": "987654",
       "tipo": "pago",
       "monto": 100.50,
       "fecha": "2025-03-15T12:30:00Z"
   }
   ```  

6. **QLDB utiliza un modelo de almacenamiento basado en un diario (journal), donde cada modificación se encadena con un hash criptográfico**, asegurando la integridad de los datos.  

7. **Cada transacción en QLDB se registra de forma secuencial en el diario y no puede ser eliminada ni alterada**, lo que lo hace ideal para casos de uso que requieren un historial verificable.  

8. **El rendimiento de QLDB es hasta 3 veces superior a los frameworks tradicionales de blockchain ledger**, ya que no tiene el costo computacional de la descentralización.  

9. **Permite ejecutar consultas utilizando un lenguaje basado en SQL llamado PartiQL**, lo que facilita su integración con otros sistemas.  

10. **Ejemplo de consulta en PartiQL para obtener el historial de una cuenta:**  
   ```sql
   SELECT * FROM transacciones
   WHERE account = '987654'
   ORDER BY fecha DESC
   ```  

11. **Diferencia clave con Amazon Managed Blockchain:** QLDB no es descentralizado, lo que significa que AWS es la autoridad central, mientras que Managed Blockchain permite una estructura distribuida sin un único punto de control.  

12. **Se integra fácilmente con AWS Lambda, Amazon API Gateway y Amazon Kinesis**, lo que facilita la automatización y el procesamiento de eventos en tiempo real.  

13. **Para el examen de certificación AWS, si se menciona auditoría, rastreo de transacciones financieras o inmutabilidad de datos sin descentralización, la respuesta correcta es Amazon QLDB.**  

# 109. Managed Blockchain Overview  

1. **Amazon Managed Blockchain es un servicio de AWS para crear y gestionar redes de blockchain escalables sin necesidad de configuración manual.**  

2. **Blockchain es una tecnología descentralizada que permite realizar transacciones entre múltiples partes sin necesidad de una autoridad central de confianza.**  

3. **Este servicio permite unirse a redes de blockchain públicas como Ethereum o crear redes privadas basadas en Hyperledger Fabric.**  

4. **Amazon Managed Blockchain elimina la complejidad de aprovisionar hardware, configurar software, gestionar certificados y escalar la red.**  

5. **Hyperledger Fabric es un framework de blockchain diseñado para empresas que requieren control sobre la identidad de los participantes y permisos de acceso.**  

6. **Ethereum, por otro lado, es una blockchain pública descentralizada utilizada para contratos inteligentes y transacciones sin permisos específicos.**  

7. **AWS Managed Blockchain permite escalar automáticamente la red añadiendo o eliminando nodos según la demanda.**  

8. **La administración de miembros y permisos es simplificada mediante el uso de AWS IAM y herramientas integradas de gestión de identidad.**  

9. **Ejemplo de un contrato inteligente simple en Solidity (Ethereum):**  
   ```solidity
   pragma solidity ^0.8.0;

   contract SimpleContract {
       string public message;

       constructor(string memory _message) {
           message = _message;
       }

       function updateMessage(string memory _newMessage) public {
           message = _newMessage;
       }
   }
   ```  

10. **Las organizaciones pueden usar Managed Blockchain para rastrear activos, gestionar cadenas de suministro, registrar transacciones financieras o asegurar datos críticos.**  

11. **Managed Blockchain facilita la integración con otros servicios de AWS, como Amazon S3, AWS Lambda y Amazon QLDB.**  

12. **A diferencia de Amazon QLDB, que ofrece un libro mayor centralizado e inmutable, Managed Blockchain opera en un entorno descentralizado sin una entidad única de control.**  

13. **El modelo de precios de Managed Blockchain se basa en los nodos aprovisionados y el tráfico de datos, permitiendo optimizar costos según la necesidad.**  

14. **Para el examen de certificación AWS, si se menciona blockchain, descentralización, Ethereum o Hyperledger Fabric, la respuesta correcta es Amazon Managed Blockchain.**  

# 110. Glue Overview  

1. **AWS Glue es un servicio completamente gestionado de ETL (Extract, Transform, Load)** diseñado para facilitar la preparación y transformación de datos antes de su análisis.  

2. **ETL es un proceso fundamental en análisis de datos y Big Data**, ya que permite extraer datos de múltiples fuentes, transformarlos en un formato adecuado y cargarlos en un destino para su análisis.  

3. **Glue es un servicio serverless**, lo que significa que no requiere aprovisionamiento ni administración de servidores, reduciendo la complejidad operativa.  

4. **Ejemplo de proceso ETL con AWS Glue:**  
   - Extraer datos desde Amazon S3 y RDS.  
   - Transformar los datos aplicando reglas de limpieza y conversión de formatos.  
   - Cargar los datos en Amazon Redshift para su análisis.  

5. **AWS Glue puede integrarse con diversas fuentes y destinos de datos**, como Amazon S3, RDS, DynamoDB, Redshift y bases de datos externas.  

6. **El procesamiento de datos en Glue se realiza mediante scripts en Python o Scala**, utilizando Apache Spark como motor de ejecución.  

7. **Ejemplo de script en Python para AWS Glue:**  
   ```python
   import sys
   from awsglue.transforms import *
   from awsglue.utils import getResolvedOptions
   from awsglue.context import GlueContext
   from pyspark.context import SparkContext

   sc = SparkContext()
   glueContext = GlueContext(sc)

   # Extraer datos desde S3
   datasource = glueContext.create_dynamic_frame.from_catalog(
       database="mi_base_de_datos",
       table_name="mi_tabla"
   )

   # Transformación simple: Filtrar valores nulos
   transformed_data = Filter.apply(frame=datasource, f=lambda row: row["columna"] is not None)

   # Cargar los datos transformados en Redshift
   glueContext.write_dynamic_frame.from_options(
       frame=transformed_data,
       connection_type="redshift",
       connection_options={"database": "mi_redshift_db", "table_name": "tabla_destino"}
   )
   ```  

8. **AWS Glue incluye el Glue Data Catalog**, un servicio de catalogación que permite rastrear y administrar metadatos sobre los datasets almacenados en la infraestructura de AWS.  

9. **El Glue Data Catalog almacena información sobre nombres de columnas, tipos de datos y relaciones entre tablas**, facilitando el descubrimiento y organización de los datos.  

10. **Glue se integra con servicios de análisis como Amazon Athena, Amazon Redshift y Amazon EMR**, lo que permite realizar consultas SQL sobre los datos procesados.  

11. **Permite la automatización mediante trabajos programados y triggers**, que pueden ejecutarse en función de eventos o en intervalos regulares.  

12. **Glue también incluye crawlers**, que escanean automáticamente los datos en S3 y otras fuentes para inferir su estructura y registrar metadatos en el Data Catalog.  

13. **El modelo de precios de AWS Glue se basa en el uso de "Data Processing Units" (DPU)**, pagando solo por el tiempo de cómputo utilizado durante la ejecución de los procesos ETL.  

14. **Para el examen de certificación AWS, si se menciona transformación de datos, ETL o procesamiento sin servidores, AWS Glue es la respuesta correcta.**  


# 111. DMS Overview  

1. **AWS Database Migration Service (DMS) es un servicio de AWS diseñado para migrar bases de datos de manera rápida, segura y con mínima interrupción.**  

2. **DMS permite migraciones homogéneas (entre bases de datos del mismo tipo, como Oracle a Oracle) y heterogéneas (entre diferentes tecnologías, como SQL Server a Aurora).**  

3. **El proceso de migración involucra tres componentes principales: una base de datos de origen, una instancia de DMS en EC2 que ejecuta el software de migración y una base de datos de destino.**  

4. **Una ventaja clave de DMS es que permite que la base de datos de origen siga operativa durante la migración, minimizando el tiempo de inactividad.**  

5. **El servicio es altamente resiliente y puede recuperarse automáticamente de interrupciones en la red o fallas en la instancia de migración.**  

6. **DMS admite múltiples motores de bases de datos como Oracle, SQL Server, MySQL, PostgreSQL, MariaDB, Aurora y bases de datos NoSQL como DynamoDB.**  

7. **Durante la migración, DMS puede realizar conversiones de esquema cuando la base de datos de destino tiene un formato diferente al de origen.**  

8. **Para migraciones heterogéneas más complejas, AWS recomienda el uso de AWS Schema Conversion Tool (SCT) junto con DMS para transformar estructuras y tipos de datos.**  

9. **Ejemplo de migración desde MySQL a PostgreSQL con DMS:**  
   ```json
   {
     "SourceDatabase": "MySQL",
     "TargetDatabase": "PostgreSQL",
     "ReplicationInstance": "dms-replication-instance",
     "MigrationType": "full-load-and-cdc"
   }
   ```  

10. **DMS admite replicación continua de datos mediante Change Data Capture (CDC), lo que permite que los cambios en la base de datos de origen se reflejen en la de destino en tiempo real.**  

11. **El servicio es completamente administrado por AWS, eliminando la necesidad de gestionar infraestructura o preocuparse por la escalabilidad y el mantenimiento.**  

12. **DMS puede migrar bases de datos hacia AWS (on-premises a la nube), dentro de AWS (de una región a otra) o desde AWS a un entorno local.**  

13. **La facturación de DMS se basa en el tiempo de uso de la instancia de replicación y el volumen de datos transferidos, lo que permite optimizar costos.**  

14. **Para el examen de certificación AWS, si se menciona la migración de bases de datos, especialmente con mínima interrupción, la respuesta correcta será AWS DMS.**  

# 112. Databases & Analytics Summary  

1. **Las bases de datos relacionales en AWS incluyen Amazon RDS y Aurora, diseñadas para OLTP (Online Transaction Processing) y compatibles con SQL.**  

2. **En RDS, es fundamental conocer las diferencias entre Multi-AZ (alta disponibilidad), Read Replicas (escalabilidad en lectura) y Multi-Region (disponibilidad global).**  

3. **Para bases de datos en memoria o almacenamiento en caché, AWS ofrece ElastiCache con soporte para Redis y Memcached.**  

4. **DynamoDB es una base de datos NoSQL orientada a clave-valor, completamente administrada y sin servidor, ideal para cargas de trabajo escalables y de alta velocidad.**  

5. **DAX (DynamoDB Accelerator) es una caché en memoria diseñada específicamente para DynamoDB, reduciendo la latencia de consulta a microsegundos.**  

6. **Para almacenamiento de datos a gran escala y procesamiento OLAP (Online Analytical Processing), Amazon Redshift es la solución ideal en AWS.**  

7. **Amazon EMR permite la creación de clústeres Hadoop para procesamiento de Big Data, integrando tecnologías como Apache Spark y Presto.**  

8. **Athena permite consultas SQL sobre datos almacenados en Amazon S3 sin necesidad de infraestructura, proporcionando análisis rápidos y escalables.**  

9. **QuickSight es la solución de AWS para visualización de datos e inteligencia empresarial, permitiendo la creación de dashboards interactivos.**  

10. **DocumentDB es la versión administrada de MongoDB en AWS, diseñada para almacenar y consultar datos en formato JSON.**  

11. **Amazon QLDB es un servicio de libro mayor para transacciones financieras, proporcionando un historial inmutable y verificable de los datos.**  

12. **Amazon Managed Blockchain permite la creación de redes blockchain basadas en Hyperledger Fabric y Ethereum, con un enfoque en descentralización.**  

13. **AWS Glue es el servicio administrado de AWS para ETL (Extract, Transform, Load), facilitando la preparación y transformación de datos.**  

14. **DMS (Database Migration Service) es la herramienta recomendada para migrar bases de datos dentro de AWS o desde entornos locales.**  

15. **Para bases de datos especializadas, Neptune es la solución para bases de datos de grafos, mientras que Timestream es la mejor opción para datos de series temporales.**  