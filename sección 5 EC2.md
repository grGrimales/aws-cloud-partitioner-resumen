Aquí tienes el resumen en formato de viñetas numeradas:

---

### 34. AWS Budget Setup

1. Para evitar gastos innecesarios en AWS, es fundamental configurar un presupuesto y una alerta de gasto. Esto se hace desde la consola de facturación, accesible a través del menú en la esquina superior derecha de la pantalla.

2. Los usuarios IAM con acceso administrativo no pueden ver los datos de facturación de AWS por defecto. Para habilitarlo, es necesario acceder con la cuenta raíz y activar el acceso a la facturación en la sección de "Accounts".

3. Una vez activado el acceso a la facturación para los usuarios IAM, es posible visualizar información detallada sobre los costos en la consola de facturación, incluyendo costos acumulados del mes, pronósticos de gastos y costos del mes anterior.

4. En la sección de facturación, es posible analizar el desglose de costos por mes, identificando el número de servicios activos y los cargos asociados a cada uno. Por ejemplo, en Amazon EC2, se puede ver cuánto cuesta cada instancia, el uso de Elastic IPs y otros recursos asociados.

5. Para revisar los cargos detallados por servicio, se debe acceder a la sección de facturación, seleccionar un mes específico y desplazarse hasta la parte inferior de la página para ver el desglose de costos por servicio.

6. AWS ofrece un nivel gratuito (Free Tier), donde se puede monitorear el uso actual y el pronóstico de consumo. Si se excede el límite gratuito, AWS lo marca en rojo y los recursos comienzan a generar costos adicionales.

7. Configurar un presupuesto en AWS permite recibir alertas cuando se alcanzan ciertos umbrales de gasto. Para ello, se debe acceder a la sección de "Budgets" en la consola de facturación.

8. El presupuesto "Zero Spend Budget" se configura para recibir una alerta cuando el gasto alcance 1 centavo. Este tipo de presupuesto es útil para evitar cargos inesperados.

9. También se puede configurar un presupuesto mensual, estableciendo un límite de gasto, como por ejemplo $10 al mes. Esto ayuda a controlar los costos del curso y evitar gastos innecesarios.

10. AWS permite configurar alertas en distintos niveles de gasto: por ejemplo, recibir un correo electrónico cuando se alcance el 85% del presupuesto y otro cuando se alcance el 100%.

11. Una vez creado el presupuesto, si se ha generado algún gasto, se recibe una notificación inmediata con el detalle del gasto y la alerta configurada.

12. La combinación de presupuestos, alertas y monitoreo del Free Tier permite identificar rápidamente cualquier anomalía en los costos y corregir problemas antes de que generen facturas elevadas.

13. Controlar y analizar los costos en AWS es una habilidad esencial para evitar gastos imprevistos y administrar eficientemente los recursos en la nube.

---

### Ejemplo de código: Configuración de un presupuesto en AWS con AWS CLI

Para crear un presupuesto de gasto cero usando AWS CLI, se puede ejecutar el siguiente comando:

```sh
aws budgets create-budget \
  --account-id 123456789012 \
  --budget '{"BudgetName": "ZeroSpendBudget", "BudgetLimit": {"Amount": "0.01", "Unit": "USD"}, "TimeUnit": "MONTHLY", "BudgetType": "COST"}' \
  --notifications-with-subscribers '[{"Notification": {"NotificationType": "ACTUAL", "ComparisonOperator": "GREATER_THAN", "Threshold": 0.01}, "Subscribers": [{"SubscriptionType": "EMAIL", "Address": "stephane@example.com"}]}]'
```

Este comando crea un presupuesto mensual con un límite de $0.01 y envía un correo de alerta cuando se excede.

---
### 35. EC2 Basics

1. Amazon EC2 (Elastic Compute Cloud) es uno de los servicios más populares de AWS y representa la base de la infraestructura como servicio (IaaS). Permite alquilar máquinas virtuales en la nube con una configuración flexible y escalable según las necesidades del usuario.

2. Una instancia EC2 es un servidor virtual alquilado en AWS. Puede ejecutarse en diferentes sistemas operativos como Linux, Windows o Mac OS, dependiendo de los requerimientos del usuario.

3. La configuración de una instancia EC2 implica elegir la cantidad de CPU, la memoria RAM, el almacenamiento y la red. Existen múltiples opciones de almacenamiento, como volúmenes EBS (Elastic Block Store), almacenamiento de red EFS (Elastic File System) o almacenamiento en disco físico de la instancia (Instance Store).

4. La conectividad de una instancia EC2 se gestiona a través de redes virtuales. Se puede configurar el tipo de tarjeta de red, asignar direcciones IP públicas o privadas y definir reglas de acceso mediante grupos de seguridad (Security Groups).

5. El **EC2 User Data** permite ejecutar un script automáticamente en el primer arranque de la instancia. Este mecanismo se conoce como "bootstrapping" y facilita la configuración inicial, como la instalación de software o la descarga de archivos.

6. Un script de **User Data** solo se ejecuta una vez, en el primer arranque de la instancia. Se utiliza para automatizar tareas como actualizaciones de paquetes, configuración de servicios y descarga de dependencias.

7. **Ejemplo de un User Data en una instancia Linux** que instala un servidor web Apache automáticamente:

   ```bash
   #!/bin/bash
   yum update -y
   yum install httpd -y
   systemctl start httpd
   systemctl enable httpd
   echo "Hola, mundo desde mi instancia EC2" > /var/www/html/index.html
   ```

   Este script actualiza el sistema, instala Apache, lo inicia y crea una página web básica.

8. Existen diferentes tipos de instancias EC2 con características variadas en CPU, memoria y rendimiento de red. Por ejemplo, una **t2.micro** tiene 1 vCPU y 1 GB de RAM, mientras que una **c5d.4xlarge** tiene 16 vCPU, 32 GB de RAM y almacenamiento SSD NVMe.

9. La selección del tipo de instancia depende de la carga de trabajo. Para aplicaciones ligeras o de prueba, un **t2.micro** es suficiente, mientras que cargas intensivas en procesamiento pueden requerir instancias de alto rendimiento como **r5.16xlarge**.

10. AWS permite escalar las instancias de EC2 automáticamente mediante grupos de escalado automático (**Auto Scaling Groups**), que ajustan la cantidad de instancias en función de la demanda.

11. AWS ofrece un nivel gratuito para EC2, que permite utilizar hasta **750 horas al mes** de una instancia **t2.micro**, lo que equivale a ejecutarla continuamente durante un mes sin costo adicional.

12. Para administrar la seguridad de las instancias, se utilizan **Security Groups**, que actúan como firewalls virtuales y controlan el tráfico de entrada y salida, permitiendo solo conexiones autorizadas.

13. EC2 es una de las tecnologías clave en la nube de AWS, ya que permite crear servidores bajo demanda en cuestión de minutos. Su flexibilidad y capacidad de escalado lo hacen esencial para cualquier arquitectura basada en la nube.

### 36. Create an EC2 Instance with EC2 User Data to have a Website Hands On

1. Para lanzar una instancia EC2 en AWS, se accede a la consola de EC2, se selecciona la opción "Launch Instances" y se proporciona un nombre a la instancia, como "My First Instance".

2. La imagen de máquina de Amazon (AMI) define el sistema operativo de la instancia. Se elige **Amazon Linux 2**, que es elegible para el nivel gratuito y está optimizado para trabajar con AWS.

3. El tipo de instancia determina la capacidad de cómputo. Se selecciona **t2.micro**, que tiene 1 vCPU y 1 GB de RAM, suficiente para pruebas y dentro del nivel gratuito de AWS.

4. Para acceder a la instancia mediante SSH, se necesita un **par de claves (key pair)**. Se genera un nuevo par llamado **EC2 Tutorial**, con formato **.pem** para Mac/Linux y **.ppk** para versiones antiguas de Windows.

5. La configuración de red asigna una dirección IP pública a la instancia, permitiendo su acceso desde Internet. Se crea automáticamente un **grupo de seguridad (Security Group)** para definir reglas de acceso.

6. Se agregan reglas al **Security Group** para permitir el tráfico SSH (puerto 22) y HTTP (puerto 80). SSH permite acceso remoto a la instancia, y HTTP es necesario para hospedar un sitio web.

7. La configuración de almacenamiento define un volumen **EBS (Elastic Block Store)** de 8 GB con SSD de propósito general (**gp2**). AWS permite hasta 30 GB en el nivel gratuito.

8. Se habilita la opción **"Delete on Termination"**, asegurando que el almacenamiento asociado a la instancia se elimine automáticamente al finalizar la instancia.

9. En la sección **User Data**, se define un script que se ejecuta automáticamente en el primer arranque de la instancia, permitiendo la instalación de un servidor web.

10. **Ejemplo de script User Data** para instalar un servidor web Apache en Amazon Linux:

    ```bash
    #!/bin/bash
    yum update -y
    yum install httpd -y
    systemctl start httpd
    systemctl enable httpd
    echo "Hello World from $(hostname -I)" > /var/www/html/index.html
    ```

    Este script instala Apache, lo inicia y genera una página HTML con la dirección IP de la instancia.

11. La instancia se lanza con la configuración definida y aparece en estado **pending**. En unos segundos, cambia a **running**, lo que indica que ya está operativa.

12. Cada instancia EC2 tiene una **dirección IP pública** asignada dinámicamente. Si la instancia se detiene y se inicia nuevamente, su IP pública puede cambiar.

13. Para acceder al servidor web, se copia la **IP pública** y se ingresa en un navegador con el prefijo **http://**, asegurando que no se use HTTPS, ya que la instancia no tiene un certificado SSL configurado.

14. Si la página no carga inmediatamente, se recomienda esperar unos minutos y refrescar la página, ya que puede tardar un poco en activarse el servidor web.

15. AWS permite **detener una instancia EC2** cuando no se está usando para evitar costos. Una instancia detenida no genera costos de cómputo, pero el almacenamiento EBS asociado sigue acumulando costos.

16. **Ejemplo de comando para detener una instancia EC2** desde la CLI:

    ```sh
    aws ec2 stop-instances --instance-ids i-0123456789abcdef0
    ```

17. Se puede **eliminar una instancia EC2** cuando ya no se necesita. Al eliminarla, su almacenamiento asociado también se borra si la opción **Delete on Termination** está activada.

18. **Ejemplo de comando para terminar una instancia EC2** desde la CLI:

    ```sh
    aws ec2 terminate-instances --instance-ids i-0123456789abcdef0
    ```

19. Si la instancia se detiene y se inicia nuevamente, AWS puede asignarle una nueva dirección IP pública, pero la **IP privada permanece constante** dentro de la red de AWS.

20. Para evitar que la IP pública cambie al reiniciar la instancia, se puede asignar una **Elastic IP**, que es una IP pública estática asociada a la instancia EC2.

21. Con esta práctica, se ha desplegado un servidor web en la nube de AWS usando EC2, aprovechando la automatización con **User Data**, la configuración de seguridad con **Security Groups** y el almacenamiento **EBS**, demostrando la flexibilidad y rapidez de la computación en la nube.

### 37. EC2 Instance Types Basics

1. Las instancias EC2 en AWS están categorizadas en distintos tipos optimizados para diferentes casos de uso, incluyendo instancias de propósito general, optimizadas para cómputo, memoria y almacenamiento.

2. La nomenclatura de las instancias EC2 sigue un patrón donde la primera letra representa la clase de instancia, el número indica la generación del hardware y el sufijo define el tamaño de la instancia. Por ejemplo, **M5.2xlarge** representa una instancia de propósito general de quinta generación con un tamaño **2xlarge**.

3. Las **instancias de propósito general** ofrecen un balance entre cómputo, memoria y red. Son ideales para servidores web, repositorios de código y aplicaciones generales. Ejemplo: **T2.micro**, que es parte del nivel gratuito de AWS.

4. **Ejemplo de lanzamiento de una instancia T2.micro con AWS CLI**:

   ```sh
   aws ec2 run-instances --image-id ami-12345678 --count 1 --instance-type t2.micro --key-name my-key --security-groups my-security-group
   ```

5. Las **instancias optimizadas para cómputo** están diseñadas para tareas que requieren alto rendimiento de CPU, como procesamiento de datos por lotes, transcodificación de medios, servidores de juegos y aprendizaje automático. Se identifican con la letra **C** (ejemplo: **C5**, **C6**).

6. **Ejemplo de instancia optimizada para cómputo**:

   ```sh
   aws ec2 run-instances --image-id ami-12345678 --instance-type c5.large --count 1 --key-name my-key
   ```

7. Las **instancias optimizadas para memoria** son ideales para cargas de trabajo que procesan grandes volúmenes de datos en RAM, como bases de datos relacionales y NoSQL, almacenamiento en caché y análisis de datos en tiempo real. Se identifican con la letra **R** (ejemplo: **R5.16xlarge**).

8. **Ejemplo de instancia optimizada para memoria**:

   ```sh
   aws ec2 run-instances --image-id ami-12345678 --instance-type r5.large --count 1 --key-name my-key
   ```

9. Las **instancias optimizadas para almacenamiento** están diseñadas para aplicaciones que requieren alto rendimiento en lectura y escritura de datos en almacenamiento local. Son útiles para bases de datos transaccionales, sistemas de archivos distribuidos y procesamiento de datos masivos. Se identifican con las letras **I**, **G** o **H** (ejemplo: **I3.large**).

10. **Ejemplo de instancia optimizada para almacenamiento**:

    ```sh
    aws ec2 run-instances --image-id ami-12345678 --instance-type i3.large --count 1 --key-name my-key
    ```

11. Comparando tipos de instancias, una **T2.micro** tiene 1 vCPU y 1 GB de RAM, mientras que una **R5.16xlarge** tiene 16 vCPU y 512 GB de RAM, mostrando un enfoque en el rendimiento de memoria.

12. Una **C5d.4xlarge**, con 16 vCPU y 32 GB de RAM, tiene menos memoria pero más potencia de cómputo, ideal para aplicaciones que dependen más del procesamiento que de la memoria.

13. AWS permite visualizar y comparar todas las instancias disponibles en su sitio web, proporcionando detalles sobre costos, memoria y rendimiento de red.

14. Existen herramientas externas como **instanceinstances.info**, que permiten comparar características y precios de diferentes instancias EC2 de manera más accesible.

15. La selección del tipo de instancia adecuado depende del caso de uso específico, asegurando que los recursos asignados sean los óptimos para la carga de trabajo y evitando costos innecesarios en AWS.

### 38. Security Groups & Classic Ports Overview

1. Los **Security Groups** en AWS actúan como firewalls virtuales para las instancias EC2, controlando el tráfico entrante (**inbound**) y saliente (**outbound**) basado en reglas definidas por el usuario.

2. A diferencia de los firewalls tradicionales, los Security Groups **solo contienen reglas de permisos**, es decir, permiten tráfico específico pero no bloquean tráfico de forma explícita.

3. Se pueden definir reglas de seguridad basadas en direcciones IP o en otros Security Groups, lo que permite una gestión flexible del acceso a las instancias EC2.

4. Cada Security Group puede aplicarse a **múltiples instancias EC2**, y una instancia EC2 puede estar asociada a **varios Security Groups** simultáneamente.

5. Los Security Groups están **ligados a una región y una VPC específica**, lo que significa que si se cambia de región o de VPC, se deben configurar nuevos Security Groups.

6. El tráfico bloqueado por un Security Group nunca llega a la instancia EC2, ya que el firewall opera externamente, impidiendo cualquier intento de acceso no autorizado.

7. Para mejorar la seguridad y administración, se recomienda **tener un Security Group separado exclusivamente para acceso SSH (puerto 22)** y otro para la aplicación.

8. Si una aplicación en EC2 no responde y se obtiene un **timeout**, es probable que el Security Group esté bloqueando el tráfico de entrada. Si se recibe un mensaje de **"Connection Refused"**, significa que el tráfico llegó a la instancia pero el servicio no está corriendo o configurado correctamente.

9. **Por defecto, todo el tráfico de entrada está bloqueado y todo el tráfico de salida está permitido**. Esto significa que una instancia EC2 puede acceder a Internet, pero nadie puede conectarse a ella sin reglas explícitas.

10. Es posible referenciar un Security Group dentro de otro Security Group, lo que permite definir permisos entre instancias sin necesidad de conocer sus direcciones IP.

11. **Ejemplo de configuración de un Security Group** con acceso SSH y HTTP usando AWS CLI:

    ```sh
    aws ec2 create-security-group --group-name MySecurityGroup --description "Security group for SSH and HTTP access"
    
    aws ec2 authorize-security-group-ingress --group-name MySecurityGroup --protocol tcp --port 22 --cidr 0.0.0.0/0
    
    aws ec2 authorize-security-group-ingress --group-name MySecurityGroup --protocol tcp --port 80 --cidr 0.0.0.0/0
    ```

12. Existen **puertos estándar** en redes, y los más importantes en AWS incluyen:
    - **22 (SSH):** acceso remoto seguro a servidores Linux.
    - **21 (FTP):** transferencia de archivos no segura.
    - **22 (SFTP):** transferencia de archivos segura mediante SSH.
    - **80 (HTTP):** tráfico web sin cifrar.
    - **443 (HTTPS):** tráfico web cifrado con SSL/TLS.
    - **3389 (RDP):** acceso remoto a servidores Windows.

13. **Ejemplo de configuración de un Security Group** permitiendo acceso a un servidor web en HTTPS:

    ```sh
    aws ec2 authorize-security-group-ingress --group-name WebServerSG --protocol tcp --port 443 --cidr 0.0.0.0/0
    ```

14. Al utilizar **Load Balancers**, es común definir Security Groups que solo permitan tráfico desde otros Security Groups, asegurando que solo instancias específicas puedan comunicarse entre sí.

15. Si se crea una nueva instancia EC2 en la misma VPC, se puede autorizar su acceso a otra instancia usando Security Groups en lugar de direcciones IP, evitando la necesidad de actualizaciones manuales.

16. Comprender el funcionamiento de los Security Groups es esencial para la seguridad en la nube, asegurando que solo el tráfico autorizado pueda acceder a las instancias EC2 y evitando configuraciones que expongan los recursos a ataques externos.

### 39. Security Groups Hands On

1. Los **inbound rules** en un Security Group controlan el tráfico entrante hacia una instancia EC2. Para permitir conexiones SSH y HTTP, se configuran reglas que habilitan los puertos **22 (SSH)** y **80 (HTTP)** desde cualquier origen (`0.0.0.0/0` para IPv4 y `::/0` para IPv6).

2. Si se elimina la regla de **puerto 80**, el acceso HTTP a la instancia EC2 se bloquea. Esto se manifiesta en el navegador con una **pantalla de carga infinita**, indicando que la conexión no puede establecerse.

3. Un **timeout** al intentar conectar por SSH, HTTP u otro servicio indica que el problema está en los Security Groups. Si la regla adecuada no está configurada, la conexión simplemente no llegará a la instancia.

4. Para solucionar un bloqueo por Security Groups, se debe agregar nuevamente la regla correspondiente. Si se restablece la regla para **HTTP (puerto 80)** desde cualquier origen, la página web en la instancia vuelve a ser accesible.

5. AWS permite agregar reglas personalizadas para puertos específicos. Por ejemplo, para habilitar **HTTPS (puerto 443)**, se puede configurar manualmente o seleccionar la opción predefinida en la consola.

6. Se pueden definir reglas de acceso basadas en direcciones IP. Si se elige **"My IP"**, solo la dirección IP del usuario actual podrá acceder, pero si cambia la IP, el acceso se perderá.

7. **Ejemplo de configuración de un Security Group en AWS CLI** que habilita SSH y HTTP desde cualquier origen:

   ```sh
   aws ec2 create-security-group --group-name WebAccess --description "Security group for SSH and HTTP"

   aws ec2 authorize-security-group-ingress --group-name WebAccess --protocol tcp --port 22 --cidr 0.0.0.0/0

   aws ec2 authorize-security-group-ingress --group-name WebAccess --protocol tcp --port 80 --cidr 0.0.0.0/0
   ```

8. Las **outbound rules** controlan el tráfico saliente desde la instancia. De manera predeterminada, AWS permite que la instancia EC2 pueda comunicarse con cualquier dirección IP en Internet.

9. Un **Security Group puede asociarse a múltiples instancias EC2**, lo que permite reutilizar reglas de seguridad en diferentes servidores sin necesidad de crear un nuevo grupo para cada instancia.

10. Una instancia EC2 también puede tener **múltiples Security Groups asociados simultáneamente**, combinando reglas de acceso de cada uno de ellos para ampliar la seguridad.

11. **Ejemplo de asignación de un Security Group a una instancia EC2 existente**:

    ```sh
    aws ec2 modify-instance-attribute --instance-id i-1234567890abcdef0 --groups sg-0123456789abcdef0 sg-abcdef0123456789
    ```

12. Los Security Groups pueden ser **compartidos entre diferentes instancias**, lo que facilita la administración y evita la necesidad de configurar reglas repetitivas en cada servidor.

13. La correcta configuración de Security Groups es esencial para la seguridad en AWS. Un mal ajuste puede bloquear el acceso legítimo o exponer la instancia a ataques si se permite tráfico desde cualquier origen sin restricciones.

### 40. SSH Overview

1. **SSH (Secure Shell)** es una herramienta de línea de comandos utilizada para conectarse de forma segura a servidores Linux en la nube. Permite ejecutar comandos y administrar servidores de manera remota.

2. El método para utilizar SSH varía según el sistema operativo:
   - **Mac y Linux:** SSH está integrado en la terminal.
   - **Windows 10 y versiones posteriores:** Se puede usar la terminal de Windows con SSH nativo.
   - **Windows 7 y 8:** Se requiere **PuTTY**, un cliente SSH externo.

3. **Ejemplo de conexión SSH desde Mac o Linux:**
   ```sh
   ssh -i "mi-clave.pem" ec2-user@34.123.45.67
   ```
   Donde `"mi-clave.pem"` es el archivo de clave privada y `34.123.45.67` es la dirección IP pública de la instancia EC2.

4. **PuTTY** es la alternativa para Windows si SSH no está disponible en la terminal. Requiere convertir la clave privada `.pem` en `.ppk` usando PuTTYgen.

5. **EC2 Instance Connect** es una alternativa basada en navegador que permite conectarse a instancias EC2 sin necesidad de una terminal o cliente SSH. Funciona exclusivamente con **Amazon Linux 2**.

6. **Ejemplo de conexión con EC2 Instance Connect:**
   - Ir a la consola de AWS.
   - Seleccionar la instancia EC2.
   - Hacer clic en "Connect" y elegir "EC2 Instance Connect".
   - Presionar "Connect" para abrir una terminal en el navegador.

7. SSH suele ser una fuente de problemas si no está configurado correctamente. Los errores más comunes incluyen:
   - **Reglas de Security Groups mal configuradas** (puerto 22 bloqueado).
   - **Uso incorrecto del usuario predeterminado** (por ejemplo, `ubuntu` en lugar de `ec2-user`).
   - **Permisos incorrectos en la clave privada**, lo que impide su uso.

8. **Ejemplo de corrección de permisos en Mac/Linux:**
   ```sh
   chmod 400 mi-clave.pem
   ```
   Esto evita que la clave privada sea accesible por otros usuarios, un requisito de SSH.

9. Si ninguna opción de conexión funciona, se recomienda probar **EC2 Instance Connect**, ya que puede resolver problemas sin requerir configuraciones adicionales en el cliente.

10. Aunque SSH es una herramienta poderosa, no es obligatoria en este curso, y se pueden explorar otras formas de administrar instancias en AWS sin necesidad de usarlo constantemente.

### 46. EC2 Instance Roles Demo

1. Los **roles de IAM** permiten otorgar permisos a una instancia EC2 sin necesidad de configurar credenciales manualmente. Esto evita exponer claves de acceso en la instancia, lo cual es una mala práctica de seguridad.

2. Para acceder a una instancia EC2, se puede usar **SSH**, **PuTTY** (en Windows) o **EC2 Instance Connect**, que permite abrir una terminal en el navegador sin necesidad de configuración adicional.

3. **Ejemplo de conexión usando EC2 Instance Connect**:
   - Ir a la consola de AWS.
   - Seleccionar la instancia EC2.
   - Hacer clic en "Connect" y luego en "EC2 Instance Connect".
   - Presionar "Connect" para abrir la terminal.

4. Para verificar que la CLI de AWS está instalada en la instancia, se puede ejecutar el siguiente comando:
   ```sh
   aws --version
   ```
   Esto debería devolver la versión instalada de la CLI de AWS.

5. Intentar listar los usuarios de IAM sin un rol de IAM configurado dará un error de permisos:
   ```sh
   aws iam list-users
   ```
   La respuesta será **"Unable to locate credentials"**, indicando que no hay credenciales configuradas.

6. **No se deben ingresar credenciales de IAM manualmente** en una instancia EC2 usando `aws configure`, ya que esto expone información sensible que cualquier usuario con acceso a la instancia podría recuperar.

7. Para proporcionar permisos a una instancia EC2, se debe crear un **rol de IAM** con las políticas necesarias y asignarlo a la instancia desde la consola de AWS.

8. **Ejemplo de asignación de un rol IAM a una instancia EC2** desde AWS CLI:
   ```sh
   aws ec2 associate-iam-instance-profile --instance-id i-1234567890abcdef0 --iam-instance-profile Name=DemoRoleForEC2
   ```

9. Una vez asignado el rol, ejecutar nuevamente el comando `aws iam list-users` ahora devolverá la lista de usuarios de IAM, ya que la instancia tiene permisos adecuados.

10. Si se elimina el rol de IAM de la instancia y se intenta ejecutar el comando nuevamente, se recibirá un mensaje de **"Access Denied"**, demostrando que los permisos están vinculados al rol.

11. Si se agregan nuevas políticas al rol de IAM, puede tomar unos minutos para que los cambios se propaguen en AWS antes de que tengan efecto en la instancia.

12. Usar **roles de IAM** en lugar de credenciales manuales es una práctica fundamental para mejorar la seguridad en AWS, permitiendo que las instancias accedan a servicios sin comprometer información sensible.

### 47. EC2 Instance Purchasing Options

1. **Las instancias EC2 On-Demand** permiten ejecutar servidores sin compromisos a largo plazo, pagando solo por el uso. Son ideales para cargas de trabajo de corta duración y entornos de prueba.

2. **Ejemplo de lanzamiento de una instancia On-Demand:**
   ```sh
   aws ec2 run-instances --image-id ami-12345678 --count 1 --instance-type t2.micro --key-name my-key
   ```
   Esto lanza una instancia sin compromiso de tiempo y se factura por segundo.

3. **Las instancias reservadas** ofrecen descuentos de hasta **72%** a cambio de un compromiso de **1 o 3 años**. Son ideales para aplicaciones con cargas de trabajo predecibles, como bases de datos.

4. **Ejemplo de compra de una instancia reservada:**
   ```sh
   aws ec2 purchase-reserved-instances-offering --reserved-instances-offering-id ID --instance-count 1
   ```
   Esto permite reservar una instancia EC2 con una tarifa reducida.

5. **Las instancias reservadas convertibles** permiten cambiar el tipo de instancia dentro de la misma familia, aunque ofrecen un descuento menor (hasta **66%** en lugar de **72%**).

6. **Los Savings Plans** funcionan de manera similar a las instancias reservadas, pero en lugar de comprometerse con un tipo de instancia, se compromete un **gasto fijo en dólares por hora** durante **1 o 3 años**.

7. **Ejemplo de activación de un Savings Plan de $10/hora por 1 año:**
   ```sh
   aws savingsplans create-savings-plan --commitment 10 --savings-plan-type COMPUTE_SP
   ```
   Esto garantiza tarifas con descuento sin depender de una instancia específica.

8. **Las Spot Instances** ofrecen hasta **90% de descuento** en comparación con On-Demand, pero pueden ser terminadas en cualquier momento si el precio supera el límite máximo definido por el usuario.

9. **Ejemplo de solicitud de una Spot Instance con un precio máximo de $0.02/hora:**
   ```sh
   aws ec2 request-spot-instances --spot-price "0.02" --instance-count 1 --type "one-time" --launch-specification file://config.json
   ```
   Estas instancias son ideales para tareas no críticas, como procesamiento por lotes o análisis de datos.

10. **Las instancias dedicadas** garantizan que el hardware físico no sea compartido con otras cuentas de AWS, lo que mejora la seguridad y el rendimiento para ciertas aplicaciones reguladas.

11. **Ejemplo de lanzamiento de una instancia dedicada:**
    ```sh
    aws ec2 run-instances --instance-type m5.large --placement Tenancy=dedicated
    ```
    Esto asegura que la instancia se ejecute en hardware exclusivo.

12. **Los Dedicated Hosts** permiten reservar un servidor físico completo, proporcionando control total sobre la colocación de instancias y optimización de licencias de software.

13. **Las Capacity Reservations** garantizan capacidad en una **Zona de Disponibilidad (AZ)** sin descuentos, asegurando que los recursos estén disponibles cuando se necesiten.

14. **Ejemplo de reserva de capacidad en una AZ específica:**
    ```sh
    aws ec2 create-capacity-reservation --instance-type m5.large --instance-platform Linux/UNIX --availability-zone us-east-1a --instance-count 1
    ```
    Esto bloquea recursos para evitar problemas de disponibilidad.

15. **Comparación de opciones según el caso de uso:**
    - **On-Demand:** Aplicaciones impredecibles, entornos de prueba.
    - **Reserved Instances:** Aplicaciones de uso constante.
    - **Savings Plans:** Compromiso de gasto a largo plazo.
    - **Spot Instances:** Procesos flexibles sin dependencias críticas.
    - **Dedicated Hosts:** Cumplimiento normativo y optimización de licencias.
    - **Capacity Reservations:** Asegurar disponibilidad en una AZ específica.

16. **El modelo de precios en AWS varía según el compromiso y flexibilidad**. A mayor compromiso, mayor descuento.

17. **Resumen con analogía de resort:**
    - **On-Demand:** Pagas la tarifa completa al llegar sin reserva.
    - **Reserved Instances:** Reservas con anticipación y obtienes descuento.
    - **Savings Plan:** Te comprometes a gastar una cantidad fija cada mes.
    - **Spot Instances:** Descuentos de última hora, pero pueden revocar tu habitación.
    - **Dedicated Host:** Alquilas todo el hotel.
    - **Capacity Reservation:** Reservas una habitación sin descuento, pero garantizas disponibilidad.

18. **Para elegir la mejor opción, se debe analizar la carga de trabajo y la disponibilidad de presupuesto**, combinando diferentes tipos de instancias según el caso.

### 48. IP Address Charges in AWS

1. **Desde el 1 de febrero de 2024, AWS cobra por todas las direcciones IPv4 públicas** asignadas en una cuenta, incluso si no están en uso. El costo es de **$0.005 por hora**, lo que equivale a aproximadamente **$3.60 al mes** por cada dirección IPv4 pública.

2. **Las instancias EC2 tienen un beneficio en el Free Tier** que otorga **750 horas de uso de IPv4 públicas al mes** durante los primeros 12 meses de la cuenta. Sin embargo, si se excede este límite, se comienzan a generar costos.

3. **Ejemplo de cálculo del uso de IPv4 en EC2:**
   - Una sola instancia EC2 con IPv4 = gratis (750 horas mensuales).
   - Dos instancias funcionando simultáneamente = **375 horas gratuitas por cada una**.
   - Cuatro instancias simultáneas = **cobro por las horas que excedan las 750 horas mensuales**.

4. **Otros servicios de AWS como RDS, Load Balancers y NAT Gateway no tienen Free Tier**, por lo que cualquier dirección IPv4 pública asociada genera costos desde el inicio.

5. **Ejemplo de asignación de una dirección IPv4 a una instancia EC2:**
   ```sh
   aws ec2 allocate-address
   ```
   Esto asigna una dirección IP elástica (EIP), que se cobra si no está asociada a una instancia en ejecución.

6. **Los Load Balancers pueden generar múltiples direcciones IPv4**, una por zona de disponibilidad (AZ). Un balanceador en tres AZ generará **tres direcciones IPv4 públicas** y costos adicionales.

7. **Las bases de datos en Amazon RDS que requieren acceso público también generan costos por IPv4**, ya que no forman parte del Free Tier de EC2.

8. **Para evitar cargos innecesarios, es recomendable liberar las direcciones IPv4 no utilizadas:**
   ```sh
   aws ec2 release-address --allocation-id eipalloc-12345678
   ```
   Esto libera la dirección IPv4 y detiene los cargos asociados.

9. **IPv6 no genera costos adicionales**, ya que AWS está promoviendo su adopción. Sin embargo, muchos proveedores de internet aún no soportan IPv6 completamente, lo que puede dificultar su uso en algunas regiones.

10. **Para verificar si un proveedor de internet admite IPv6**, se puede usar herramientas como:
    - [Test de conectividad IPv6](http://test-ipv6.com)

11. **Si se decide utilizar IPv6 en lugar de IPv4, es necesario configurar reglas de seguridad manualmente**, asegurando que los Security Groups permitan el tráfico IPv6.

12. **Para monitorear y entender los cargos generados por IPv4, se debe revisar el apartado de "Billing & Cost Management" en AWS**, donde se pueden ver los costos desglosados por servicio.

13. **Para identificar direcciones IPv4 en uso dentro de una cuenta de AWS, se puede usar el servicio AWS IP Address Manager (IPAM):**
    ```sh
    aws ec2 describe-addresses
    ```
    Esto lista todas las direcciones IP asignadas a la cuenta.

14. **El servicio AWS IPAM permite obtener insights sobre las direcciones IP públicas usadas en la cuenta**, lo que ayuda a evitar costos innecesarios.

15. **Para activar IPAM desde la consola de AWS:**
    - Ir a **Amazon VPC IP Address Manager (IPAM)**.
    - Seleccionar **Public IP Insights**.
    - Crear una nueva IPAM con la opción gratuita del Free Tier.
    - Agregar todas las regiones necesarias y generar el informe.

16. **Para evitar cargos excesivos en AWS, es importante no dejar instancias, balanceadores de carga o bases de datos corriendo innecesariamente**, ya que las direcciones IPv4 públicas asociadas a estos recursos generan costos aunque no se usen.

### 49. Shared Responsibility Model for EC2

1. **AWS es responsable de la infraestructura física**, lo que incluye los centros de datos, el hardware y el aislamiento de los servidores. Los usuarios no tienen control sobre estos aspectos, pero pueden confiar en que AWS gestiona la seguridad y el mantenimiento del hardware.

2. **AWS garantiza la seguridad del hardware subyacente**, reemplazando componentes defectuosos y asegurando que las instancias EC2 operen en un entorno estable y conforme a regulaciones de seguridad.

3. **El usuario es responsable de la seguridad dentro de la nube**, lo que significa que debe configurar correctamente los grupos de seguridad, evitando accesos no autorizados a las instancias EC2.

4. **Ejemplo de configuración de una regla en un Security Group para permitir solo conexiones SSH desde una IP específica:**
   ```sh
   aws ec2 authorize-security-group-ingress --group-id sg-12345678 --protocol tcp --port 22 --cidr 203.0.113.0/32
   ```
   Esto evita que cualquier IP desconocida acceda al servidor mediante SSH.

5. **El usuario administra el sistema operativo de la instancia EC2**, ya sea Linux o Windows. Esto incluye la instalación de parches y actualizaciones, ya que AWS no se encarga del mantenimiento del software interno de la instancia.

6. **Ejemplo de actualización de paquetes en una instancia EC2 con Linux:**
   ```sh
   sudo yum update -y   # Para Amazon Linux
   sudo apt update && sudo apt upgrade -y   # Para Ubuntu
   ```
   Mantener el sistema operativo actualizado es clave para evitar vulnerabilidades de seguridad.

7. **El usuario es responsable de la configuración y mantenimiento del software dentro de la instancia**, incluyendo servidores web, bases de datos, aplicaciones y cualquier otro software instalado.

8. **Las credenciales y accesos deben ser gestionados por el usuario**, asegurándose de utilizar IAM Roles en lugar de credenciales almacenadas en texto plano dentro de las instancias.

9. **Ejemplo de asignación de un IAM Role a una instancia EC2:**
   ```sh
   aws ec2 associate-iam-instance-profile --instance-id i-1234567890abcdef --iam-instance-profile Name=EC2Role
   ```
   Esto permite que la instancia asuma permisos sin necesidad de almacenar claves de acceso manualmente.

10. **El usuario debe proteger los datos almacenados en la instancia**, utilizando cifrado en volúmenes EBS y respaldos periódicos para evitar pérdida de información.

11. **Ejemplo de habilitación del cifrado en un volumen EBS:**
   ```sh
   aws ec2 modify-volume --volume-id vol-12345678 --encrypted
   ```
   Esto asegura que los datos en el disco no puedan ser leídos sin autorización, mejorando la seguridad de la instancia EC2.

Este modelo de responsabilidad compartida obliga al usuario a gestionar la seguridad dentro de la instancia, mientras que AWS garantiza la protección de la infraestructura en la nube.

### 50. EC2 Summary

1. **Una instancia EC2 está compuesta por una AMI**, que define el sistema operativo y la configuración base de la máquina virtual. Puede ser Amazon Linux, Ubuntu, Windows, entre otros.

2. **El tamaño de la instancia EC2 determina el poder de cómputo**, definiendo cuántos vCPU y cuánta memoria RAM tendrá la instancia. Ejemplo de creación de una instancia con `t2.micro`:
   ```sh
   aws ec2 run-instances --image-id ami-12345678 --instance-type t2.micro --count 1
   ```

3. **El almacenamiento de la instancia se define al momento de su creación**, pudiendo ser volúmenes EBS persistentes o almacenamiento efímero con Instance Store.

4. **Los grupos de seguridad funcionan como firewalls**, controlando el tráfico de entrada y salida a la instancia EC2 mediante reglas de acceso específicas.

5. **Ejemplo de regla en Security Group para permitir tráfico HTTP (puerto 80):**
   ```sh
   aws ec2 authorize-security-group-ingress --group-id sg-12345678 --protocol tcp --port 80 --cidr 0.0.0.0/0
   ```

6. **El script EC2 User Data permite automatizar tareas al iniciar la instancia**, ejecutando comandos al arranque para instalar software o configurar la máquina. Ejemplo de instalación de un servidor web Apache en Linux:
   ```sh
   #!/bin/bash
   yum update -y
   yum install -y httpd
   systemctl start httpd
   systemctl enable httpd
   ```

7. **SSH es el método principal de acceso a las instancias EC2 en Linux**, utilizando el puerto 22 para conectarse de forma segura desde un terminal.

8. **Ejemplo de conexión SSH a una instancia EC2 con clave privada:**
   ```sh
   ssh -i "mi-clave.pem" ec2-user@54.123.45.67
   ```

9. **Los roles de IAM pueden asignarse a instancias EC2**, permitiendo ejecutar comandos de AWS sin necesidad de credenciales dentro de la instancia.

10. **Opciones de compra en EC2 para optimizar costos:**
    - **On-demand:** Pago por uso, ideal para cargas de trabajo variables.
    - **Spot Instances:** Hasta 90% de descuento, pero pueden ser terminadas en cualquier momento.
    - **Reserved Instances:** Descuentos de hasta 72% para compromisos de 1 a 3 años.
    - **Dedicated Host:** Se asigna un servidor físico completo a un solo cliente.

11. **Ejemplo de creación de una Reserved Instance de 1 año con pago parcial:**
    ```sh
    aws ec2 purchase-reserved-instances-offering --instance-type m5.large --instance-count 1 --offering-class standard --offering-type Partial Upfront
    ```

12. **EC2 es uno de los servicios fundamentales en AWS**, proporcionando máquinas virtuales en la nube con opciones flexibles de configuración, seguridad y costos.