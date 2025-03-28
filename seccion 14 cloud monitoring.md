# 159. CloudWatch Metrics & CloudWatch Alarms Overview

1. **CloudWatch Metrics** permite monitorear variables de desempeño de los servicios en AWS como si fueran sensores que registran datos con el paso del tiempo, incluyendo indicadores como `CPUUtilization`, `NetworkIn` y otros.

2. **Cada métrica tiene timestamps**, lo que permite ver cómo evoluciona a lo largo del tiempo y facilita la detección de patrones o anomalías.

3. **Los dashboards de CloudWatch** permiten visualizar múltiples métricas al mismo tiempo, organizadas en paneles para obtener una visión completa del estado de los servicios.

4. **La métrica de facturación (`Billing metric`)** permite conocer cuánto se ha gastado en AWS en un mes. Esta métrica está disponible únicamente en la región `us-east-1`.

5. **Las métricas se actualizan cada cinco minutos por defecto**, pero se puede activar el `Detailed Monitoring` para recibir datos cada minuto, lo cual tiene un costo adicional.

6. **En EC2**, las métricas clave incluyen `CPUUtilization`, `StatusCheckFailed` y `NetworkIn/Out`, pero no es posible monitorear directamente el uso de memoria RAM sin herramientas adicionales.

7. **La métrica `CPUUtilization`** es esencial para detectar si una instancia está sobrecargada y necesita escalar horizontal o verticalmente.

8. **Las métricas de discos EBS**, como `VolumeReadOps` y `VolumeWriteOps`, muestran cuántas operaciones de lectura y escritura se están realizando en el disco.

9. **Para los buckets S3**, se puede monitorear el tamaño del bucket en bytes, el número de objetos, y el número de solicitudes realizadas, lo que ayuda a controlar almacenamiento y tráfico.

10. **Se pueden crear métricas personalizadas**, permitiendo a los desarrolladores enviar sus propios datos para ser monitoreados con la misma infraestructura de CloudWatch.

11. **CloudWatch Alarms** permite definir umbrales para una métrica y ejecutar acciones cuando ese umbral se supera o se incumple.

12. **Las alarmas pueden desencadenar acciones** como aumentar o reducir el número de instancias en un Auto Scaling Group, detener o reiniciar instancias EC2, o enviar notificaciones por medio de SNS.

13. **Ejemplo de alarma**: si `CPUUtilization` supera el 90%, se puede enviar una notificación por correo electrónico a través de SNS para alertar al equipo.

14. **Las alarmas permiten configurar diferentes criterios de evaluación**, como `max`, `min`, `average`, y diferentes periodos de muestreo como 5 minutos, 10 minutos o una hora.

15. **Es posible crear una alarma de facturación**, configurada para activar una alerta si el gasto estimado supera un límite definido, como $10 o $20.

16. **Los estados de una alarma** incluyen `OK` (todo está bien), `INSUFFICIENT_DATA` (no hay suficientes datos para evaluar), y `ALARM` (la condición de alerta se ha cumplido).

17. **Ejemplo de creación de alarma en AWS CLI**:
   ```bash
   aws cloudwatch put-metric-alarm \
     --alarm-name "HighCPUAlarm" \
     --metric-name CPUUtilization \
     --namespace AWS/EC2 \
     --statistic Average \
     --period 300 \
     --threshold 90 \
     --comparison-operator GreaterThanThreshold \
     --dimensions Name=InstanceId,Value=i-1234567890abcdef0 \
     --evaluation-periods 2 \
     --alarm-actions arn:aws:sns:us-east-1:123456789012:MySNSTopic \
     --unit Percent
   ```

# 160. CloudWatch Metrics & CloudWatch Alarms Hands On

1. **La consola de CloudWatch** permite visualizar métricas de todos los servicios de AWS, como SQS, EC2 y muchos más, facilitando el monitoreo desde una única interfaz centralizada.

2. **En las métricas de SQS**, se puede observar información como el número de mensajes recibidos, eliminados, enviados y vacíos, lo que ayuda a evaluar el tráfico y eficiencia de las colas.

3. **Las métricas de EC2 por instancia** incluyen datos importantes como `CPUUtilization`, que permiten monitorear el desempeño y detectar posibles cuellos de botella.

4. **Para ver datos significativos en una métrica**, es importante que la instancia haya estado corriendo durante un tiempo razonable, ya que métricas recién creadas pueden no tener datos suficientes.

5. **La creación de una alarma en CloudWatch** comienza seleccionando una métrica específica, como el uso de CPU de una instancia EC2, para luego definir condiciones de activación.

6. **El umbral de activación de una alarma** puede basarse en el promedio, suma, máximo u otros estadísticos de la métrica monitoreada, permitiendo configurar distintos tipos de análisis.

7. **Ejemplo de configuración de una alarma simple**: activar una alerta si el `CPUUtilization` supera el 80% durante un periodo de 5 minutos.

8. **Las notificaciones de alarmas** pueden enviarse a través de un SNS Topic, que a su vez reenvía el mensaje a direcciones de correo electrónico previamente registradas.

9. **Ejemplo práctico**: se crea el topic `Default_CloudWatch_Alarms_Topic` para enviar un correo a `stephan@example.com` cada vez que la alarma cambia de estado.

10. **Además de notificaciones, las alarmas pueden realizar acciones automáticas**, como escalar grupos de Auto Scaling, reiniciar instancias EC2, o ejecutar tareas con Systems Manager.

11. **Desde la consola de EC2**, es posible crear alarmas directamente para cada instancia, accediendo a la pestaña Monitoring y utilizando el botón de creación de alarmas.

12. **Una alarma puede configurarse para recuperar una instancia EC2**, si ocurre un fallo en los chequeos de estado, utilizando la opción `Status check failed: system`.

13. **Cada acción de alarma debe estar ligada a una métrica específica**, por lo tanto, seleccionar correctamente el tipo de verificación es fundamental para evitar errores en la creación.

14. **Ejemplo avanzado**: reiniciar una instancia EC2 si el `CPUUtilization` supera el 95% durante tres períodos consecutivos de cinco minutos, útil para detectar ciclos infinitos de CPU.

15. **Una misma instancia EC2 puede tener múltiples alarmas asociadas**, lo que permite manejar distintos escenarios de monitoreo y respuesta automática en paralelo.

16. **La métrica de facturación (`EstimatedCharges`)** solo está disponible en la región `us-east-1`, y permite configurar alarmas en función de los costos acumulados en la cuenta.

17. **Para habilitar la opción de facturación en CloudWatch**, se debe cambiar a la región `us-east-1` desde el selector de región en la consola de AWS.

18. **Ejemplo de alarma de facturación**: activar una notificación si los gastos estimados superan los 8 USD, enviando un correo mediante un SNS topic exclusivo para esta región.

19. **Cada recurso de AWS está vinculado a una región específica**, por lo que al crear un SNS topic en `us-east-1`, este no estará disponible en otras regiones como Irlanda o Frankfurt.

20. **Después del ejercicio práctico**, es importante limpiar los recursos creados para evitar costos innecesarios, incluyendo la eliminación de alarmas y la terminación de instancias EC2.

21. **El uso conjunto de métricas y alarmas** en CloudWatch proporciona un sistema automatizado de vigilancia y respuesta ante eventos críticos en la infraestructura de AWS.

# 161. CloudWatch Logs Overview

1. **CloudWatch Logs permite recopilar archivos de log** generados por aplicaciones que se ejecutan en servidores, con el fin de facilitar la supervisión y el diagnóstico de problemas en tiempo real.

2. **Un archivo de log es un registro textual** que detalla las operaciones realizadas por una aplicación, como solicitudes de usuarios, procesos de limpieza, errores y otras acciones relevantes.

3. **Los logs pueden provenir de múltiples fuentes** dentro de AWS como Elastic Beanstalk, ECS, Lambda, CloudTrail, Route53, o desde servidores EC2 y on-premise mediante agentes de log.

4. **El agente de CloudWatch Logs** se instala en una instancia EC2 o en un servidor local, y es responsable de enviar los logs generados localmente hacia el servicio de CloudWatch Logs.

5. **Sin el agente de logs, las instancias EC2 no envían datos automáticamente** a CloudWatch Logs. Es necesaria una instalación y configuración explícita para iniciar la transferencia de datos.

6. **Los logs en CloudWatch son útiles para la resolución de problemas**, ya que permiten ver con precisión lo que ocurrió en una aplicación durante un periodo determinado.

7. **CloudWatch Logs permite configurar la retención de datos**, lo cual brinda control sobre cuánto tiempo conservar los registros: una semana, 30 días, un año, o indefinidamente.

8. **Ejemplo de configuración de retención**:
   ```bash
   aws logs put-retention-policy \
     --log-group-name "/aws/ec2/my-application" \
     --retention-in-days 30
   ```

9. **El agente de CloudWatch Logs puede operar en modo híbrido**, recolectando información tanto desde instancias EC2 como desde servidores on-premise, lo cual ofrece una solución centralizada de monitoreo.

10. **La funcionalidad de CloudWatch Logs no está limitada a AWS**, permitiendo que servidores fuera de la nube también puedan integrarse al sistema de monitoreo de logs.

11. **Para que el agente funcione correctamente**, la instancia EC2 debe tener un rol de instancia con permisos adecuados de IAM que le permitan escribir en el servicio de CloudWatch Logs.

12. **Ejemplo de política IAM básica para envío de logs**:
   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": [
           "logs:CreateLogGroup",
           "logs:CreateLogStream",
           "logs:PutLogEvents"
         ],
         "Resource": "*"
       }
     ]
   }
   ```

13. **Los logs enviados pueden visualizarse en tiempo real**, lo cual permite reacciones inmediatas ante eventos inesperados o fallos en el sistema.

14. **La instalación del agente puede automatizarse** durante el proceso de creación de la instancia EC2, asegurando que todas las nuevas máquinas comiencen a enviar sus logs desde el primer minuto.

15. **CloudWatch Logs se convierte en una fuente centralizada** para toda la información operacional de la infraestructura, facilitando auditorías, seguimiento de errores y análisis de seguridad.