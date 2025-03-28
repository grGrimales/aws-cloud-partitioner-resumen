# 151. Cloud Integrations Overview

1. **Cuando varias aplicaciones interactúan entre sí, se requiere una integración eficiente**, y AWS ofrece múltiples patrones para lograrlo de forma escalable y resiliente.

2. **El patrón de comunicación síncrona implica que una aplicación llama directamente a otra**, como un servicio de compras que invoca al servicio de envíos tras completar una transacción.

3. **En este patrón síncrono, ambas aplicaciones están estrechamente acopladas**, lo que puede generar problemas si una de ellas se vuelve lenta o no responde.

4. **Una alternativa más robusta es el patrón de comunicación asíncrona o basada en eventos**, en el que las aplicaciones se comunican mediante un intermediario, como una cola.

5. **En un patrón asíncrono, el servicio de compras envía un mensaje a una cola**, y el servicio de envíos consume ese mensaje desde la cola cuando esté listo para procesarlo.

6. **Este enfoque se conoce como acoplamiento débil (loose coupling)**, lo que reduce la dependencia directa entre los componentes y permite mayor flexibilidad.

7. **El uso de colas mejora la tolerancia a picos de tráfico**, ya que los mensajes se almacenan temporalmente hasta que el sistema receptor pueda procesarlos.

8. **Un caso práctico es cuando normalmente se codifican 10 videos, pero de repente llegan 1000**, un sistema acoplado podría colapsar, pero uno con colas puede gestionar la carga progresivamente.

9. **Amazon SQS (Simple Queue Service) es la solución de AWS para implementar colas**, ideal para desacoplar procesos y mejorar la escalabilidad.

10. **Amazon SNS (Simple Notification Service) implementa un modelo de publicación-suscripción (pub/sub)**, en el que múltiples consumidores pueden reaccionar a un mismo evento.

11. **SNS es útil cuando se quiere notificar a múltiples servicios simultáneamente**, como enviar un email, iniciar un proceso backend y registrar un evento a partir de una sola acción.

12. **Amazon Kinesis es otra herramienta útil para integración, orientada al procesamiento en tiempo real**, ideal para grandes flujos de datos como logs, métricas o clics de usuarios.

13. **La integración basada en colas permite que los servicios escalen de forma independiente**, ya que cada componente puede ajustarse a su ritmo sin bloquear a los demás.

14. **Este diseño facilita la resiliencia del sistema**, ya que si una parte del sistema falla, los mensajes en la cola no se pierden y pueden ser reintentados luego.

15. **Los patrones asíncronos con SQS, SNS o Kinesis forman la base de arquitecturas desacopladas modernas en la nube**, ayudando a construir sistemas más mantenibles y robustos.

# 152. SQS Overview

1. **Amazon SQS (Simple Queue Service) es un servicio completamente gestionado que permite desacoplar aplicaciones**, ofreciendo un sistema de mensajería asincrónica entre productores y consumidores.

2. **Una cola (queue) en SQS funciona como un buffer temporal donde los productores envían mensajes**, y los consumidores los leen cuando estén listos, evitando la necesidad de comunicación directa entre ellos.

3. **El sistema admite múltiples productores y múltiples consumidores**, lo que permite distribuir la carga y escalar horizontalmente el procesamiento de mensajes.

4. **Los consumidores extraen (polling) mensajes de la cola y, una vez procesados, deben eliminarlos manualmente**, asegurando así que no se reprocesen accidentalmente.

5. **SQS es uno de los servicios más antiguos de AWS y ha demostrado ser extremadamente escalable y confiable**, con soporte para pasar de 1 mensaje por segundo a decenas de miles por segundo sin esfuerzo adicional.

6. **La retención de mensajes en la cola por defecto es de 4 días**, aunque se puede configurar hasta un máximo de 14 días, lo que permite cierto margen de tolerancia para procesamiento tardío.

7. **No existe límite en la cantidad de mensajes que pueden almacenarse en una cola**, lo que lo hace ideal para cargas variables y procesamiento asincrónico.

8. **El servicio es serverless**, lo cual significa que no requiere aprovisionamiento de infraestructura por parte del usuario y se integra fácilmente con otros servicios de AWS.

9. **La latencia para publicar y recibir mensajes es extremadamente baja**, generalmente menor a 10 milisegundos, permitiendo interacciones casi en tiempo real.

10. **Un patrón arquitectónico clásico con SQS es desacoplar el frontend del backend**, por ejemplo: servidores web insertan tareas en la cola y un grupo de instancias EC2 las consume para procesarlas en segundo plano.

11. **Este modelo permite que los servidores web y los procesadores de tareas escalen de forma independiente**, lo que mejora el rendimiento general y la eficiencia de costos.

12. **El autoescalado puede configurarse con base en la cantidad de mensajes pendientes en la cola**, permitiendo que se ajusten dinámicamente los recursos según la carga del sistema.

13. **SQS permite implementar un flujo de trabajo basado en eventos de manera robusta**, en donde cada capa del sistema puede operar a su propio ritmo sin bloquear a las demás.

14. **La variante FIFO (First-In-First-Out) garantiza que los mensajes se procesen en el orden en que fueron enviados**, algo esencial cuando el orden importa, como en transacciones financieras.

15. **Una cola estándar de SQS no garantiza el orden de procesamiento ni la entrega única de mensajes**, mientras que una cola FIFO sí garantiza ambas, pero tiene un rendimiento más limitado.

16. **El uso de SQS permite construir aplicaciones resilientes y escalables**, que pueden tolerar fallos y adaptarse a distintos niveles de tráfico sin perder datos ni sobrecargar componentes.

17. **Para efectos del examen de certificación, siempre que veas la necesidad de desacoplar componentes o manejar picos de carga asincrónicamente, SQS será la opción recomendada por AWS.**


# 153. SQS Hands On

1. **Amazon SQS es un servicio de mensajería de alto rendimiento** que permite la comunicación entre sistemas desacoplados mediante el uso de colas para enviar y recibir mensajes.

2. **Existen dos tipos de colas en SQS: estándar y FIFO**, donde la cola estándar ofrece mayor rendimiento y la FIFO garantiza el orden de los mensajes; para esta práctica se utilizó una cola estándar.

3. **La creación de una cola estándar se puede hacer sin configurar parámetros avanzados**, simplemente asignando un nombre, como por ejemplo `demo-sqs`, y usando las opciones predeterminadas.

4. **Una vez creada la cola, se puede enviar un mensaje directamente desde la consola de AWS**, ingresando un contenido como `hello world` y haciendo clic en “Send message”.

5. **Los mensajes enviados quedan en estado “disponible” hasta que algún consumidor los lea**, momento en el cual pasan a estar “en vuelo” (in-flight) si no se eliminan de inmediato.

6. **La consola permite visualizar detalles del mensaje**, como su ID, checksum MD5 del cuerpo, y cualquier atributo adicional que se hubiera agregado.

7. **Se puede seguir enviando múltiples mensajes**, como por ejemplo `another hello`, y estos se irán acumulando en la cola en estado pendiente hasta que un consumidor los procese.

8. **El polling es la acción de consultar la cola para recibir los mensajes**, lo cual se puede hacer desde la misma consola con el botón “Poll for messages”.

9. **Al realizar un polling exitoso, los mensajes se muestran con sus respectivos cuerpos**, permitiendo leer el contenido como lo haría una aplicación cliente real.

10. **La lógica de procesamiento de mensajes debe ser desarrollada por el usuario**, por ejemplo mediante una aplicación que extraiga, procese y elimine los mensajes según su lógica de negocio.

11. **La eliminación de un mensaje tras su procesamiento es necesaria para que no vuelva a ser recibido**, ya que por defecto los mensajes no desaparecen de la cola tras ser leídos.

12. **SQS no garantiza que un mensaje sea entregado una sola vez en colas estándar**, por lo tanto, las aplicaciones deben ser idempotentes y manejar duplicados.

13. **Desde la consola se pueden observar estadísticas de la cola**, como cuántos mensajes están disponibles, cuántos están en vuelo y cuántos están diferidos.

14. **Al terminar la práctica, es recomendable eliminar la cola para evitar acumulación de recursos**, aunque mantenerla no genera costos si no está en uso.

15. **Para borrar una cola en la consola se requiere confirmar escribiendo la palabra “delete”**, como medida de seguridad para evitar eliminaciones accidentales.

16. **Este ejercicio demuestra el funcionamiento básico de SQS**, desde la creación de la cola, envío y recepción de mensajes, hasta su procesamiento y eliminación, todo desde la consola de AWS sin necesidad de código.

# 154. Kinesis Overview

1. **Amazon Kinesis Data Streams es un servicio diseñado para la ingesta, procesamiento y análisis de datos en tiempo real**, ideal para aplicaciones que requieren respuestas inmediatas a eventos entrantes.

2. **Cuando se menciona Kinesis en un examen de AWS**, debe asociarse directamente con escenarios de “real-time big data streaming”, es decir, transmisión de datos a gran escala en tiempo real.

3. **Kinesis permite recopilar datos desde múltiples fuentes de alta velocidad**, como clics en sitios web, sensores IoT, logs de servidores o métricas de aplicaciones.

4. **La arquitectura típica incluye Kinesis Data Streams como punto de entrada**, donde se almacenan los datos en tiempo real antes de ser procesados o redirigidos a otros servicios.

5. **Amazon Kinesis Data Firehose es una extensión del ecosistema de Kinesis**, que permite entregar automáticamente los datos recibidos a destinos como S3, Redshift, OpenSearch, entre otros.

6. **Kinesis Data Streams funciona con productores y consumidores**, donde los productores envían datos de manera continua al stream y los consumidores los procesan casi en tiempo real.

7. **La latencia en Kinesis puede ser tan baja como 70 milisegundos**, lo que permite reaccionar casi instantáneamente a eventos generados en aplicaciones o dispositivos.

8. **Cada stream de Kinesis se compone de shards**, que son unidades de capacidad para paralelizar la escritura y lectura de datos, lo cual permite escalar el sistema de manera horizontal.

9. **El almacenamiento temporal en Kinesis Data Streams permite retener datos por un mínimo de 24 horas y hasta 7 días**, según la configuración del stream.

10. **Amazon Kinesis se integra fácilmente con otras herramientas de análisis**, como AWS Lambda para procesamiento en tiempo real o Amazon Analytics para visualización de resultados.

11. **Un caso de uso común de Kinesis es la analítica de comportamiento de usuarios en aplicaciones web**, capturando clics, interacciones y eventos sin necesidad de almacenarlos primero en una base de datos.

12. **Amazon Kinesis Data Firehose actúa como un servicio administrado para cargar los datos directamente en servicios de almacenamiento o análisis**, eliminando la necesidad de escribir código personalizado para la transferencia.

13. **La configuración de Kinesis Firehose es simple y automatizada**, ya que se encarga de escalar, particionar, encriptar, comprimir y entregar los datos a los destinos configurados.

14. **Kinesis ayuda a desacoplar la generación de eventos del procesamiento**, permitiendo que los sistemas productores y consumidores escalen independientemente.

15. **El uso de Kinesis favorece arquitecturas orientadas a eventos**, donde las decisiones del negocio pueden ser tomadas en tiempo real con base en el flujo continuo de datos entrantes.

# 155. SNS Overview

1. **Amazon SNS (Simple Notification Service) es un servicio de mensajería tipo Pub/Sub (publicador/suscriptor)** que permite enviar un solo mensaje a múltiples destinatarios de manera simultánea y eficiente.

2. **El patrón Pub/Sub ayuda a desacoplar componentes de una aplicación**, permitiendo que un servicio publique mensajes en un tema (topic) sin necesidad de saber quién los va a recibir.

3. **En lugar de integrar directamente un servicio con múltiples destinos**, como correos electrónicos, colas SQS o funciones Lambda, SNS centraliza la publicación y distribuye automáticamente el mensaje.

4. **Un "topic" de SNS actúa como un canal de comunicación**, al cual se suscriben los receptores (subscribers) que desean recibir los mensajes emitidos por un publicador.

5. **Todos los suscriptores de un tema de SNS reciben cada mensaje enviado**, a diferencia de SQS, donde los consumidores comparten los mensajes y los eliminan después de procesarlos.

6. **SNS es ideal para casos donde una acción debe desencadenar múltiples respuestas paralelas**, como el envío de correos, activación de funciones Lambda y almacenamiento en Firehose de forma simultánea.

7. **El número de suscripciones por tema en SNS puede superar los 12 millones**, lo que lo convierte en una solución escalable para sistemas distribuidos de gran tamaño.

8. **Cada cuenta de AWS puede crear hasta 100,000 topics de SNS**, aunque este límite es suave y puede ampliarse si se solicita.

9. **SNS admite múltiples tipos de suscriptores como destinos**, incluyendo Amazon SQS, AWS Lambda, Amazon Kinesis Data Firehose, direcciones de correo electrónico, SMS, notificaciones móviles y endpoints HTTP/HTTPS.

10. **El envío de notificaciones por correo electrónico desde SNS es muy utilizado**, especialmente para alertas, confirmaciones de eventos o monitoreo.

11. **Las notificaciones por SMS permiten contactar directamente a usuarios en sus dispositivos móviles**, facilitando la comunicación en tiempo real sin necesidad de una aplicación.

12. **SNS también puede integrarse con aplicaciones móviles mediante notificaciones push**, compatibles con servicios como Apple Push Notification Service (APNS) o Firebase Cloud Messaging (FCM).

13. **Los endpoints HTTP/HTTPS permiten conectar SNS con APIs externas**, posibilitando integraciones con sistemas de terceros u otros microservicios.

14. **Cuando aparece el concepto de notificación, publicación o múltiples receptores en un examen**, la mejor opción a considerar es SNS por su naturaleza de distribución masiva.

15. **La simplicidad de SNS radica en su arquitectura desacoplada**, donde los emisores de mensajes no necesitan conocer a los destinatarios, fomentando sistemas más flexibles y mantenibles.

# 156. SNS Hands On

1. **Amazon SNS permite la creación de un "topic" para centralizar el envío de mensajes**, donde múltiples suscriptores pueden recibir los mismos mensajes desde una única fuente de publicación.

2. **El primer paso en la práctica con SNS es crear un topic**, que en este caso se llamó `demo-SNS`, y se dejó con la configuración predeterminada.

3. **Un topic en SNS puede tener múltiples tipos de suscripciones**, como HTTP, HTTPS, Email, Email-JSON, SQS y funciones Lambda, ofreciendo gran flexibilidad para la entrega de mensajes.

4. **En esta demostración se utilizó la opción de suscripción por correo electrónico**, debido a su simplicidad y facilidad para verificar la entrega del mensaje.

5. **Se usó un correo temporal proporcionado por Mailinator**, lo cual es útil para pruebas rápidas sin comprometer correos reales.

6. **Una vez creada la suscripción por email, esta queda en estado de "pending confirmation"**, hasta que el usuario confirme el enlace recibido por correo.

7. **Al acceder al buzón de Mailinator**, se visualizó un correo de AWS con un enlace de confirmación para validar la suscripción al topic SNS.

8. **Después de hacer clic en el enlace de confirmación**, el estado de la suscripción pasó a "confirmed", permitiendo recibir mensajes desde ese topic.

9. **La consola de SNS permite enviar mensajes directamente desde el panel de control**, lo que es útil para pruebas manuales o mensajes urgentes.

10. **El mensaje de prueba enviado incluyó un asunto y un cuerpo simple**: asunto `demo subject line` y cuerpo `hello world`.

11. **Tras la publicación del mensaje**, todos los suscriptores activos del topic, sin importar el protocolo, recibirán el mensaje enviado.

12. **Al revisar nuevamente el buzón en Mailinator**, se confirmó que el mensaje fue recibido exitosamente con el asunto y contenido definidos.

13. **Este flujo demuestra el poder de SNS para distribuir un mensaje simultáneamente a múltiples consumidores**, eliminando la necesidad de múltiples integraciones directas.

14. **La interfaz gráfica facilita la gestión del ciclo completo de vida de un topic**, desde la creación, suscripción, publicación y eliminación.

15. **Eliminar el topic después de finalizar la prueba es una buena práctica**, aunque SNS no tiene costos por mantener un topic inactivo, ayuda a mantener el entorno limpio.

# 157. Amazon MQ Overview

1. **Amazon MQ es un servicio de broker de mensajes completamente administrado**, diseñado para trabajar con tecnologías tradicionales como RabbitMQ y ActiveMQ.

2. **A diferencia de SQS y SNS, que son servicios nativos de AWS**, Amazon MQ utiliza protocolos abiertos estándar ampliamente adoptados en sistemas on-premises.

3. **Amazon MQ soporta protocolos como MQTT, AMQP, STOMP, OpenWire y WebSocket Secure (WSS)**, facilitando la migración de aplicaciones legadas sin necesidad de reescritura de código.

4. **El uso principal de Amazon MQ es en contextos de migración hacia la nube**, cuando una empresa ya utiliza estos protocolos abiertos y busca mantener compatibilidad.

5. **Amazon MQ permite a las aplicaciones comunicarse usando colas (queue) y temas (topics)**, combinando funcionalidades similares a las de SQS y SNS en un solo servicio.

6. **SQS y SNS son altamente escalables, serverless y están profundamente integrados en el ecosistema de AWS**, lo que los hace más eficientes para nuevas arquitecturas cloud-native.

7. **Amazon MQ corre sobre servidores físicos o virtuales administrados por AWS**, por lo tanto, no es serverless y puede presentar limitaciones de escalabilidad y disponibilidad en comparación.

8. **Para mejorar la disponibilidad, Amazon MQ puede configurarse en múltiples zonas de disponibilidad (Multi-AZ)**, permitiendo una conmutación por error automática en caso de fallo.

9. **RabbitMQ y ActiveMQ son tecnologías tradicionales muy usadas en entornos empresariales**, y Amazon MQ ofrece una vía sencilla para mantener su uso sin gestionar la infraestructura manualmente.

10. **Cuando se decide entre SQS/SNS y Amazon MQ, el criterio principal debe ser el tipo de protocolo necesario**, es decir, si se necesita compatibilidad con protocolos abiertos, MQ es la elección adecuada.

11. **Amazon MQ no es una solución ideal para cargas de trabajo que requieran escalabilidad masiva**, ya que su capacidad está limitada por los recursos subyacentes del servidor.

12. **A nivel funcional, MQ puede recibir múltiples tipos de eventos desde productores y distribuirlos a múltiples consumidores**, conservando el modelo clásico de publicación/suscripción.

13. **Amazon MQ requiere de configuración y mantenimiento en cuanto a broker, seguridad, y monitoreo**, lo cual implica un mayor nivel de gestión que los servicios completamente administrados como SQS/SNS.

14. **Un ejemplo de uso sería una empresa que tiene un sistema SCADA industrial basado en MQTT**, que migra su infraestructura a AWS y necesita mantener el mismo protocolo de mensajería.

15. **La recomendación general es utilizar SQS y SNS si no se depende explícitamente de protocolos abiertos**, ya que ofrecen mayor rendimiento, menor esfuerzo de mantenimiento y mejor integración con otros servicios de AWS.

# 158. Cloud Integrations Summary

1. **Amazon SQS es un servicio de colas completamente administrado** que permite desacoplar componentes de una aplicación mediante el uso de mensajes persistentes.

2. **Los mensajes en SQS pueden permanecer en la cola hasta 14 días**, tiempo en el cual los consumidores deben leerlos y procesarlos.

3. **Los consumidores en SQS comparten la carga de trabajo**, leyendo mensajes de manera concurrente para asegurar una alta escalabilidad y rendimiento.

4. **Una vez que un consumidor procesa un mensaje, este debe ser eliminado de la cola**, lo que asegura que no se procese dos veces.

5. **SQS es ideal cuando se busca desacoplar componentes**, como separar una aplicación web de un servicio de procesamiento de datos o videos.

6. **Amazon SNS es un servicio de notificaciones basado en el modelo pub/sub (publicador/suscriptor)**, donde un mensaje enviado a un tema se distribuye a todos los suscriptores.

7. **En SNS, no hay persistencia de mensajes**, lo que significa que los mensajes no se almacenan y deben ser entregados inmediatamente.

8. **SNS permite múltiples destinos para los mensajes**, como correos electrónicos, funciones Lambda, colas SQS, endpoints HTTP o dispositivos móviles.

9. **SNS es útil cuando un único evento necesita generar múltiples acciones en paralelo**, como enviar alertas, ejecutar funciones o almacenar registros.

10. **Kinesis es un servicio de streaming en tiempo real**, ideal para recolectar, procesar y analizar datos que llegan de manera continua.

11. **Kinesis permite persistencia de datos y análisis en tiempo real**, lo cual es útil para casos como monitoreo de clics, análisis de logs o métricas de sensores.

12. **Amazon MQ ofrece compatibilidad con protocolos de mensajería tradicionales** como MQTT, AMQP, STOMP, y es ideal para empresas que migran sistemas legados.

13. **Amazon MQ soporta RabbitMQ y ActiveMQ como tecnologías subyacentes**, brindando familiaridad y soporte para aplicaciones que ya usan estos sistemas.

14. **A diferencia de SQS y SNS, MQ requiere instancias de servidor subyacentes**, lo que puede limitar su escalabilidad y aumentar la complejidad operativa.

15. **Elegir entre SQS, SNS, Kinesis y MQ depende del caso de uso**, siendo SQS y SNS más adecuados para arquitecturas modernas y serverless, y MQ más apto para compatibilidad con sistemas legados.