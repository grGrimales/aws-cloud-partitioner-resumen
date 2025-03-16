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