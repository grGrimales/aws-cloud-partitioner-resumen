Aquí tienes el resumen en formato de viñetas numeradas:  

---

### 51. EBS Overview  

1. **Elastic Block Store (EBS)** es un servicio de almacenamiento en bloque para instancias EC2, permitiendo la persistencia de datos incluso después de la terminación de la instancia.  

2. Los **EBS Volumes** son similares a memorias USB conectadas a través de la red, lo que permite moverlos entre instancias dentro de la misma zona de disponibilidad.  

3. Cada volumen de EBS está **ligado a una zona de disponibilidad específica**, lo que significa que un volumen creado en `us-east-1a` no puede ser adjuntado directamente a una instancia en `us-east-1b`.  

4. Para mover un volumen de EBS entre zonas de disponibilidad, es necesario **crear un snapshot** y restaurarlo en otra zona.  

5. **EBS ofrece 30 GB gratuitos por mes** en volúmenes de tipo General Purpose SSD (GP2, GP3) o Magnetic.  

6. Los **EBS Volumes se comunican a través de la red**, lo que puede generar cierta latencia en las operaciones de lectura y escritura en comparación con los discos locales.  

7. Un volumen de EBS solo puede estar **adjunto a una instancia EC2 a la vez** en el nivel de Certified Cloud Practitioner.  

8. **Es posible adjuntar múltiples volúmenes a una misma instancia**, lo que permite aumentar la capacidad de almacenamiento sin necesidad de modificar la instancia.  

9. Los volúmenes de EBS requieren **provisión de capacidad en GBs y especificación de IOPS (Input/Output Operations Per Second)** en el momento de la creación, lo que afecta el rendimiento y el costo.  

10. **La capacidad de un volumen de EBS puede aumentarse** con el tiempo para mejorar el rendimiento o el almacenamiento disponible.  

11. Existen volúmenes de EBS **no adjuntos**, lo que significa que se pueden crear y mantener sin necesidad de estar asociados a una instancia hasta que sea necesario.  

12. **Delete on Termination** es un atributo que controla si un volumen de EBS se elimina cuando se termina la instancia EC2. Por defecto:  
    - El volumen raíz (`Root Volume`) **se elimina** con la instancia.  
    - Cualquier otro volumen adjunto **no se elimina**, a menos que se configure manualmente.  

13. Si se quiere preservar el almacenamiento del sistema operativo al terminar una instancia EC2, se puede **deshabilitar Delete on Termination** en el volumen raíz.  

14. La configuración de **Delete on Termination** puede cambiarse en la consola de AWS o a través de la CLI, lo que permite controlar la retención de datos después de la eliminación de una instancia.  

15. Un escenario de examen podría preguntar sobre el comportamiento de **Delete on Termination** y cómo evitar la pérdida de datos cuando una instancia EC2 es terminada.  

---

### Ejemplo de código: Creación de un volumen EBS y adjuntarlo a una instancia EC2

```bash
# Crear un volumen EBS de 10GB en us-east-1a
aws ec2 create-volume --size 10 --volume-type gp3 --availability-zone us-east-1a

# Adjuntar el volumen a una instancia EC2
aws ec2 attach-volume --volume-id vol-0123456789abcdef0 --instance-id i-0123456789abcdef0 --device /dev/sdf
```

### Ejemplo de configuración de Delete on Termination:

```bash
# Modificar la configuración para evitar la eliminación del volumen raíz al terminar la instancia
aws ec2 modify-instance-attribute --instance-id i-0123456789abcdef0 --block-device-mappings '[{"DeviceName":"/dev/xvda","Ebs":{"DeleteOnTermination":false}}]'
```

Este resumen cubre los puntos esenciales de la clase en un formato adecuado para el estudio y la preparación para el examen.


Aquí tienes el resumen en formato de viñetas numeradas:

---

### 53. EBS Hands On  

1. Al acceder a una instancia EC2 en la consola de AWS, se puede verificar el almacenamiento en la pestaña **Storage**, donde se muestra el volumen raíz y otros volúmenes adjuntos.  

2. Un volumen EBS se puede visualizar en la consola de **EBS Volumes**, donde se muestra su estado, tamaño y a qué instancia está adjunto.  

3. Para agregar un nuevo volumen EBS, se debe crear en la misma **Zona de Disponibilidad (AZ)** donde está la instancia EC2, ya que los volúmenes están limitados a una zona específica.  

4. Al crear un volumen EBS, se puede elegir el tipo de almacenamiento, como **GP2, GP3 o Magnetic**, y especificar su tamaño en GB.  

5. Una vez creado el volumen, aparece en estado **Available** hasta que se adjunta a una instancia EC2.  

6. Para adjuntar un volumen a una instancia, se debe seleccionar el volumen en la consola, hacer clic en **Attach Volume**, elegir la instancia y confirmar la operación.  

7. Una vez adjuntado, el nuevo volumen aparecerá en la pestaña **Storage** de la instancia EC2 junto con el volumen raíz.  

8. Si un volumen EBS se crea en una **zona de disponibilidad diferente** a la de la instancia EC2, no podrá ser adjuntado a esa instancia.  

9. Un volumen EBS puede eliminarse en cualquier momento seleccionándolo en la consola y eligiendo **Delete Volume**, lo que muestra la flexibilidad del almacenamiento en la nube.  

10. Si una instancia EC2 es terminada, su volumen raíz se elimina automáticamente si tiene activado el atributo **Delete on Termination**.  

11. Para verificar si un volumen tiene habilitado **Delete on Termination**, se debe ir a la pestaña **Storage** de la instancia y revisar los detalles del volumen en la columna correspondiente.  

12. Durante la creación de una instancia EC2, la opción **Delete on Termination** está activada por defecto en el volumen raíz, pero se puede desactivar manualmente si se desea conservarlo tras la terminación de la instancia.  

13. Si una instancia EC2 es terminada y el volumen raíz tiene **Delete on Termination** activado, el volumen se elimina automáticamente de la consola de EBS Volumes.  

14. Los volúmenes adicionales que no tienen activado **Delete on Termination** permanecen en la consola después de que la instancia EC2 es terminada y pueden ser reutilizados en otra instancia.  

15. La flexibilidad de EBS permite crear, adjuntar, eliminar y mover volúmenes de almacenamiento en segundos, lo que demuestra la eficiencia de la infraestructura en la nube.  

16. Para hacer que un volumen EBS adjuntado a una instancia sea utilizable en un sistema Linux, es necesario formatearlo y montarlo en un directorio del sistema, lo cual no es cubierto en esta clase pero puede consultarse en la documentación de AWS.  

---

### Ejemplo de código: Crear y adjuntar un volumen EBS a una instancia EC2

```bash
# Crear un volumen EBS de 2GB en la zona de disponibilidad eu-west-1b
aws ec2 create-volume --size 2 --volume-type gp2 --availability-zone eu-west-1b

# Listar los volúmenes existentes
aws ec2 describe-volumes

# Adjuntar el volumen a una instancia EC2
aws ec2 attach-volume --volume-id vol-0123456789abcdef0 --instance-id i-0123456789abcdef0 --device /dev/sdf
```

### Ejemplo de eliminación de un volumen EBS

```bash
# Eliminar un volumen EBS (debe estar en estado "Available")
aws ec2 delete-volume --volume-id vol-0123456789abcdef0
```

Este resumen cubre los puntos esenciales de la clase en un formato adecuado para el estudio y la preparación para el examen.

### 54. EBS Snapshots Overview  

1. Un **EBS Snapshot** es una copia de seguridad de un volumen EBS en un momento específico, lo que permite restaurar el estado del volumen incluso si este es eliminado.  

2. Para crear un snapshot no es necesario **desmontar el volumen de la instancia**, aunque es recomendable hacerlo para garantizar que los datos sean consistentes.  

3. Los snapshots permiten **migrar volúmenes entre Zonas de Disponibilidad (AZs)** dentro de la misma región, facilitando la transferencia de datos sin necesidad de copiar manualmente los archivos.  

4. Para mover un volumen entre zonas, se debe tomar un snapshot, restaurarlo en una nueva zona y adjuntar el nuevo volumen a una instancia en la zona de destino.  

5. Los snapshots también pueden **copiarse entre regiones**, lo que facilita la replicación de datos y la recuperación ante desastres en diferentes partes del mundo.  

6. **Los snapshots no son volúmenes completos**, sino copias incrementales, lo que significa que solo almacenan los cambios realizados desde el último snapshot, optimizando el almacenamiento y reduciendo costos.  

7. AWS ofrece la opción de **EBS Snapshot Archive**, un nivel de almacenamiento más económico (75% más barato) para snapshots que no se necesitan con frecuencia.  

8. Restaurar un snapshot desde el **archivo de EBS** puede tardar entre **24 y 72 horas**, por lo que solo es recomendable para datos que no requieren acceso inmediato.  

9. Para evitar pérdidas accidentales, AWS ofrece la función **Recycle Bin para EBS Snapshots**, donde los snapshots eliminados pueden recuperarse en un período configurable de **un día a un año**.  

10. Los snapshots en el **Recycle Bin** permiten restaurar datos eliminados por error antes de que sean eliminados permanentemente.  

11. Cuando se elimina un snapshot con el **Recycle Bin activado**, este no desaparece de inmediato, sino que queda almacenado hasta que el tiempo de retención configurado expire.  

12. La gestión de snapshots es clave en estrategias de **backup y recuperación de desastres**, asegurando que los datos críticos puedan restaurarse en caso de fallos o ataques.  

13. AWS permite automatizar la creación de snapshots mediante **Lifecycle Policies**, configurando reglas para generar y eliminar snapshots de manera periódica.  

---

### Ejemplo de código: Crear y restaurar un snapshot  

```bash
# Crear un snapshot de un volumen EBS
aws ec2 create-snapshot --volume-id vol-0123456789abcdef0 --description "Backup del volumen principal"

# Listar snapshots disponibles
aws ec2 describe-snapshots --owner-id 123456789012

# Crear un nuevo volumen a partir del snapshot
aws ec2 create-volume --snapshot-id snap-0123456789abcdef0 --availability-zone us-east-1b
```

### Ejemplo de configuración del Recycle Bin  

```bash
# Crear una regla de retención en Recycle Bin para snapshots eliminados
aws recycle-bin create-rule --rule-name "Retención de Snapshots" --retention-period 30 --resource-type "SNAPSHOT"
```

Este resumen proporciona los puntos clave de la clase en un 
formato ideal para estudio y preparación para el examen.

### 55. EBS Snapshots Hands On

1. Un **EBS Snapshot** permite crear respaldos incrementales de un volumen EBS para proteger datos o transferir información entre zonas.

2. Para crear un snapshot desde la consola de AWS, basta seleccionar un volumen EBS y luego la opción **Actions > Create snapshot**.

3. Una vez creado un snapshot, este muestra información como tamaño, estado, progreso y disponibilidad; cuando alcanza el estado **Completed (100%)**, está listo para utilizarse.

4. Los snapshots pueden copiarse fácilmente entre **regiones de AWS**, proporcionando una solución para recuperación ante desastres o respaldo geográfico.

5. Para copiar un snapshot a otra región, se utiliza la opción **Actions > Copy snapshot** en la consola, seleccionando la región de destino.

6. Los snapshots también permiten **crear nuevos volúmenes EBS** en cualquier Zona de Disponibilidad dentro de la misma región.

7. Para crear un volumen a partir de un snapshot, seleccionar el snapshot en la consola y luego elegir **Actions > Create volume from snapshot**, especificando tipo y tamaño.

8. Al crear volúmenes desde snapshots, es posible elegir diferentes Zonas de Disponibilidad, facilitando la migración entre AZs.

8. Al crear un snapshot, no es obligatorio detener o desconectar el volumen EBS, aunque detener la instancia temporalmente puede asegurar la consistencia del respaldo.

9. Los snapshots pueden cambiar su **nivel de almacenamiento** para reducir costos, migrándolos desde el estándar hacia un nivel de archivo (Archive Storage Tier).

10. Restaurar un snapshot desde el nivel de archivo puede tardar más tiempo, por lo que es recomendable solo para datos menos críticos.

11. AWS ofrece la funcionalidad de **Recycle Bin** para proteger snapshots contra eliminaciones accidentales, permitiendo restaurarlos en un período definido.

12. Para activar la papelera de reciclaje, se configura una regla indicando el tiempo de retención de los snapshots eliminados (desde 1 día hasta 1 año).

13. Al eliminar un snapshot con Recycle Bin activado, éste puede recuperarse posteriormente desde la sección **Recycle Bin** en la consola AWS.

---

### Ejemplo de creación y copia de snapshot:

```bash
# Crear un snapshot de un volumen específico
aws ec2 create-snapshot --volume-id vol-0123456789abcdef0 --description "Backup volumen prod-db"

# Copiar un snapshot a otra región (ej. us-east-1)
aws ec2 copy-snapshot --source-region eu-west-1 --source-snapshot-id snap-0123456789abcdef0 --destination-region us-east-1
```

### Ejemplo para restaurar volumen desde snapshot:

```bash
# Crear un volumen nuevo desde un snapshot
aws ec2 create-volume --snapshot-id snap-0123456789abcdef0 --volume-type gp2 --availability-zone eu-west-1a
```

### Ejemplo de configuración del Recycle Bin:

```bash
# Crear regla de retención de Recycle Bin para snapshots eliminados
aws recycle-bin create-rule \
  --rule-name "ProteccionSnapshots" \
  --retention-period 7 \
  --resource-type "SNAPSHOT"
```

Este documento es ideal para repasar y dominar conceptos claves sobre EBS Snapshots en AWS.

## 56. AMI Overview

1. Una **AMI (Amazon Machine Image)** es una imagen preconfigurada que contiene el sistema operativo, aplicaciones, configuraciones y datos necesarios para lanzar una instancia EC2 rápidamente.

2. Las AMIs facilitan un **inicio más rápido de las instancias EC2**, pues todo el software y configuraciones ya vienen empaquetados en la imagen.

3. Existen tres tipos principales de AMIs: proporcionadas por AWS (públicas), personalizadas por los usuarios, y las del **AWS Marketplace** creadas por terceros.

4. Las AMIs proporcionadas por AWS, como **Amazon Linux 2**, están optimizadas y listas para usarse inmediatamente sin configuración adicional.

5. Las AMIs personalizadas permiten al usuario **preinstalar software y configuraciones específicas**, reduciendo considerablemente el tiempo necesario para preparar instancias en el futuro.

6. Es posible crear una AMI propia a partir de una instancia EC2 en ejecución, pero es recomendable **detener la instancia antes de crear la AMI** para asegurar que los datos estén íntegros y consistentes.

7. Durante el proceso de creación de una AMI, AWS automáticamente crea **snapshots de los volúmenes EBS** vinculados a dicha instancia.

8. Las AMIs son específicas para una región, aunque se pueden **copiar fácilmente a otras regiones de AWS** para aprovechar la infraestructura global y mejorar la resiliencia.

9. Las AMIs del **AWS Marketplace** permiten utilizar imágenes creadas y mantenidas por terceros, simplificando el despliegue de software especializado o configuraciones avanzadas mediante una compra directa.

10. El AWS Marketplace también ofrece la posibilidad de que empresas o desarrolladores puedan crear un modelo de negocio, **vendiendo sus propias AMIs** personalizadas y generando ingresos.

11. Usar AMIs personalizadas facilita la implementación de una estrategia robusta de **recuperación ante desastres y escalabilidad** al permitir lanzar múltiples instancias idénticas en poco tiempo.

12. Aunque no es obligatorio, se recomienda **automatizar la creación y mantenimiento de AMIs** mediante herramientas de infraestructura como código (por ejemplo, usando AWS CLI, Terraform o CloudFormation).

13. Crear AMIs personalizadas mejora la eficiencia operativa, especialmente en entornos dinámicos, reduciendo considerablemente los tiempos de **boot y configuración** al lanzar nuevas instancias EC2.

14. Las AMIs pueden formar parte clave de arquitecturas avanzadas en la nube, permitiendo gestionar eficientemente ambientes de desarrollo, pruebas y producción.

---

### Ejemplo de código para crear y copiar AMI:

```bash
# Crear AMI a partir de una instancia EC2
aws ec2 create-image --instance-id i-0abcdef1234567890 --name "MiAMI-Personalizada" --description "AMI con aplicaciones preinstaladas"

# Copiar AMI a otra región (por ejemplo de us-east-1 a eu-west-1)
aws ec2 copy-image --source-image-id ami-0123456789abcdef0 --source-region us-east-1 --region eu-west-1 --name "Copia-AMI-EU"
```

### Ejemplo para lanzar una instancia desde una AMI personalizada:

```bash
# Lanzar nueva instancia EC2 usando AMI personalizada
aws ec2 run-instances --image-id ami-0123456789abcdef0 --instance-type t2.micro --key-name ClaveAcceso --security-group-ids sg-0123456789abcdef0 --subnet-id subnet-0123456789abcdef0
```

Este documento es una herramienta práctica para estudiar y reforzar conceptos esenciales relacionados con las AMIs en AWS.

## 57. AMI Hands On

1. Una **AMI (Amazon Machine Image)** permite lanzar instancias EC2 con configuraciones predefinidas, acelerando el proceso de arranque y despliegue.

2. La creación de una AMI personalizada se inicia con el lanzamiento de una instancia EC2, en la que se instalan y configuran las herramientas necesarias.

3. Es común utilizar scripts como **user data** para instalar automáticamente el software requerido al iniciar la instancia, agilizando la configuración inicial.

4. El proceso de creación de una AMI implica detener la instancia original para garantizar la consistencia e integridad del sistema antes de generar la imagen.

5. Al crear una AMI, AWS realiza automáticamente snapshots de los volúmenes EBS asociados para preservar los datos y la configuración del sistema operativo.

6. Una vez creada, la AMI puede usarse repetidamente para lanzar nuevas instancias con una configuración idéntica, simplificando tareas repetitivas y ahorrando tiempo.

7. La AMI personalizada aparece en la consola de AWS en la sección **Images/AMIs**, con el estado inicialmente en "pendiente" hasta que esté completamente disponible.

8. Al utilizar una AMI personalizada para lanzar una nueva instancia EC2, el tiempo necesario para instalar software y configuraciones se reduce significativamente.

9. En caso de querer desplegar instancias en distintas Zonas de Disponibilidad dentro de una región, se puede utilizar la misma AMI creada, sin necesidad de configuraciones adicionales.

10. La opción de copiar AMIs entre diferentes regiones facilita implementar rápidamente infraestructuras redundantes o migrar aplicaciones a otras regiones para cumplir requisitos regulatorios.

11. El uso de AMIs reduce considerablemente la posibilidad de errores humanos en la configuración inicial de las instancias, aumentando la consistencia en implementaciones a gran escala.

12. El tiempo necesario para que una AMI personalizada pase del estado **Pending** al estado **Available** depende principalmente del tamaño del volumen EBS asociado y la cantidad de datos que contenga.

12. Las AMIs personalizadas son fundamentales en escenarios empresariales que requieren despliegues rápidos y consistentes, especialmente en ambientes escalables o de recuperación ante desastres.

13. Es posible automatizar la creación y gestión de AMIs con herramientas como AWS EC2 Image Builder, facilitando el mantenimiento de versiones actualizadas de las imágenes.

14. AWS permite compartir AMIs entre cuentas, lo que facilita la colaboración dentro de organizaciones grandes o entre socios comerciales.

15. Las AMIs también pueden venderse en el **AWS Marketplace**, proporcionando oportunidades comerciales para quienes desarrollen configuraciones especializadas o soluciones listas para ser usadas por terceros.

---

### Ejemplo para crear una AMI con AWS CLI:

```bash
# Crear una AMI desde una instancia EC2 existente
aws ec2 create-image \
  --instance-id i-0123456789abcdef0 \
  --name "MiAMIpersonalizada" \
  --description "AMI con Apache instalado"
```

### Ejemplo para lanzar una instancia desde AMI con user data:

```bash
# Lanzar instancia EC2 desde una AMI personalizada con user data
aws ec2 run-instances \
  --image-id ami-0123456789abcdef0 \
  --instance-type t2.micro \
  --key-name MiClaveSSH \
  --security-group-ids sg-0123456789abcdef0 \
  --user-data file://userdata.txt
```

_Contenido del archivo `userdata.txt` para instalar Apache automáticamente:_
```bash
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "Hola desde AMI" > /var/www/html/index.html
```

Este documento ayuda a comprender profundamente cómo crear y gestionar AMIs para optimizar despliegues en AWS.

# 58. EC2 Image Builder Overview

1. **EC2 Image Builder** es un servicio de AWS que automatiza la creación y gestión continua de AMIs para instancias EC2 y contenedores.

2. Facilita el proceso automático de **creación, mantenimiento, validación y distribución** de AMIs, asegurando consistencia y actualizaciones frecuentes del software.

3. Image Builder permite automatizar la instalación de software específico y configuraciones requeridas dentro de una imagen, evitando tareas manuales repetitivas.

4. Al ejecutar un proceso de creación de AMI, Image Builder lanza automáticamente una **instancia EC2 Builder**, que contiene todas las configuraciones necesarias definidas en el proceso.

5. Una vez configurada automáticamente la instancia, Image Builder procede a **crear una AMI personalizada** basada en esa instancia de referencia.

6. Después de crear la AMI, EC2 Image Builder puede lanzar automáticamente una **instancia EC2 de prueba** usando la AMI para asegurar su correcto funcionamiento.

7. Si la validación automatizada es exitosa, la AMI puede distribuirse automáticamente a diferentes **regiones AWS**, facilitando un despliegue global y ágil.

8. EC2 Image Builder soporta distribución multi-región, lo que simplifica significativamente estrategias de alta disponibilidad y recuperación ante desastres.

9. El uso de Image Builder permite reducir significativamente el tiempo y esfuerzo manual necesarios para actualizar imágenes, acelerando el ciclo de despliegue de aplicaciones.

10. Aunque EC2 Image Builder es un servicio gratuito, el usuario paga por los recursos subyacentes utilizados durante la creación y almacenamiento, como las instancias EC2 temporales y el almacenamiento EBS de las AMIs generadas.

11. La automatización con Image Builder reduce riesgos asociados con errores humanos, brindando un proceso consistente y repetible para creación de AMIs.

11. Image Builder ofrece la opción de distribuir automáticamente AMIs a **múltiples regiones de AWS**, facilitando una arquitectura global, altamente disponible y tolerante a fallos.

12. Las AMIs generadas por EC2 Image Builder pueden ser fácilmente integradas en **pipeline CI/CD**, optimizando el flujo de trabajo del desarrollo y despliegue de aplicaciones.

13. Image Builder es ideal para organizaciones que requieren un enfoque sistemático para la creación y actualización constante de imágenes personalizadas, aumentando la seguridad y eficiencia operacional.

---

### Ejemplo básico de código para automatizar Image Builder mediante AWS CLI:

```bash
# Crear un pipeline de EC2 Image Builder
aws imagebuilder create-image-pipeline \
  --name "mi-pipeline-ami" \
  --image-recipe-arn arn:aws:imagebuilder:us-east-1:123456789012:image-recipe/mi-receta/1.0.0 \
  --infrastructure-configuration-arn arn:aws:imagebuilder:us-east-1:123456789012:infrastructure-configuration/mi-configuracion-infraestructura \
  --distribution-configuration-arn arn:aws:imagebuilder:us-east-1:123456789012:distribution-configuration/mi-distribucion
```

### Ejemplo de automatización del proceso de distribución entre regiones:

```bash
# Copiar AMI generada por Image Builder a otra región
aws ec2 copy-image \
  --source-image-id ami-0123456789abcdef0 \
  --source-region us-east-1 \
  --region eu-west-1 \
  --name "AMI-Copia-Europa"
```

Este resumen provee un estudio detallado para dominar EC2 Image Builder y aplicarlo en escenarios prácticos empresariales.