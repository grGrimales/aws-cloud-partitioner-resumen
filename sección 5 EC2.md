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