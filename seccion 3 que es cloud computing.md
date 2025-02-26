**7. Traditional IT Overview**

1. **Funcionamiento básico de los sitios web:** Un sitio web está alojado en un servidor y el cliente (navegador) se comunica con él a través de una red. El cliente envía paquetes de datos al servidor, que responde con la información solicitada. Este proceso es similar al envío de cartas a través de un sistema postal.

2. **Direcciones IP y comunicación en red:** Tanto los clientes como los servidores tienen direcciones IP únicas que permiten la localización y el intercambio de información. Al igual que una carta con remitente y destinatario, los dispositivos en una red usan direcciones IP para enviarse datos mutuamente.

3. **Componentes esenciales de un servidor:** Un servidor consta de una CPU (procesador), memoria RAM y almacenamiento. La CPU realiza cálculos, la RAM almacena datos temporales y el almacenamiento (HDD/SSD) guarda información de forma permanente. Para bases de datos estructuradas, se emplean sistemas como MySQL o PostgreSQL.

4. **Redes y dispositivos de conexión:** Los servidores están conectados a redes mediante routers y switches. Los routers encaminan paquetes de datos en internet, mientras que los switches dirigen los paquetes dentro de una red local. Un router actúa como la oficina de correos, y un switch como el personal de distribución dentro del edificio.

5. **Evolución del alojamiento de servidores:** Inicialmente, las empresas alojaban sus servidores en garajes o habitaciones dedicadas. A medida que crecían, se crearon centros de datos con múltiples servidores en un entorno controlado para escalar la infraestructura.

6. **Problemas del alojamiento tradicional de servidores:** Mantener servidores propios implica costos elevados en alquiler, electricidad, refrigeración y mantenimiento. Además, escalar la infraestructura requiere tiempo, ya que se deben comprar e instalar nuevos servidores manualmente.

7. **Limitaciones del escalamiento tradicional:** Si una empresa crece rápidamente, necesita más servidores, lo que implica limitaciones físicas y logísticas. Un aumento súbito del tráfico web puede colapsar un sistema si no se cuenta con servidores suficientes.

8. **Requerimientos de personal técnico:** La administración de servidores requiere un equipo especializado disponible 24/7 para monitorear el sistema, solucionar fallos y realizar mantenimiento. Sin este equipo, una empresa corre el riesgo de sufrir interrupciones en sus servicios.

9. **Riesgos asociados a la infraestructura física:** Desastres como incendios, cortes de electricidad o terremotos pueden destruir un centro de datos y provocar la pérdida de información. Esto hace necesario contar con planes de recuperación de desastres y copias de seguridad.

10. **Ejemplo de implementación de un servidor web:** Un servidor HTTP como Apache o Nginx se configura para servir páginas web. Ejemplo en Python con Flask:

    ```python
    from flask import Flask
    app = Flask(__name__)

    @app.route('/')
    def home():
        return "Hola, mundo!"

    if __name__ == '__main__':
        app.run(host='0.0.0.0', port=80)
    ```

    Este código inicia un servidor web local accesible desde la red.

11. **Alternativas al modelo tradicional:** Las empresas buscan alternativas para externalizar la gestión de servidores y evitar sus costos y complejidades. Aquí es donde entra en juego la computación en la nube, que proporciona recursos bajo demanda sin necesidad de infraestructura propia.

12. **La nube como solución:** La computación en la nube permite externalizar la infraestructura a proveedores como AWS, Azure o Google Cloud. Estos ofrecen escalabilidad, redundancia, disponibilidad global y optimización de costos, eliminando la necesidad de administrar centros de datos propios.

**8. What is Cloud Computing?**

1. **Definición de computación en la nube:** Se refiere a la entrega bajo demanda de potencia de cómputo, almacenamiento y otros recursos de TI a través de una plataforma de servicios en la nube. El modelo de pago por uso permite pagar solo por los recursos consumidos sin inversiones iniciales.

2. **Escalabilidad y flexibilidad:** La nube permite ajustar la capacidad de cómputo según la demanda. Un usuario puede aumentar o reducir la cantidad de servidores en segundos sin necesidad de comprar hardware físico.

3. **Acceso inmediato a los recursos:** No es necesario esperar horas o días para la provisión de servidores o almacenamiento. Con la nube, los recursos están disponibles en segundos a través de una interfaz fácil de usar.

4. **Ejemplo de aprovisionamiento de un servidor en AWS:**
   ```python
   import boto3
   ec2 = boto3.client('ec2')
   response = ec2.run_instances(
       ImageId='ami-12345678',
       InstanceType='t2.micro',
       MinCount=1,
       MaxCount=1
   )
   print("Instancia creada: ", response['Instances'][0]['InstanceId'])
   ```
   Este código en Python lanza una instancia EC2 en AWS en segundos.

5. **Ejemplos de servicios en la nube:** Servicios como Gmail, Dropbox y Netflix usan la nube para almacenar datos y procesar información sin que el usuario tenga que gestionar la infraestructura.

6. **Tipos de nubes:** Existen tres tipos principales de nubes: privada (infraestructura propia), pública (como AWS, Azure y Google Cloud) e híbrida (combinación de nube privada y pública para mayor control y flexibilidad).

7. **Características clave de la computación en la nube:** Incluye autoservicio bajo demanda, acceso a través de redes, recurso compartido entre múltiples usuarios (multi-tenancy), escalabilidad automática y un modelo de pago por consumo.

8. **Ahorro de costos:** Se reduce la inversión en infraestructura (CAPEX) y se adopta un modelo de gastos operativos (OPEX), pagando solo por el uso real de los recursos.

9. **Economía de escala:** Al compartir la infraestructura con miles de clientes, los proveedores de nube pueden ofrecer costos reducidos y mejor eficiencia en la gestión de los recursos.

10. **Eliminación de la incertidumbre en la capacidad:** En lugar de estimar y comprar servidores en exceso para manejar el peor escenario, la nube permite escalar de forma dinámica según la demanda real.

11. **Alta disponibilidad y tolerancia a fallos:** Los centros de datos de los proveedores de nube están distribuidos globalmente, asegurando que las aplicaciones permanezcan operativas incluso si un centro de datos falla.

12. **Mayor agilidad y rapidez en el desarrollo:** Las empresas pueden probar, desplegar y escalar aplicaciones en minutos sin necesidad de adquirir y configurar hardware, lo que acelera la innovación y la adaptación al mercado.

**9. The Different Types of Cloud Computing**

1. **Tipos de computación en la nube:** Existen tres modelos principales: Infraestructura como Servicio (IaaS), Plataforma como Servicio (PaaS) y Software como Servicio (SaaS). Cada uno ofrece distintos niveles de gestión y automatización, facilitando la migración desde sistemas tradicionales a la nube.

2. **Infraestructura como Servicio (IaaS):** Proporciona los componentes básicos de TI, como redes, almacenamiento y servidores virtuales. Permite a los usuarios tener flexibilidad para configurar y administrar su entorno. Ejemplo en AWS:
   ```python
   import boto3
   ec2 = boto3.client('ec2')
   response = ec2.run_instances(
       ImageId='ami-12345678',
       InstanceType='t2.micro',
       MinCount=1,
       MaxCount=1
   )
   print("Instancia EC2 creada: ", response['Instances'][0]['InstanceId'])
   ```
   
3. **Plataforma como Servicio (PaaS):** El proveedor de la nube gestiona la infraestructura subyacente, permitiendo a los desarrolladores centrarse en la implementación y gestión de aplicaciones. Ejemplo: AWS Elastic Beanstalk permite desplegar aplicaciones sin preocuparse por servidores.

4. **Software como Servicio (SaaS):** Proporciona aplicaciones completas gestionadas por el proveedor, eliminando la necesidad de mantenimiento y actualización. Ejemplos incluyen Gmail, Dropbox y Zoom, donde los usuarios acceden a servicios sin preocuparse por la infraestructura.

5. **Comparación de modelos:** En un entorno on-premises, la organización gestiona todos los aspectos de la infraestructura. Con IaaS, se delega la administración del hardware y la virtualización. PaaS elimina también la gestión del sistema operativo y runtime, mientras que SaaS externaliza toda la operación de software.

6. **Ejemplos de proveedores de cada modelo:** AWS EC2, Google Cloud y Azure ofrecen IaaS. Para PaaS, existen servicios como Heroku, Google App Engine y AWS Elastic Beanstalk. En SaaS, se encuentran aplicaciones como Google Apps y Salesforce.

7. **Modelo de precios de la nube:** La computación en la nube sigue un esquema de "pago por uso" en tres áreas clave:
   - **Cómputo:** Se paga por el tiempo de ejecución de las instancias.
   - **Almacenamiento:** Se cobra por la cantidad exacta de datos almacenados.
   - **Red:** Solo se paga por los datos que salen de la nube, mientras que la entrada de datos es gratuita.

8. **Ahorro de costos en la nube:** En comparación con la TI tradicional, donde se requiere una inversión inicial en infraestructura (CAPEX), la nube permite convertir estos gastos en costos operativos (OPEX), pagando solo por lo que se usa y reduciendo significativamente el costo total de propiedad.

9. **Ventajas generales de la computación en la nube:** La escalabilidad, flexibilidad y ahorro en costos convierten a la nube en una solución eficiente. Las empresas pueden lanzar productos y servicios globales rápidamente sin preocuparse por la infraestructura subyacente.

**10. AWS Cloud Overview**

1. **Historia de AWS:** AWS fue creado internamente por Amazon en 2002 para externalizar la gestión de TI. En 2004, lanzó SQS y, en 2006, expandió su oferta con S3 y EC2. Desde entonces, ha crecido hasta ser utilizado por empresas como Netflix, Airbnb y la NASA.

2. **Liderazgo en el mercado:** AWS ha sido líder en la computación en la nube durante 13 años consecutivos. En 2024, representa el 31% del mercado con ingresos de $90 mil millones y más de 1 millón de usuarios activos.

3. **Casos de uso:** AWS permite crear aplicaciones escalables utilizadas en diversas industrias. Se puede usar para almacenamiento, big data, hospedaje de sitios web, backends para aplicaciones móviles y servidores de videojuegos.

4. **Infraestructura global:** AWS cuenta con regiones, zonas de disponibilidad, centros de datos y ubicaciones perimetrales distribuidas en el mundo. Estas estructuras permiten ofrecer servicios de baja latencia y alta disponibilidad.

5. **Regiones de AWS:** Cada región es un conjunto de centros de datos ubicados en un área geográfica específica. Ejemplo de nombres de regiones: `us-east-1`, `eu-west-3`, `ap-southeast-2`.

6. **Elección de la región adecuada:** La selección de una región depende de factores como regulaciones de datos, latencia, disponibilidad de servicios y costos. Por ejemplo, datos franceses deben permanecer en Francia según normativas locales.

7. **Zonas de disponibilidad:** Cada región tiene múltiples zonas de disponibilidad, usualmente tres. Estas zonas son centros de datos separados para evitar fallos en cascada en caso de desastres.

8. **Alta disponibilidad y tolerancia a fallos:** Las zonas de disponibilidad están conectadas con redes de baja latencia y alta capacidad, permitiendo redundancia y continuidad del servicio en caso de fallas.

9. **Puntos de presencia y edge locations:** AWS cuenta con más de 400 puntos de presencia en 90 ciudades y 40 países. Esto permite entregar contenido con baja latencia mediante servicios como CloudFront.

10. **Ejemplo de despliegue en AWS:** Crear una instancia EC2 en una región específica usando Python y boto3:
    ```python
    import boto3
    ec2 = boto3.client('ec2', region_name='us-east-1')
    response = ec2.run_instances(
        ImageId='ami-12345678',
        InstanceType='t2.micro',
        MinCount=1,
        MaxCount=1
    )
    print("Instancia EC2 creada: ", response['Instances'][0]['InstanceId'])
    ```

11. **Servicios globales y por región:** Algunos servicios de AWS, como IAM y Route 53, son globales, mientras que otros, como EC2, Lambda y Elastic Beanstalk, están limitados a regiones específicas.

12. **Conectividad entre regiones:** AWS opera una red privada interconectada entre regiones, permitiendo replicación de datos y baja latencia en la comunicación entre servicios distribuidos geográficamente.

13. **Modelo de costos por región:** Los precios de los servicios varían según la región. Es recomendable revisar la tabla de precios de AWS antes de desplegar una aplicación en una región específica.

14. **Preparación para el examen:** Conocer la infraestructura global de AWS, las regiones y los servicios disponibles en cada una es clave para la certificación AWS. Se recomienda revisar la tabla de regiones de AWS y sus servicios.

**11. [Important] AWS Console UI Update**

1. **Actualización de la interfaz de AWS:** La consola de AWS ha cambiado visualmente, presentando un diseño más moderno con botones redondeados y colores más brillantes, en contraste con la versión anterior que tenía tonos más apagados y botones cuadrados.

2. **Consistencia en la usabilidad:** A pesar de los cambios visuales, la funcionalidad y estructura de la consola permanecen intactas. Los usuarios pueden seguir navegando y utilizando los servicios de AWS sin necesidad de reaprender la interfaz.

3. **Diferencias principales entre la UI antigua y la nueva:** La nueva interfaz se caracteriza por una estética más suave y moderna, mientras que la versión anterior presentaba un diseño más cuadrado y colores más apagados. Sin embargo, las ubicaciones de los elementos clave siguen siendo las mismas.

4. **Impacto en los videos y documentación:** Aunque los videos existentes pueden mostrar la interfaz anterior, la información sigue siendo válida. Los cambios son puramente visuales y no afectan la funcionalidad de los servicios de AWS.

5. **Posible reubicación de botones en futuras actualizaciones:** Si AWS introduce cambios más profundos que afecten la ubicación de los botones o las funciones principales, se recomienda revisar la documentación oficial o contactar al instructor para aclaraciones.

6. **Adaptabilidad del usuario:** Los cambios en la UI pueden requerir un breve periodo de adaptación, pero la estructura subyacente se mantiene. Es importante concentrarse en la funcionalidad en lugar del diseño visual.

7. **Compromiso con la actualización de contenido:** Si la interfaz de AWS sufre modificaciones significativas que afectan la navegación, se hará lo posible por actualizar los videos y materiales del curso según sea necesario.

**12. Tour of the Console & Services in AWS**

1. **Selector de regiones:** En la parte superior derecha de la consola de AWS, se encuentra el selector de regiones. Elegir una región cercana geográficamente reduce la latencia. Sin embargo, no es obligatorio estar en la región físicamente para usarla.

2. **Vista de servicios recientes:** En la consola principal, se muestra una lista de servicios visitados recientemente. Esta se actualizará automáticamente a medida que se usen diferentes servicios de AWS.

3. **Información de la cuenta y estado de AWS:** En la parte inferior de la consola, se encuentran detalles sobre el estado de los servicios, problemas de salud en la infraestructura de AWS y el uso y costos de la cuenta.

4. **Navegación por servicios:** AWS permite explorar servicios por orden alfabético o por categoría, como computación, almacenamiento y bases de datos. Esto facilita encontrar rápidamente los recursos necesarios.

5. **Uso de la barra de búsqueda:** Para acceder rápidamente a un servicio específico, la barra de búsqueda permite escribir el nombre del servicio y filtrar resultados, mostrando también blogs y documentación relevante.

6. **Servicios globales y servicios por región:** Algunos servicios, como Route 53, son globales y no requieren selección de región, mientras que otros, como EC2, dependen de la región en la que se desplieguen los recursos.

7. **Diferencias de disponibilidad según la región:** No todos los servicios de AWS están disponibles en todas las regiones. Se recomienda consultar la tabla de servicios por región para verificar la disponibilidad antes de desplegar una solución.

8. **Ejemplo de creación de instancia EC2 en una región específica:**
   ```python
   import boto3
   ec2 = boto3.client('ec2', region_name='us-east-1')
   response = ec2.run_instances(
       ImageId='ami-12345678',
       InstanceType='t2.micro',
       MinCount=1,
       MaxCount=1
   )
   print("Instancia EC2 creada: ", response['Instances'][0]['InstanceId'])
   ```

9. **Consulta de la infraestructura global de AWS:** AWS proporciona información sobre su infraestructura global, permitiendo verificar la conectividad entre regiones y entender la organización de los centros de datos.

10. **Importancia de mantenerse en la misma región durante el curso:** Para evitar confusión en la administración de recursos y garantizar que los servicios estén disponibles, se recomienda seleccionar una región y mantenerla constante durante la formación en AWS.

**13. About the UI changes in the course**

1. **Transición de la interfaz de AWS:** AWS está en un proceso de actualización de su interfaz de usuario, adoptando nuevas guías de diseño. Este cambio ocurre de manera gradual y no todos los servicios se actualizan simultáneamente.

2. **Diferencias entre UI antigua y nueva:** Algunas partes de la consola aún presentan la interfaz antigua, mientras que otras han sido actualizadas a un diseño más moderno y estructurado, lo que puede generar discrepancias entre los videos del curso y la plataforma actual.

3. **Imposibilidad de actualizar constantemente los videos:** Debido a la constante evolución de AWS, mantener cada video del curso completamente actualizado en tiempo real es un desafío. Sin embargo, se monitorean los cambios para asegurarse de que la información siga siendo relevante.

4. **Distinguir cambios menores y mayores:** Si un cambio en la interfaz es solo visual y no afecta la funcionalidad, los videos no se actualizarán de inmediato. Solo en caso de cambios críticos que afecten la comprensión o el uso de un servicio, se realizarán nuevas grabaciones.

5. **Opción de alternar entre UI nueva y antigua:** En muchos servicios de AWS, existe la opción de cambiar entre la nueva y la antigua experiencia de usuario. Si un usuario se siente más cómodo con la UI anterior, puede utilizar esta funcionalidad temporalmente.

6. **El curso no está desactualizado:** A pesar de los cambios visuales en AWS, los conceptos y explicaciones del curso siguen siendo válidos. La estructura de los servicios y sus funcionalidades permanecen intactas, por lo que la formación sigue siendo efectiva.

7. **Compromiso con la actualización de contenido:** Se monitorean diariamente las actualizaciones en AWS. Si un cambio en la UI afecta directamente la enseñanza del curso, se generarán nuevos videos para reflejar estos ajustes.

8. **Enfoque en el aprendizaje del estudiante:** La prioridad del curso es garantizar que los estudiantes comprendan los servicios de AWS y aprueben su examen, más allá de los cambios visuales. Se recomienda enfocarse en los conceptos más que en la apariencia de la consola.

**14. Shared Responsibility Model & AWS Acceptable Policy**

1. **Modelo de Responsabilidad Compartida:** AWS y el usuario tienen responsabilidades distintas en la nube. El usuario es responsable de la seguridad dentro de la nube, mientras que AWS es responsable de la seguridad de la nube, incluyendo infraestructura, hardware y software.

2. **Responsabilidades del usuario:** El cliente debe garantizar la seguridad de los datos, sistemas operativos, configuraciones de red y firewalls. Cualquier configuración incorrecta o acceso no autorizado es su responsabilidad.

3. **Responsabilidades de AWS:** AWS es responsable de mantener segura la infraestructura subyacente, incluyendo la red, servidores y centros de datos. Esto garantiza una base confiable para los clientes.

4. **Importancia en certificaciones AWS:** En el examen de AWS Certified Cloud Practitioner, se evalúa el entendimiento del modelo de responsabilidad compartida. Es crucial diferenciar qué aspectos corresponden al usuario y cuáles a AWS.

5. **Política de Uso Aceptable de AWS:** Al utilizar AWS, se acepta su política de uso, que prohíbe actividades ilegales, contenido ofensivo, violaciones de seguridad y abuso de redes o correos electrónicos.

6. **Ejemplo de responsabilidad del usuario:** Si un usuario almacena datos en S3 sin habilitar el cifrado o permisos adecuados, AWS no se responsabiliza por una brecha de seguridad, ya que la configuración es controlada por el usuario.

7. **Ejemplo de responsabilidad de AWS:** AWS se encarga de la protección física de sus centros de datos, garantizando medidas de seguridad como vigilancia y acceso restringido, lo que exime al usuario de preocuparse por estos aspectos.

8. **Preparación para el curso:** Conociendo el modelo de responsabilidad compartida y la política de uso aceptable, el usuario está mejor preparado para utilizar AWS de manera segura y eficiente, evitando malas configuraciones y prácticas inadecuadas.

