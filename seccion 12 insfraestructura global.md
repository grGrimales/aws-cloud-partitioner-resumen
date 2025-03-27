# 139. Why Global Applications?

1. **Una aplicación global es aquella desplegada en múltiples geografías** y, en AWS, esto significa distribuirla entre varias regiones o Edge Locations para mejorar el rendimiento y disponibilidad.

2. **La principal razón para globalizar una aplicación es reducir la latencia**, ya que los usuarios se conectan a servidores más cercanos geográficamente, acelerando el tiempo de respuesta.

3. **La latencia representa el tiempo que tarda un paquete de red en llegar a un servidor**, por lo tanto, cuanto más lejos esté el servidor, mayor será este tiempo.

4. **Ejemplo práctico: un usuario en India accediendo a un servidor en EE. UU. tendrá mayor latencia** que si accediera a un servidor ubicado en Asia, como en Singapur o Mumbai.

5. **Desplegar una aplicación en múltiples regiones garantiza una mejor experiencia de usuario global**, con menor tiempo de carga y mayor eficiencia en la entrega de contenido.

6. **Otra razón clave para aplicaciones globales es la estrategia de recuperación ante desastres (DR)**, que permite mantener la continuidad del servicio si una región de AWS falla.

7. **Aunque una región completa de AWS nunca ha fallado**, existe la posibilidad debido a desastres naturales, fallos eléctricos o factores políticos, lo que hace indispensable una estrategia de failover.

8. **Una arquitectura multirregional aumenta la disponibilidad y confiabilidad del sistema**, al permitir que los usuarios sean redirigidos automáticamente a otra región funcional en caso de caída.

9. **La tercera razón para optar por una aplicación global es la resiliencia ante ataques**, ya que una arquitectura distribuida es más difícil de colapsar por parte de atacantes.

10. **Al dispersar la infraestructura, se complica para un atacante tumbar toda la aplicación**, reduciendo la efectividad de ataques DDoS y otros ataques coordinados.

11. **AWS ofrece un mapa interactivo donde se visualizan las regiones, zonas de disponibilidad (AZs) y puntos de presencia (PoPs)**, facilitando entender la cobertura global del proveedor.

12. **Cada región de AWS está compuesta por varias zonas de disponibilidad**, que a su vez están formadas por uno o más centros de datos físicamente separados pero conectados con redes rápidas.

13. **Las AZs están diseñadas para estar suficientemente alejadas unas de otras**, para evitar que un desastre afecte a todas al mismo tiempo, pero a la vez conectadas con baja latencia.

14. **Los Points of Presence o Edge Locations son ubicaciones especiales para entrega de contenido**, utilizados por servicios como CloudFront para estar más cerca del usuario final.

15. **En el mapa de AWS, los PoPs están marcados en color rosado y no son lugares donde se despliega infraestructura**, sino nodos de entrega de contenido para mejorar el acceso a archivos estáticos.

16. **La red de AWS incluye conexiones privadas y cables submarinos que conectan continentes y regiones**, permitiendo una comunicación estable, segura y rápida entre todos sus puntos.

17. **Servicios como Route 53, CloudFront, S3 Transfer Acceleration y Global Accelerator permiten implementar estrategias globales**, cada uno enfocado en mejorar rendimiento y resiliencia desde distintos frentes.

18. **Route 53 ayuda a dirigir a los usuarios a la instancia más cercana geográficamente**, optimizando rutas DNS para disminuir latencia y reforzar la estrategia de recuperación ante fallos.

19. **CloudFront actúa como CDN para cachear y replicar contenido en Edge Locations**, reduciendo carga en servidores de origen y mejorando tiempos de entrega para usuarios de todo el mundo.

# 140. Route 53 Overview

1. **Amazon Route 53 es un servicio DNS gestionado por AWS**, que permite traducir nombres de dominio legibles por humanos a direcciones IP utilizadas por las máquinas.

2. **El DNS funciona como una agenda telefónica de Internet**, permitiendo que los navegadores localicen servidores a partir de nombres de dominio como `www.miapp.com`.

3. **Una A Record mapea un nombre de dominio a una dirección IPv4**, por ejemplo: `www.miapp.com → 192.0.2.1`.

4. **Una AAAA Record realiza la misma función pero con direcciones IPv6**, como `www.miapp.com → 2001:db8::1`.

5. **Una CNAME Record mapea un nombre de host a otro nombre de host**, útil cuando se redirige a otro dominio, como `blog.miapp.com → www.miapp.com`.

6. **Una Alias Record es específica de AWS y permite apuntar a recursos como Load Balancers, CloudFront o buckets S3**, siendo más flexible y sin costo adicional en ciertas condiciones.

7. **Route 53 permite crear estos registros para que los usuarios accedan a sus aplicaciones por medio de URLs**, sin necesidad de recordar IPs.

8. **El funcionamiento básico de un A Record implica que cuando un navegador consulta `app.miweb.com`**, Route 53 responde con la IP del servidor, permitiendo establecer la conexión HTTP.

9. **Route 53 incluye políticas de enrutamiento para dirigir el tráfico según diferentes criterios**, fundamentales para optimizar disponibilidad, rendimiento y recuperación ante fallos.

10. **La Simple Routing Policy es la más básica y no incluye health checks**, simplemente responde con la IP configurada para el dominio solicitado.

11. **La Weighted Routing Policy permite distribuir el tráfico entre múltiples instancias usando pesos**, por ejemplo: 70% a una, 20% a otra y 10% a una tercera, útil para pruebas A/B o escalamiento gradual.

12. **Esta política sí permite utilizar health checks para evitar enviar tráfico a instancias no saludables**, asegurando una mejor disponibilidad.

13. **La Latency Routing Policy dirige a los usuarios a la instancia con menor latencia**, según su ubicación geográfica y el tiempo estimado de respuesta a las distintas regiones de AWS.

14. **Ejemplo: usuarios de Europa acceden a instancias en Irlanda, mientras que usuarios de Oceanía acceden a instancias en Sídney**, lo que mejora la experiencia del usuario.

15. **La Failover Routing Policy permite tener una instancia primaria y una de respaldo (failover)**, funcionando como solución de recuperación ante desastres.

16. **En esta política se realiza constantemente un health check a la instancia primaria**, y si esta falla, Route 53 redirige automáticamente el tráfico hacia la instancia secundaria.

17. **Solo la política simple carece de verificación de estado; las otras tres (Weighted, Latency y Failover) utilizan health checks**, lo que las hace adecuadas para escenarios de alta disponibilidad, balanceo de carga y continuidad operativa.

# 141. Route 53 Hands On

1. **Para comenzar a usar Route 53 es necesario registrar un dominio**, lo cual se puede hacer desde la consola de AWS, y tiene un costo aproximado de 12 dólares por año.

2. **Una vez registrado el dominio, se crea automáticamente una zona alojada (Hosted Zone)**, donde se almacenarán los registros DNS que dirigirán el tráfico a las instancias de EC2.

3. **Se crean dos instancias EC2 en diferentes regiones**, una en Irlanda (eu-west-1) y otra en Oregón (us-west-2), cada una configurada para mostrar un mensaje distinto al acceder por HTTP.

4. **En ambas instancias se configura el User Data** para lanzar un servidor web que responde con el mensaje "Hello World from [región]".

   ```bash
   #!/bin/bash
   echo "Hello World from Ireland" > /var/www/html/index.html
   yum install -y httpd
   systemctl start httpd
   systemctl enable httpd
   ```

5. **Se asigna un grupo de seguridad a las instancias que permita tráfico HTTP (puerto 80)** desde cualquier lugar, sin necesidad de habilitar SSH.

6. **Cada instancia obtiene una IP pública**, que se utilizará para crear los registros A en Route 53.

7. **Se crean dos registros A con el mismo nombre (www.stephane-ccp.com)** pero con diferentes direcciones IP, cada una apuntando a una de las instancias EC2.

8. **Se utiliza la política de enrutamiento por latencia (Latency Routing Policy)**, que permite que los usuarios sean dirigidos a la instancia más cercana según la latencia de red.

9. **Cada registro A especifica la región correspondiente**, por ejemplo "EU-West-1" para Irlanda y "US-West-2" para Oregón.

10. **El navegador del usuario realiza una consulta DNS a Route 53**, y este responde con la IP del servidor más cercano geográficamente o con menor latencia.

11. **Cuando se accede al dominio desde Europa**, la respuesta es "Hello World from Ireland", ya que Irlanda es la región más próxima.

12. **Al utilizar una VPN para simular una conexión desde EE.UU.**, Route 53 redirige la solicitud a la instancia en Oregón, mostrando "Hello World from the US".

13. **Este comportamiento confirma que la política de latencia está funcionando correctamente**, seleccionando el servidor más cercano en tiempo de respuesta.

14. **Route 53 permite administrar dominios y registros de manera centralizada**, facilitando la configuración de aplicaciones globales con balanceo inteligente del tráfico.

15. **Al utilizar políticas de enrutamiento avanzadas como la de latencia**, se mejora significativamente la experiencia del usuario final al reducir el tiempo de carga.

16. **Es posible ver y editar los registros desde la consola de Route 53**, observando que hay dos registros para el mismo subdominio con diferentes ubicaciones y prioridades.

17. **Los registros DNS pueden combinarse con otras políticas como failover o weighted**, ampliando las posibilidades de distribución de tráfico según el caso de uso.

18. **Al finalizar el experimento, se recomienda eliminar las instancias EC2 para evitar costos adicionales**, así como desactivar la renovación automática del dominio si no se desea mantener.

19. **Este ejercicio demuestra cómo Route 53 puede ser usado como parte clave de una arquitectura global**, permitiendo balancear tráfico geográficamente sin necesidad de infraestructura compleja.

20. **Route 53 ofrece integración con otros servicios de AWS como CloudFront, ELB y S3**, mediante registros Alias que permiten redireccionar sin necesidad de una IP pública.

21. **Desde una perspectiva de examen, es fundamental comprender cómo configurar registros A, CNAME y Alias**, así como las diferencias entre las políticas de enrutamiento disponibles en Route 53.


# 142. CloudFront Overview

1. **CloudFront es una red de entrega de contenido (CDN)** que mejora el rendimiento de lectura al almacenar en caché el contenido en ubicaciones distribuidas globalmente llamadas Edge Locations.

2. **Con más de 216 puntos de presencia en todo el mundo**, CloudFront permite que los usuarios accedan a los datos desde ubicaciones más cercanas, reduciendo significativamente la latencia.

3. **El contenido distribuido globalmente a través de CloudFront también ofrece protección contra ataques DDoS**, ayudado por servicios adicionales como AWS Shield y WAF (Web Application Firewall).

4. **Cuando un usuario solicita contenido almacenado en S3 en Australia desde EE.UU.**, la primera vez el contenido se trae desde el origen y se almacena en la Edge Location de EE.UU., desde donde se servirá a futuras solicitudes.

5. **CloudFront admite múltiples orígenes de contenido**, como buckets S3, balanceadores de carga (ALB), instancias EC2 y cualquier backend HTTP personalizado.

6. **Cuando se utiliza un bucket S3 como origen**, se puede restringir el acceso solo a CloudFront usando Origin Access Control (OAC), que reemplaza al método anterior Origin Access Identity (OAI).

7. **CloudFront también puede gestionar el tráfico de entrada (ingress)**, permitiendo enviar archivos desde el cliente hasta un bucket S3 utilizando la red global de AWS.

8. **Al colocar CloudFront frente a un backend HTTP**, se requiere habilitar el sitio web estático si se trata de un bucket S3 actuando como sitio web.

9. **El flujo de trabajo básico de CloudFront implica** que cuando un cliente realiza una solicitud, el edge verifica si el contenido ya está en la caché. Si no está, lo solicita al origen, lo guarda en caché y lo entrega al cliente.

10. **Una vez cacheado, futuras solicitudes similares serán respondidas directamente desde el edge**, sin necesidad de consultar nuevamente al origen.

11. **Los orígenes como buckets S3 deben estar protegidos adecuadamente**, utilizando OAC y políticas de bucket que limiten el acceso únicamente a CloudFront.

12. **El tráfico entre las Edge Locations y el origen se realiza a través de la red privada de AWS**, lo cual mejora tanto la seguridad como el rendimiento.

13. **Por ejemplo, un usuario en São Paulo será atendido por una Edge Location cercana en Brasil**, que a su vez se conecta privadamente con el origen para obtener el contenido, si es necesario.

14. **La gran ventaja de CloudFront es que permite que el contenido alojado en un solo bucket S3 se distribuya globalmente**, reduciendo la latencia y mejorando la experiencia de usuario.

15. **CloudFront se diferencia de S3 Cross-Region Replication**, ya que este último replica buckets completos en otras regiones, mientras que CloudFront solo cachea archivos populares durante cierto tiempo en múltiples ubicaciones.

16. **S3 Replication se configura por región y no almacena en caché**, siendo ideal para datos dinámicos que deben estar sincronizados en tiempo real entre regiones específicas.

17. **CloudFront es ideal para contenido estático de alta demanda**, como imágenes, videos, archivos CSS o JS, mientras que S3 Replication se usa más en escenarios de alta disponibilidad o respaldo.

18. **Al integrar CloudFront con otros servicios como S3, EC2 o ALB**, se crea una arquitectura global eficiente que ofrece velocidad, seguridad y escalabilidad sin comprometer la experiencia del usuario.

# 143. CloudFront Hands On

1. **El primer paso para usar CloudFront consiste en crear un bucket S3**, que contendrá los archivos a distribuir globalmente a través de la CDN.

2. **Después de crear el bucket**, se subieron archivos estáticos como `index.html`, `beach.jpeg` y `coffee.jpeg`, que serán accesibles mediante CloudFront.

3. **Intentar acceder directamente al archivo HTML mediante su URL genera un error de acceso denegado**, ya que los objetos no son públicos por defecto en S3.

4. **Al usar el botón “Open” en S3 se genera una URL prefirmada**, que permite acceder temporalmente a los archivos, pero no es una solución práctica para acceso público global.

5. **Para resolver esto sin hacer públicos los objetos**, se configura una distribución en CloudFront, que servirá los archivos con permisos controlados.

6. **CloudFront es un servicio global y no requiere selección de región**, lo que facilita su configuración para aplicaciones distribuidas.

7. **En la configuración de origen de CloudFront**, se selecciona el bucket de S3 como fuente de contenido para la distribución.

8. **El acceso al bucket se controla mediante un mecanismo llamado OAC (Origin Access Control)**, que reemplaza a la antigua OAI (Origin Access Identity).

9. **Se debe crear un OAC**, nombrarlo y luego integrarlo con la distribución para garantizar que CloudFront pueda acceder a los objetos del bucket.

10. **Luego se debe actualizar la política del bucket S3** para permitir a la distribución específica de CloudFront acceder a los objetos usando el OAC.

11. **La política generada automáticamente incluye permisos para realizar la acción `s3:GetObject`**, condicionada a que la solicitud provenga de una distribución CloudFront autorizada.

12. **En los ajustes de CloudFront, se define el objeto raíz por defecto** (por ejemplo `index.html`), que será cargado al acceder directamente al dominio de la distribución.

13. **Una vez creada la distribución**, puede tardar unos minutos en estar disponible, ya que CloudFront propaga la configuración a todos sus edge locations.

14. **Al probar el dominio de CloudFront en el navegador**, se accede correctamente al contenido `index.html`, incluyendo imágenes previamente inaccesibles directamente desde S3.

15. **Acceder directamente a `/coffee.jpeg` o `/beach.jpeg` desde la distribución funciona correctamente**, demostrando que los archivos están disponibles globalmente.

16. **La caché de CloudFront mejora el rendimiento al evitar múltiples solicitudes al bucket S3**, ya que los objetos se almacenan en edge locations tras la primera petición.

17. **La consola de CloudFront permite gestionar fácilmente los OACs activos**, asignados a distribuciones específicas, asegurando un acceso controlado y seguro.

18. **El uso de CloudFront permite servir contenido estático de manera segura, rápida y eficiente**, sin necesidad de hacer públicos los objetos ni comprometer la seguridad de los datos almacenados en S3.

# 144. S3 Transfer Acceleration

1. **Amazon S3 vincula cada bucket a una sola región**, lo que puede generar alta latencia al transferir archivos desde ubicaciones lejanas.

2. **S3 Transfer Acceleration permite acelerar la carga y descarga de archivos** desde y hacia un bucket ubicado en una región distinta a la del usuario.

3. **El mecanismo utiliza edge locations de AWS** para recibir los archivos cerca del usuario y luego los transfiere a través de la red privada de AWS hasta el bucket S3 destino.

4. **Esta técnica reduce significativamente el tiempo de transferencia**, ya que se evita enviar los archivos por redes públicas de larga distancia.

5. **El uso de S3 Transfer Acceleration es ideal para aplicaciones globales**, donde múltiples usuarios desde distintas partes del mundo deben interactuar con un único bucket.

6. **La funcionalidad se activa en el bucket S3**, y al hacerlo se habilita un nuevo endpoint para el bucket que utiliza la aceleración, con la forma:  
   `bucketname.s3-accelerate.amazonaws.com`.

7. **La herramienta de prueba oficial de AWS permite evaluar el rendimiento** de S3 Transfer Acceleration frente a una carga directa sin aceleración.

8. **Durante la prueba, se mide la velocidad de carga desde la ubicación actual del usuario** hacia un bucket S3 en distintas regiones, tanto con como sin aceleración.

9. **En el caso práctico presentado, se realizó la prueba desde Europa**, cargando archivos hacia el bucket ubicado en la región de Northern Virginia (us-east-1).

10. **Los resultados mostraron una mejora del 13% con la aceleración habilitada**, lo que demuestra su efectividad incluso con una buena conexión a internet.

11. **Otras regiones como San Francisco, Oregon, Dublin y Frankfurt también mostraron mejoras claras**, evidenciando que el beneficio se extiende globalmente.

12. **La mejora depende de factores como la calidad de conexión local, la ubicación geográfica y el tamaño de los archivos transferidos.**

13. **Este enfoque es útil para arquitecturas de aplicaciones que requieren centralizar los datos**, especialmente cuando los usuarios generan contenido desde distintos países.

14. **S3 Transfer Acceleration solo aplica a cargas y descargas**, no a otras operaciones del bucket como listados de objetos o administración de permisos.

15. **La activación del servicio puede tener un costo adicional**, por lo tanto, debe analizarse su uso en función del volumen de datos y la dispersión geográfica de los usuarios.

# 145. AWS Global Accelerator

1. **AWS Global Accelerator mejora la disponibilidad y el rendimiento de aplicaciones globales** al enrutar el tráfico a través de la red privada global de AWS.

2. **Las solicitudes de los usuarios se redirigen primero a una edge location**, la cual actúa como punto de entrada al backbone privado de AWS que conecta las regiones.

3. **Este mecanismo permite optimizar el enrutamiento de red hasta en un 60%**, reduciendo la latencia y mejorando la confiabilidad de la conexión.

4. **El tráfico en la red pública solo ocurre entre el usuario y la edge location más cercana**, y desde allí se utiliza la red privada de AWS hasta la aplicación.

5. **Los usuarios acceden a la aplicación a través de dos direcciones IP estáticas**, conocidas como Anycast IPs, que son gestionadas por AWS y no cambian con el tiempo.

6. **El uso de IPs Anycast permite una conmutación por error automática y eficiente**, ya que siempre se enruta al edge location óptimo según la ubicación del cliente.

7. **Una diferencia clave con CloudFront es que Global Accelerator no realiza caching**, sino que reenvía todas las solicitudes a la aplicación en la región destino.

8. **Global Accelerator mejora el rendimiento para todo tipo de tráfico TCP o UDP**, no solo HTTP, lo que lo hace útil en escenarios como juegos en línea, VoIP o APIs críticas.

9. **CloudFront se utiliza para entregar contenido estático como imágenes, videos o páginas web**, almacenándolos en caché en edge locations para servirlos más rápidamente.

10. **Ambos servicios comparten ciertas ventajas**, como integración con AWS Shield para protección contra ataques DDoS y uso del backbone global de AWS.

11. **Global Accelerator es ideal cuando se requieren IPs fijas**, failover regional rápido y rendimiento consistente para aplicaciones que no pueden beneficiarse del caching.

12. **AWS proporciona una herramienta de comparación de velocidad**, que permite medir las mejoras de rendimiento entre el uso de internet directo y Global Accelerator.

13. **En el caso de pruebas desde Europa, el uso de Global Accelerator mostró mejoras del 23% en Norteamérica**, lo cual valida su efectividad en escenarios intercontinentales.

14. **La mejora fue aún más evidente en regiones lejanas como Sídney (53%) y Singapur (34%)**, demostrando el valor de este servicio para usuarios internacionales.

15. **En regiones cercanas como Irlanda y Frankfurt**, no se observaron diferencias significativas, ya que la proximidad geográfica reduce la latencia de por sí.

16. **La herramienta evalúa descargas de archivos pequeños (5 MB)**, lo que simula escenarios comunes como cargas y descargas de datos, páginas o archivos multimedia.

17. **Global Accelerator se configura para enrutar automáticamente a instancias o balanceadores en múltiples regiones**, facilitando la resiliencia y alta disponibilidad.

18. **En aplicaciones globales que requieren comunicación rápida y confiable desde cualquier punto del planeta**, Global Accelerator representa una ventaja competitiva clave.

# 146. AWS Outposts

1. **AWS Outposts permite extender los servicios de AWS directamente a centros de datos on-premises**, brindando una experiencia de nube híbrida con infraestructura gestionada por AWS.

2. **El concepto de nube híbrida implica mantener infraestructura local junto con la nube pública**, lo que normalmente genera duplicación de herramientas, APIs y habilidades.

3. **Con Outposts, las empresas pueden unificar su forma de operar**, utilizando la misma consola, CLI y APIs que en la nube, incluso cuando los recursos están físicamente en su propia infraestructura.

4. **AWS entrega racks físicos preconfigurados**, conocidos como Outposts, que se instalan en los centros de datos de los clientes y permiten ejecutar servicios de AWS localmente.

5. **Una diferencia clave entre EC2 en la nube y EC2 en Outposts** es la responsabilidad del cliente sobre la seguridad física del rack, dado que este se encuentra dentro de sus propias instalaciones.

6. **Outposts mantiene la infraestructura sincronizada con la nube de AWS**, lo que facilita migraciones progresivas y evita la necesidad de rediseñar arquitecturas para moverse entre entornos.

7. **El procesamiento local de datos es una de sus mayores ventajas**, permitiendo ejecutar cargas sensibles a la latencia sin enviar datos a regiones remotas.

8. **La residencia de datos es otra ventaja significativa**, ya que los datos pueden permanecer físicamente en el país o región donde están instalados los racks, cumpliendo con regulaciones locales.

9. **Outposts ofrece una solución de baja latencia** ideal para aplicaciones en tiempo real o que necesitan alta disponibilidad local.

10. **Las empresas pueden comenzar migrando sus cargas a Outposts** como paso intermedio antes de moverse completamente a la nube pública de AWS.

11. **La gestión es completamente delegada a AWS**, lo que reduce la carga operativa del equipo interno de TI al mantener y actualizar el hardware y software.

12. **Se pueden ejecutar servicios clave como EC2 para cómputo, EBS para almacenamiento de bloques**, y S3 para almacenamiento de objetos (aunque en modo limitado, inicialmente).

13. **También están disponibles servicios de contenedores como ECS y EKS**, lo que permite la ejecución de aplicaciones en contenedores con la misma experiencia que en la nube.

14. **Para bases de datos, es posible utilizar Amazon RDS en Outposts**, conservando opciones como MySQL y PostgreSQL.

15. **Amazon EMR también está soportado**, lo que habilita procesamiento de big data y analítica distribuida directamente en la infraestructura local.

16. **AWS Outposts representa una evolución estratégica para organizaciones** que necesitan cumplir con normativas, minimizar la latencia o mantener ciertas cargas dentro de sus instalaciones mientras se integran con la nube.

# 147. AWS WaveLength

1. **AWS WaveLength permite extender servicios de AWS directamente a las redes 5G**, ubicando infraestructura en los centros de datos de los proveedores de telecomunicaciones.

2. **Estas zonas WaveLength están integradas en el borde de la red 5G**, lo que significa que las aplicaciones pueden ejecutarse lo más cerca posible del usuario final para lograr latencias ultrabajas.

3. **Se pueden desplegar servicios de AWS como EC2, EBS y VPC dentro de una zona WaveLength**, brindando capacidades de cómputo y almacenamiento directamente en la red del proveedor.

4. **La conexión entre los dispositivos móviles 5G y las instancias en WaveLength es directa**, evitando el paso por la red pública de Internet y reduciendo así la latencia drásticamente.

5. **El tráfico dentro de la red del proveedor de servicios de comunicación (CSP)** permanece dentro de esa red mientras no se necesite comunicación con otros servicios en AWS.

6. **Si las instancias EC2 en WaveLength necesitan conectarse con servicios como RDS o DynamoDB**, lo hacen a través de la región principal de AWS, que se encuentra conectada a la WaveLength Zone.

7. **El Carrier Gateway es el componente que permite enrutar el tráfico entre las zonas WaveLength y el resto de AWS**, proporcionando la integración necesaria sin comprometer la baja latencia.

8. **WaveLength es ideal para aplicaciones que dependen de respuestas en tiempo real**, como el gaming, la realidad aumentada (AR) o la realidad virtual (VR).

9. **Los desarrolladores pueden aprovechar WaveLength para casos como diagnósticos médicos asistidos por ML**, donde la rapidez en el procesamiento puede ser crítica.

10. **La transmisión de video en vivo interactivo es otro caso clave de uso**, ya que los espectadores pueden experimentar retrasos mínimos y una mayor calidad de servicio.

11. **Las ciudades inteligentes (Smart Cities)** se benefician al procesar datos de sensores y cámaras cerca del punto de captura, evitando demoras por transferencia de datos a regiones lejanas.

12. **Los vehículos conectados pueden tomar decisiones más rápidas**, como en sistemas de navegación o prevención de accidentes, al tener los servidores más cerca geográficamente.

13. **No se aplican costos adicionales ni acuerdos especiales por usar WaveLength**, lo que facilita su adopción por parte de empresas con aplicaciones sensibles a la latencia.

14. **La latencia extremadamente baja se logra al ejecutar las cargas directamente sobre la red 5G**, lo que elimina cuellos de botella típicos de las redes tradicionales.

15. **WaveLength puede considerarse como una extensión de la nube pública a la periferia móvil**, combinando el poder de la nube con la inmediatez de la infraestructura local de telecomunicaciones.

# 148. AWS Local Zones

1. **AWS Local Zones permiten extender una región de AWS hacia ubicaciones geográficas cercanas al usuario final**, ofreciendo servicios con baja latencia.

2. **Estas zonas locales permiten ejecutar servicios como EC2, EBS, RDS, ECS, ElastiCache y Direct Connect más cerca del cliente**, lo cual es ideal para aplicaciones sensibles al tiempo de respuesta.

3. **Las Local Zones son extensiones de una región principal y no regiones independientes**, lo que significa que comparten la misma infraestructura subyacente y control administrativo.

4. **Cada Local Zone se comporta como una zona de disponibilidad adicional**, pero está ubicada fuera de la región física principal.

5. **Un caso de uso típico sería una aplicación que requiere baja latencia en una ciudad específica**, como Boston, mientras su backend se encuentra en una región como US-East-1 (Norte de Virginia).

6. **Desde el panel de configuración de AWS, se puede activar manualmente una Local Zone para que esté disponible en la consola**, usando la opción “Zones” en el menú de configuración del EC2.

7. **Las zonas locales deben habilitarse explícitamente**, ya que por defecto no están activadas en una región, a diferencia de las zonas de disponibilidad regulares.

8. **En la consola de EC2, al seleccionar la región US-East-1, se muestran tanto las zonas de disponibilidad, como las Local Zones y las WaveLength Zones**, cada una con su propio propósito.

9. **Habilitar una Local Zone requiere actualizar el grupo de zonas**, lo que permite que esa ubicación forme parte de la red VPC para lanzar recursos.

10. **Una vez habilitada, se puede crear una subred específica para esa Local Zone**, como por ejemplo `us-east-1-boston-1`.

11. **Crear una subred implica definir una máscara CIDR apropiada**, como `172.31.96.0/20`, aunque este paso pertenece a aspectos más avanzados de redes en AWS.

12. **Una vez creada la subred, esta aparece como una opción al momento de lanzar una instancia EC2**, permitiendo elegir una zona específica, como la de Boston.

13. **Este modelo permite distribuir los recursos más estratégicamente**, acercando las cargas de trabajo a los usuarios finales sin necesidad de crear una región nueva.

14. **Las Local Zones son especialmente útiles para sectores como medios de comunicación, videojuegos, transmisión en vivo y salud**, donde la latencia es crítica.

15. **Desde el punto de vista de red, extender una VPC hacia una Local Zone implica integración directa a través de subredes**, sin necesidad de túneles o emparejamiento de VPC adicionales.

16. **Aunque estas funcionalidades son avanzadas, comprender su existencia y propósito es suficiente para el examen de certificación**, sin necesidad de dominar toda la configuración técnica. 

17. **El uso de Local Zones refleja un enfoque híbrido moderno**, donde se busca la mejor experiencia del usuario combinando cercanía geográfica y control centralizado desde la región principal.

# 149. Global Applications Architecture

1. **Una sola instancia EC2 en una única zona de disponibilidad (AZ) dentro de una región no proporciona alta disponibilidad**, ni mejora la latencia para usuarios globales.

2. **Este tipo de arquitectura es muy simple de configurar**, lo que implica una dificultad baja, pero no cumple con requisitos de resiliencia o rendimiento global.

3. **Una arquitectura de una sola región con múltiples AZs mejora la alta disponibilidad**, ya que si una AZ falla, otra puede mantener el servicio activo.

4. **A pesar de tener alta disponibilidad, la arquitectura de múltiples AZs en una sola región no mejora la latencia global**, ya que todas las AZs están ubicadas físicamente cerca.

5. **El nivel de dificultad técnica aumenta levemente en comparación con una sola AZ**, ya que se necesita configurar balanceo entre AZs y posiblemente replicación de datos.

6. **En una arquitectura multi-región activa-pasiva, una región actúa como activa y otra como pasiva**, permitiendo lecturas y escrituras solo en la activa.

7. **La región pasiva sirve como respaldo y puede configurarse para permitir solo operaciones de lectura**, pero no de escritura.

8. **Se implementa replicación de datos desde la región activa hacia la pasiva**, para mantener la sincronización en caso de falla o desastre.

9. **La mejora principal de la arquitectura activa-pasiva es la latencia de lectura global**, ya que los datos están replicados cerca de los usuarios en diferentes regiones.

10. **Sin embargo, la latencia de escritura sigue siendo alta a nivel global**, ya que todas las escrituras deben dirigirse a la región activa.

11. **La configuración de una arquitectura activa-pasiva implica un nivel de dificultad técnica considerablemente mayor**, debido a la necesidad de gestionar múltiples regiones y sincronización de datos.

12. **La arquitectura activa-activa permite que múltiples instancias EC2 distribuidas en distintas regiones puedan manejar lecturas y escrituras simultáneamente**, reduciendo la latencia tanto de lectura como de escritura a nivel global.

13. **Esta configuración requiere replicación bidireccional entre las regiones**, lo cual implica una mayor complejidad en el diseño y en la lógica de la aplicación.

14. **Una aplicación distribuida activamente en varias regiones puede brindar una experiencia global coherente**, pero requiere mecanismos de resolución de conflictos y consistencia eventual si los datos se actualizan simultáneamente en diferentes regiones.

15. **Una solución nativa de AWS que implementa la estrategia activa-activa es DynamoDB Global Tables**, la cual replica automáticamente los datos entre regiones con consistencia eventual.

16. **Conocer las diferencias entre las arquitecturas activa-activa, activa-pasiva, y las arquitecturas regionales simples es crucial para diseñar aplicaciones globales en AWS**, y es un conocimiento clave para el examen de certificación.

17. **Elegir la arquitectura adecuada dependerá de los requisitos específicos de latencia, tolerancia a fallos y complejidad de mantenimiento** que tenga cada aplicación distribuida globalmente.

# 150. Leveraging the AWS Global Infrastructure Summary

1. **Route 53 permite enrutar a los usuarios hacia el despliegue más cercano con la menor latencia**, lo que resulta fundamental para optimizar el rendimiento global y planificar estrategias de recuperación ante desastres.

2. **La creación de registros DNS con Route 53 facilita el mapeo de nombres de host a direcciones IP**, incluyendo políticas de enrutamiento como latencia, peso o conmutación por error (failover).

3. **CloudFront actúa como una red de entrega de contenido (CDN) global**, capaz de replicar datos estáticos de aplicaciones en ubicaciones periféricas de AWS (Edge Locations).

4. **El uso de CloudFront reduce la latencia y mejora la experiencia del usuario**, al almacenar en caché respuestas comunes y servir el contenido desde el nodo más cercano.

5. **Al integrar CloudFront con Amazon S3, se consigue una entrega eficiente de archivos estáticos** como imágenes, HTML o vídeos desde los Edge Locations sin necesidad de acceder cada vez al bucket de S3.

6. **S3 Transfer Acceleration permite subir o descargar archivos de manera más rápida desde regiones lejanas**, utilizando las Edge Locations para enrutar los datos por la red interna de AWS.

7. **El aumento de velocidad con Transfer Acceleration puede llegar a ser considerable**, especialmente si los usuarios se encuentran lejos de la región del bucket de S3.

8. **AWS Global Accelerator mejora la disponibilidad y el rendimiento global de las aplicaciones**, enruta el tráfico por la red privada global de AWS en lugar de usar la red pública de Internet.

9. **Global Accelerator proporciona dos direcciones IP estáticas (Anycast)** y enruta las solicitudes al Edge Location más cercano, lo que permite una conexión más rápida y confiable hacia la aplicación.

10. **Outposts extiende la infraestructura de AWS hacia centros de datos locales**, permitiendo usar servicios como EC2, S3 o RDS en instalaciones on-premises.

11. **Con Outposts, las empresas pueden beneficiarse de la nube sin migrar completamente**, aprovechando baja latencia, procesamiento local y cumplimiento de requisitos de residencia de datos.

12. **AWS WaveLength lleva servicios de AWS al borde de las redes 5G**, permitiendo el despliegue de instancias EC2 y almacenamiento en centros de datos de proveedores de telecomunicaciones.

13. **WaveLength proporciona una latencia ultra baja**, ideal para casos como vehículos conectados, ciudades inteligentes, transmisión en vivo interactiva y gaming en tiempo real.

14. **Local Zones extiende los servicios de computación, almacenamiento y bases de datos de AWS a ubicaciones metropolitanas específicas**, mejorando el acceso local para aplicaciones sensibles a la latencia.

15. **Local Zones actualmente están disponibles principalmente en ciudades de Estados Unidos**, como Boston, Miami o Dallas, y permiten extender VPCs a esas zonas para lanzar recursos cerca del usuario.

16. **El aprovechamiento de toda la infraestructura global de AWS permite construir aplicaciones resilientes, rápidas y escalables en todo el mundo**, eligiendo estratégicamente entre Route 53, CloudFront, Transfer Acceleration, Global Accelerator, Outposts, WaveLength y Local Zones según el caso de uso.