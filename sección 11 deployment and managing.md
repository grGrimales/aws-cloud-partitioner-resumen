# 124. Descripción General de CloudFormation

1. **CloudFormation es una herramienta de infraestructura como código en AWS**, que permite definir y desplegar recursos mediante archivos declarativos.

2. **El enfoque declarativo permite describir el estado deseado de la infraestructura**, sin necesidad de especificar el orden en que los recursos deben ser creados.

3. **Ejemplo de definición en CloudFormation para crear un bucket S3:**
    ```yaml
    Resources:
      MiBucket:
        Type: AWS::S3::Bucket
        Properties:
          BucketName: mi-nombre-de-bucket
    ```

4. **CloudFormation automatiza la creación de recursos en el orden correcto**, resolviendo dependencias como relaciones entre instancias EC2 y grupos de seguridad.

5. **La infraestructura como código facilita el control de versiones**, permitiendo revisar y auditar cambios mediante procesos como el code review.

6. **Cada recurso creado por un template CloudFormation recibe una etiqueta común**, lo que facilita la estimación de costos y la organización de recursos.

7. **CloudFormation permite estrategias de ahorro de costos**, como automatizar la eliminación de stacks fuera del horario laboral y su recreación al día siguiente.

8. **Es posible destruir y recrear infraestructura rápidamente**, aumentando la productividad y reduciendo el tiempo de implementación de entornos.

9. **CloudFormation puede generar diagramas de arquitectura automáticamente**, ayudando a comprender la relación entre los distintos componentes de una aplicación.

10. **La declaración de los recursos en templates evita errores de dependencia**, ya que CloudFormation entiende el orden de creación necesario.

11. **Se pueden reutilizar templates disponibles en la comunidad y documentación oficial**, acelerando la creación de infraestructuras complejas.

12. **Casi todos los servicios de AWS son compatibles con CloudFormation**, lo que garantiza una cobertura amplia para construir cualquier arquitectura.

13. **Cuando un recurso no está soportado nativamente, se pueden usar recursos personalizados**, conocidos como Custom Resources, para integrarlos al stack.

14. **CloudFormation es ideal para replicar arquitecturas en distintas cuentas, regiones o entornos**, asegurando consistencia y eficiencia en la implementación.

15. **El servicio Infrastructure Composer permite visualizar los recursos y relaciones de un template**, facilitando el entendimiento de arquitecturas complejas.

16. **CloudFormation es fundamental en ambientes de DevOps y automatización**, sirviendo como la base para la gestión de infraestructura escalable y repetible en AWS.

# 125. CloudFormation Hands On

1. **CloudFormation permite crear recursos en AWS mediante archivos de plantilla**, lo que representa el principio de *Infrastructure as Code* (IaC), facilitando la gestión declarativa de la infraestructura.

2. **Las plantillas pueden estar escritas en YAML o JSON**, y definen recursos como instancias EC2, IPs elásticas, grupos de seguridad, entre otros, permitiendo su creación automática y organizada.

3. **La región es importante al ejecutar plantillas**, ya que recursos como las AMI (Amazon Machine Images) son específicos por región. Por eso, la clase utiliza *US East 1* (Norte de Virginia).

4. **El primer archivo usado, `0-just-EC2.yaml`, define una instancia EC2 mínima**, con las propiedades: tipo `t2.micro`, zona de disponibilidad `us-east-1a` y una imagen AMI específica.

5. **Ejemplo del archivo `0-just-EC2.yaml`**:

   ```yaml
   Resources:
     MyInstance:
       Type: "AWS::EC2::Instance"
       Properties:
         AvailabilityZone: us-east-1a
         InstanceType: t2.micro
         ImageId: ami-xxxxxxxx
   ```

6. **Se crea una pila (stack) desde CloudFormation**, seleccionando la plantilla a cargar y proporcionando un nombre como `demo-CloudFormation`.

7. **CloudFormation permite añadir etiquetas (tags)** para clasificar los recursos, como el tag personalizado `CFDemo`, aplicado automáticamente a los recursos creados.

8. **El recurso EC2 se crea correctamente**, y al inspeccionar en la consola EC2 se confirma que tiene el tipo `t2.micro` y la imagen AMI definidos en la plantilla.

9. **Application Composer brinda una vista visual del template**, permitiendo analizar los recursos y sus relaciones de forma gráfica, lo que facilita la comprensión de la arquitectura.

10. **CloudFormation aplica etiquetas adicionales automáticamente**, como el nombre lógico del recurso, el nombre de la pila y el ID del stack.

11. **La plantilla actualizada, `1-ec2-with-sg-eip.yaml`, añade más componentes**, incluyendo un Elastic IP, parámetros y múltiples grupos de seguridad.

12. **El nuevo archivo contiene una sección `Parameters`**, que permite personalizar partes del stack, como la descripción del grupo de seguridad.

    ```yaml
    Parameters:
      SGDescription:
        Type: String
        Default: "My security group"
    ```

13. **La instancia EC2 se asocia con dos grupos de seguridad**, uno para permitir conexiones SSH desde una IP específica y otro para habilitar tráfico HTTP público en el puerto 80.

14. **Se define un recurso de Elastic IP (`AWS::EC2::EIP`)**, que se vincula a la instancia EC2, asegurando que tenga una dirección IP estática accesible desde Internet.

15. **Ejemplo de recurso de Elastic IP en YAML**:

    ```yaml
    MyEIP:
      Type: AWS::EC2::EIP
      Properties:
        InstanceId: !Ref MyInstance
    ```

16. **Al actualizar la pila, se genera un *change set***, que permite previsualizar qué recursos serán añadidos, modificados o eliminados antes de aplicar los cambios.

17. **CloudFormation detecta que la instancia EC2 requiere reemplazo**, por lo que crea una nueva y luego elimina la anterior, asegurando una transición limpia.

18. **Durante el proceso de actualización, se observan dos instancias en EC2**, una en ejecución y otra en creación, lo que permite una actualización sin interrupción de servicio.

19. **La IP elástica es creada y asignada automáticamente a la nueva instancia**, lo que puede comprobarse en la sección de *networking* del recurso EC2 en la consola.

20. **CloudFormation se encarga de la limpieza automática**, eliminando los recursos antiguos una vez que los nuevos están correctamente configurados.

21. **Nunca se recomienda modificar manualmente los recursos creados por CloudFormation**, ya que esto puede generar inconsistencias; los cambios deben hacerse exclusivamente mediante actualizaciones de la plantilla o eliminando el stack.

# 126. CDK Overview

1. **CDK (Cloud Development Kit) permite definir infraestructura en AWS usando lenguajes de programación comunes**, como Python, TypeScript, JavaScript, Java o .NET, en lugar de escribir directamente en YAML o JSON como en CloudFormation.

2. **El CDK traduce el código fuente a plantillas de CloudFormation**, lo que significa que al final sigue usando CloudFormation para desplegar los recursos, pero de forma automatizada y más amigable para desarrolladores.

3. **Al usar lenguajes como Python o TypeScript, se gana acceso a características como estructuras condicionales, bucles, funciones reutilizables y tipado**, lo cual hace que la infraestructura sea más fácil de mantener y escalar.

4. **Un caso de uso ideal del CDK es cuando se trabaja con AWS Lambda**, ya que permite definir tanto la función Lambda como su infraestructura circundante en el mismo lenguaje y proyecto.

5. **Ejemplo básico en TypeScript con CDK** para definir una VPC, un cluster ECS, un Application Load Balancer y un servicio Fargate:

   ```ts
   const vpc = new ec2.Vpc(this, 'MyVpc', { maxAzs: 3 });
   const cluster = new ecs.Cluster(this, 'MyCluster', { vpc });
   const fargateService = new ecsPatterns.ApplicationLoadBalancedFargateService(this, 'MyService', {
     cluster,
     taskImageOptions: { image: ecs.ContainerImage.fromRegistry("amazon/amazon-ecs-sample") }
   });
   ```

6. **El comando `cdk synth` genera la plantilla CloudFormation** en JSON o YAML a partir del código fuente del proyecto CDK, permitiendo visualizar cómo será la infraestructura antes del despliegue.

7. **El comando `cdk deploy` aplica los cambios y despliega la infraestructura en AWS**, utilizando internamente CloudFormation para administrar el ciclo de vida del stack.

8. **CDK facilita el uso de *constructs*, que son bloques reutilizables de infraestructura**, agrupando múltiples recursos en una sola unidad lógica.

9. **Al desarrollar infraestructura con CDK se puede mantener la lógica de negocio y la infraestructura en un mismo repositorio**, lo que mejora la colaboración y reduce la duplicación de código.

10. **El CDK CLI proporciona herramientas para inicializar proyectos, compilar, probar y desplegar stacks**, facilitando el flujo completo de desarrollo y operación.

11. **La infraestructura definida con CDK es declarativa en el resultado pero imperativa en la definición**, combinando lo mejor de ambos enfoques y permitiendo lógica más compleja que sería engorrosa en YAML.

12. **El uso de bucles en CDK permite generar múltiples recursos dinámicamente**, como varias instancias EC2 o buckets de S3, sin duplicar código.

13. **Al utilizar tipado fuerte, se pueden prevenir errores en tiempo de desarrollo**, como pasar valores incorrectos o usar propiedades no válidas, algo que CloudFormation en YAML no puede detectar hasta el despliegue.

14. **CDK mejora la productividad del desarrollador**, al permitir construir infraestructura con las herramientas, lenguajes y patrones a los que ya está acostumbrado, acelerando el aprendizaje y reduciendo errores.


# 127. Beanstalk Overview

1. **Elastic Beanstalk es un servicio orientado a desarrolladores que permite desplegar aplicaciones en AWS sin gestionar directamente la infraestructura**, ofreciendo una experiencia simplificada y centrada en el código.

2. **La arquitectura típica soportada por Beanstalk sigue el modelo de tres capas (3-tier architecture)**, donde un balanceador de carga distribuye el tráfico a varias instancias EC2 que se comunican con bases de datos como Amazon RDS y opcionalmente con ElastiCache.

3. **Beanstalk abstrae los componentes subyacentes como EC2, Auto Scaling Groups, Elastic Load Balancer y RDS**, permitiendo al desarrollador enfocarse en el despliegue y funcionamiento de su aplicación sin preocuparse por detalles de infraestructura.

4. **Este servicio se clasifica como PaaS (Platform as a Service)**, en contraste con otros servicios de AWS como EC2, que pertenecen al modelo IaaS (Infrastructure as a Service).

5. **Elastic Beanstalk permite desplegar aplicaciones en múltiples entornos y con múltiples configuraciones**, como desarrollo, prueba y producción, manteniendo consistencia y control en cada uno.

6. **El uso de Elastic Beanstalk es gratuito**, pero se incurre en costos por los recursos subyacentes utilizados, como instancias EC2, balanceadores de carga y almacenamiento.

7. **Todo el aprovisionamiento de capacidad, escalado y balanceo de carga es manejado automáticamente por Beanstalk**, lo que ahorra tiempo y reduce errores de configuración.

8. **El monitoreo de salud de la aplicación está incorporado en el servicio**, con métricas visibles desde el panel de Elastic Beanstalk y reportes enviados automáticamente a CloudWatch.

9. **Cada instancia EC2 gestionada por Beanstalk contiene un agente de salud**, encargado de evaluar el estado de la aplicación y publicar eventos relacionados, facilitando la respuesta rápida ante errores.

10. **Beanstalk ofrece tres modelos de arquitectura para desplegar aplicaciones**, incluyendo: despliegue en una sola instancia (ideal para desarrollo), despliegue con balanceador y ASG (para producción), y despliegue de trabajadores sin balanceador.

11. **El despliegue en una sola instancia se recomienda solo para pruebas o entornos de desarrollo**, ya que no ofrece redundancia ni alta disponibilidad.

12. **Para aplicaciones web en producción, se recomienda el modelo con balanceador de carga y Auto Scaling Group**, asegurando tolerancia a fallos y escalabilidad automática.

13. **Beanstalk soporta múltiples plataformas de desarrollo**, entre ellas Go, Java, .NET, Node.js, PHP, Python, Ruby, Docker (incluyendo variantes Multi Docker y Preconfigured Docker), y más.

14. **El despliegue con Docker en Beanstalk permite empaquetar aplicaciones y sus dependencias**, lo que mejora la portabilidad y consistencia del entorno de ejecución.

15. **A pesar de la abstracción, Beanstalk permite acceder y modificar configuraciones de los recursos subyacentes**, brindando flexibilidad para casos que requieran personalización.

16. **La simplicidad de Beanstalk lo convierte en una opción ideal para desarrolladores que desean enfocarse en escribir código y no en administrar infraestructura**, proporcionando una entrada accesible al mundo de la nube con buenas prácticas integradas.


# 128. Beanstalk Hands On

1. **Elastic Beanstalk permite crear aplicaciones a través de entornos tipo Web Server o Worker**, siendo el entorno de servidor web el más adecuado para desplegar sitios web y el de Worker para procesar tareas asincrónicas desde colas.

2. **Cada aplicación en Beanstalk se organiza por entornos**, como `MyApplication-dev` para desarrollo o `MyApplication-prod` para producción, y cada entorno tiene su propia configuración e infraestructura.

3. **El nombre de dominio es generado automáticamente por Beanstalk**, permitiendo acceder a la aplicación sin necesidad de configurar manualmente DNS o certificados iniciales.

4. **Es posible seleccionar una plataforma gestionada por AWS para la aplicación**, como Node.js, Java, Python, .NET o Docker, y se recomienda usar las versiones más recientes disponibles.

5. **Se puede comenzar con una aplicación de ejemplo**, lo que facilita el despliegue inicial sin necesidad de subir código personalizado, ideal para pruebas o familiarización con la herramienta.

6. **Beanstalk ofrece varios *presets* de configuración**, incluyendo instancia única (elegible para capa gratuita), alta disponibilidad con balanceador de carga y configuración personalizada.

7. **La instancia única es ideal para entornos de desarrollo**, ya que minimiza costos y simplifica la arquitectura, aunque no es adecuada para producción por falta de redundancia.

8. **Beanstalk necesita permisos mediante roles de IAM para operar**, como crear instancias, configurar redes o adjuntar direcciones IP, y estos roles deben ser configurados manualmente si no se generan automáticamente.

9. **Para el perfil EC2, se debe crear un rol IAM con políticas `AWSElasticBeanstalkWebTier`, `AWSElasticBeanstalkWorkerTier` y `AWSElasticBeanstalkMulticontainerDocker`**, y nombrarlo como `aws-elasticbeanstalk-ec2-role`.

10. **Una vez configurados los roles, se puede proceder a la creación del entorno**, omitiendo configuraciones avanzadas como redes, base de datos o escalado, si se usa la opción de revisión rápida.

11. **Elastic Beanstalk utiliza CloudFormation en segundo plano para crear los recursos**, como grupos de Auto Scaling, configuraciones de lanzamiento, direcciones IP elásticas y grupos de seguridad.

12. **Desde la consola de CloudFormation se puede observar el progreso de la creación**, con estados como `CREATE_IN_PROGRESS` y `CREATE_COMPLETE`, y visualizar la plantilla generada automáticamente.

13. **Application Composer permite ver gráficamente los recursos creados por CloudFormation**, como `LaunchConfiguration`, `SecurityGroup`, `ElasticIP`, `WaitCondition` y otros recursos internos.

14. **La instancia EC2 se lanza con un tipo como `t3.micro` y recibe una IP pública**, que se puede verificar directamente desde la consola EC2, así como su asociación con el grupo de Auto Scaling.

15. **Al finalizar la creación del entorno, se recibe un dominio funcional**, donde se despliega la aplicación de ejemplo con un mensaje de éxito: "Congratulations, you are now running Elastic Beanstalk".

16. **Es posible subir nuevas versiones del código desde la interfaz**, permitiendo despliegues continuos o actualizaciones de la aplicación sin necesidad de modificar manualmente las instancias.

17. **La pestaña de `Health` proporciona información sobre el estado de las instancias**, incluyendo revisiones automáticas de salud y disponibilidad.

18. **El panel de `Logs` permite visualizar registros de la aplicación directamente desde la consola**, lo cual facilita la depuración de errores y análisis de comportamiento sin acceder manualmente al sistema operativo.

19. **Desde `Monitoring` se pueden observar métricas detalladas**, como uso de CPU, tráfico de red y otros datos relevantes para el desempeño de la aplicación, utilizando CloudWatch como backend.

20. **Cada entorno es completamente aislado**, lo que permite crear fácilmente múltiples versiones de la misma aplicación, por ejemplo, un entorno de pruebas (`-dev`) y otro de producción (`-prod`), sin interferencias.

21. **Una vez finalizado el uso de la aplicación, es recomendable eliminarla para evitar costos**, lo cual se puede hacer desde la consola de Beanstalk usando la opción `Delete application`. Esto asegura también que todos los recursos subyacentes sean limpiados.

# 129. CodeDeploy Overview

1. **CodeDeploy es un servicio de AWS que automatiza el despliegue de aplicaciones** en diferentes tipos de servidores, permitiendo pasar de una versión a otra sin intervención manual y con alta precisión.

2. **A diferencia de Beanstalk o CloudFormation, CodeDeploy no depende de otras herramientas** para funcionar, lo que lo convierte en una solución más flexible e independiente para implementar actualizaciones.

3. **El objetivo principal de CodeDeploy es gestionar la transición de versiones de una aplicación**, por ejemplo, actualizar automáticamente desde una versión V1 a una versión V2 en múltiples servidores.

4. **CodeDeploy es compatible tanto con instancias EC2 como con servidores locales (on-premises)**, lo que lo convierte en un servicio híbrido muy útil para empresas en procesos de migración hacia la nube.

5. **El carácter híbrido de CodeDeploy permite estandarizar la forma en que se hacen los despliegues**, utilizando la misma lógica y herramientas tanto en servidores físicos como en entornos virtuales en AWS.

6. **Para usar CodeDeploy, es necesario que los servidores estén previamente aprovisionados**, es decir, tú debes encargarte de tenerlos listos antes de ejecutar un despliegue.

7. **Cada servidor que formará parte del proceso debe tener instalado el agente de CodeDeploy**, el cual se encarga de recibir las instrucciones y ejecutar los cambios necesarios en el entorno.

8. **El agente de CodeDeploy permite controlar la instalación, validación, reinicio de servicios y otros pasos del ciclo de despliegue**, siguiendo los scripts definidos por el usuario.

9. **La configuración del despliegue se basa en un archivo `appspec.yml`**, que define fases como `BeforeInstall`, `Install`, `AfterInstall`, `ApplicationStart`, y `ValidateService`.

   ```yaml
   version: 0.0
   os: linux
   files:
     - source: /
       destination: /home/ec2-user/myapp
   hooks:
     BeforeInstall:
       - location: scripts/before_install.sh
     ApplicationStart:
       - location: scripts/start_server.sh
   ```

10. **CodeDeploy permite controlar la forma en que se realiza el despliegue en múltiples instancias**, con estrategias como *in-place* (sobrescribir en el mismo servidor) o *blue/green* (crear nuevos servidores y redirigir el tráfico).

11. **El servicio proporciona un único punto de control para el despliegue**, lo cual facilita el monitoreo del progreso, validación del estado y detección de errores en la actualización.

12. **CodeDeploy es ideal para organizaciones que aún dependen de infraestructura local**, ya que ofrece una ruta gradual y ordenada hacia la nube sin tener que cambiar la forma en que gestionan sus versiones.

13. **Aunque es un servicio avanzado que no fue demostrado en la clase**, es importante conocer su existencia y su papel como puente entre lo tradicional y lo moderno en los procesos de despliegue de software.


# 132. CodeBuild Overview

1. **CodeBuild es un servicio totalmente administrado de AWS que permite compilar código en la nube**, automatizando la creación de artefactos listos para ser desplegados en entornos de producción o pruebas.

2. **El flujo básico de trabajo con CodeBuild parte del código fuente ubicado en CodeCommit**, aunque también puede integrarse con GitHub o Bitbucket, para obtener el código que será construido.

3. **CodeBuild ejecuta scripts definidos por el desarrollador** en un archivo llamado `buildspec.yml`, donde se detallan fases como instalación, preconstrucción, construcción y postconstrucción.

   ```yaml
   version: 0.2
   phases:
     install:
       commands:
         - echo Installing dependencies...
     build:
       commands:
         - echo Building the project...
         - npm run build
   artifacts:
     files:
       - '**/*'
   ```

4. **El producto final del proceso de construcción son artefactos**, como archivos `.zip`, imágenes Docker, o binarios, que luego pueden ser usados por servicios como CodeDeploy o almacenados en S3.

5. **CodeBuild es completamente serverless**, lo que significa que no requiere configurar ni mantener servidores o entornos de ejecución: AWS gestiona todo el entorno de compilación.

6. **La escalabilidad automática está integrada en CodeBuild**, lo que le permite adaptarse al volumen de construcciones sin necesidad de intervención del usuario, siendo ideal para flujos CI/CD dinámicos.

7. **El servicio es altamente disponible y seguro**, ya que se ejecuta dentro de la infraestructura global de AWS y permite definir roles de IAM para controlar el acceso y permisos de ejecución.

8. **La facturación se realiza bajo un modelo de pago por uso**, cobrando únicamente por el tiempo que se tarda en ejecutar la compilación, lo que lo convierte en una solución económica para pequeños y grandes equipos.

9. **Al integrarse con CodeCommit, CodePipeline y CodeDeploy, CodeBuild se convierte en una parte esencial del ciclo de DevOps en AWS**, permitiendo automatizar desde el commit hasta el despliegue.

10. **Cada ejecución de CodeBuild puede usar imágenes Docker personalizadas o predefinidas por AWS**, lo que facilita adaptar el entorno de compilación a las necesidades específicas del proyecto.

11. **CodeBuild permite realizar pruebas automatizadas como parte del proceso de construcción**, asegurando la calidad del código antes de que sea desplegado.

12. **La generación de logs detallados durante el proceso ayuda a identificar errores en la compilación o en los scripts**, y estos registros pueden visualizarse desde la consola o enviarse a CloudWatch.

13. **Al automatizar la etapa de construcción, CodeBuild permite que los desarrolladores se concentren en escribir código**, delegando en AWS la responsabilidad de generar los paquetes listos para producción de forma segura y eficiente.

# 133. CodePipeline Overview

1. **CodePipeline es el servicio de AWS encargado de orquestar el flujo completo de integración y entrega continua (CI/CD)**, conectando servicios como CodeCommit, CodeBuild y CodeDeploy para automatizar todo el ciclo de vida del software.

2. **Una pipeline permite definir una serie de etapas secuenciales**, donde cada etapa representa una acción como obtener el código fuente, construirlo, probarlo, desplegarlo o ejecutar validaciones adicionales.

3. **El objetivo de CodePipeline es automatizar desde que el código es subido al repositorio hasta que es desplegado en producción**, permitiendo así una entrega rápida y confiable de aplicaciones.

4. **El concepto de CI/CD (Integración Continua y Entrega Continua)** implica que cada vez que un desarrollador hace un *push* de código, se inicia un proceso automático de compilación, pruebas y despliegue.

5. **Una pipeline típica puede tener esta estructura**: obtener el código desde CodeCommit, compilarlo con CodeBuild, desplegarlo con CodeDeploy y luego enviarlo a un entorno como Elastic Beanstalk.

6. **CodePipeline es totalmente gestionado por AWS**, lo que elimina la necesidad de configurar servidores o herramientas externas para mantener el proceso de entrega automatizado.

7. **Es compatible con servicios internos de AWS como CodeCommit, CodeBuild, CodeDeploy, Elastic Beanstalk y CloudFormation**, permitiendo una integración fluida dentro del ecosistema.

8. **También admite herramientas de terceros como GitHub, Bitbucket, Jenkins y plugins personalizados**, lo que brinda flexibilidad a equipos que ya tienen herramientas establecidas.

9. **Los pipelines pueden personalizarse con condiciones, aprobaciones manuales, y acciones paralelas**, permitiendo construir flujos sofisticados que reflejen la realidad de un entorno empresarial.

10. **Cada etapa dentro de CodePipeline es definida por artefactos y acciones**, y se puede monitorear visualmente el progreso desde la consola, permitiendo identificar errores rápidamente.

11. **Ejemplo de un pipeline simple en formato declarativo usando CloudFormation**:

   ```yaml
   Resources:
     MyPipeline:
       Type: AWS::CodePipeline::Pipeline
       Properties:
         RoleArn: arn:aws:iam::123456789012:role/CodePipelineServiceRole
         Stages:
           - Name: Source
             Actions:
               - Name: SourceAction
                 ActionTypeId:
                   Category: Source
                   Owner: AWS
                   Provider: CodeCommit
                   Version: 1
                 OutputArtifacts:
                   - Name: SourceOutput
         ArtifactStore:
           Type: S3
           Location: my-artifact-bucket
   ```

12. **El uso de CodePipeline acelera el ciclo de entrega de software**, permitiendo que las aplicaciones lleguen a producción más rápido y con mayor control sobre calidad y seguridad.

13. **En contextos de examen o entrevistas técnicas, cuando se mencione la orquestación de procesos CI/CD en AWS**, la respuesta correcta y directa es CodePipeline.

# 134. CodeArtifact Overview

1. **CodeArtifact es un servicio de AWS diseñado para gestionar artefactos y dependencias de software**, eliminando la necesidad de configurar infraestructura propia como servidores o buckets S3 para este fin.

2. **El desarrollo moderno de software se basa en arquitecturas de paquetes interdependientes**, lo que requiere un sistema robusto para almacenar, compartir y recuperar estas dependencias de manera eficiente.

3. **Tradicionalmente, los equipos debían implementar su propio sistema de gestión de artefactos**, lo cual implicaba configuraciones complejas y mantenimiento constante sobre EC2 o S3.

4. **AWS CodeArtifact proporciona una solución administrada, segura, escalable y rentable**, diseñada específicamente para el almacenamiento y gestión de dependencias en proyectos de desarrollo.

5. **CodeArtifact es compatible con las principales herramientas de gestión de paquetes**, como Maven y Gradle para Java, npm y yarn para JavaScript, pip y twine para Python, y NuGet para .NET.

6. **Los desarrolladores pueden publicar y obtener dependencias directamente desde CodeArtifact**, utilizando comandos nativos de sus herramientas, sin necesidad de adaptaciones o configuraciones complejas.

7. **La integración con CodeBuild permite que las dependencias necesarias para construir el proyecto se obtengan automáticamente desde CodeArtifact**, facilitando un flujo continuo de integración.

8. **Ejemplo de configuración con npm para utilizar CodeArtifact**:

   ```bash
   aws codeartifact login --tool npm --repository my-repo --domain my-domain
   npm install lodash
   ```

9. **CodeArtifact puede actuar como proxy para repositorios públicos como npmjs o pypi**, lo que permite cachear dependencias externas y reducir la latencia y el riesgo de fallos por terceros.

10. **El control de acceso a los repositorios se gestiona mediante IAM**, garantizando que solo los usuarios y servicios autorizados puedan subir o descargar artefactos.

11. **Los artefactos pueden versionarse y organizarse por dominios y repositorios**, lo cual permite estructurar los recursos de forma lógica y segmentada para diferentes proyectos o equipos.

12. **Desde la perspectiva de examen, CodeArtifact es la respuesta correcta cuando se menciona un sistema de almacenamiento y recuperación de dependencias**, especialmente si se requiere una solución nativa de AWS.

13. **Al centralizar la gestión de dependencias, CodeArtifact promueve buenas prácticas de seguridad, eficiencia en los builds y reducción de errores**, siendo una herramienta clave en entornos de CI/CD.

# 135. Systems Manager (SSM) Overview

1. **AWS Systems Manager (SSM) es un servicio híbrido que permite gestionar tanto instancias EC2 como servidores On-Premises**, lo que lo convierte en una herramienta centralizada para administrar infraestructura distribuida.

2. **SSM proporciona información operativa del estado de los sistemas**, ayudando a monitorear y mantener la salud de los servidores sin necesidad de herramientas externas adicionales.

3. **Una de sus funciones clave es el parchado automático de instancias y servidores**, mejorando la seguridad y cumplimiento normativo de forma automatizada y programable.

4. **SSM permite ejecutar comandos directamente en múltiples servidores desde la consola o mediante scripts**, lo cual elimina la necesidad de iniciar sesión en cada máquina individualmente.

5. **SSM Parameter Store es una funcionalidad destacada dentro del servicio**, que permite almacenar parámetros de configuración, secretos y variables de entorno de forma segura y versionada.

6. **El servicio es compatible con una amplia variedad de sistemas operativos**, incluyendo Linux, Windows, macOS y hasta Raspberry Pi, lo que lo hace extremadamente versátil.

7. **El agente de SSM debe estar instalado en cada sistema gestionado**, ya que este agente es el que permite la comunicación entre el servidor local y el servicio SSM en AWS.

8. **Amazon Linux AMI y algunas AMIs de Ubuntu ya incluyen el agente de SSM por defecto**, facilitando su adopción inmediata en nuevos entornos desplegados en AWS.

9. **La instalación del agente en servidores On-Premises convierte a SSM en una solución híbrida real**, ya que se puede tener control remoto y centralizado tanto de servidores locales como en la nube.

10. **El agente de SSM corre en segundo plano y se comunica constantemente con el servicio SSM**, reportando estado, ejecutando tareas y aplicando configuraciones enviadas desde AWS.

11. **En caso de que una instancia no responda a SSM, usualmente se debe a problemas con el agente**, como una instalación incompleta, versión desactualizada o falta de permisos adecuados.

12. **Una vez que el agente está activo, se pueden aplicar acciones masivas como parches, comandos o configuraciones de forma uniforme en todos los sistemas registrados**, simplificando la administración a gran escala.

13. **SSM es la opción correcta cuando se busca automatizar tareas operativas sobre múltiples instancias EC2 o servidores On-Premises**, especialmente aquellas relacionadas con seguridad, configuración y ejecución de scripts.

14. **SSM incluye más de 10 productos integrados bajo una sola consola**, aunque para propósitos de examen es suficiente conocer los más relevantes como Patch Manager, Run Command y Parameter Store.

15. **Desde una perspectiva de certificación, SSM debe asociarse mentalmente con conceptos como parchado masivo, ejecución remota de comandos y gestión centralizada de parámetros y configuraciones**.


# 136. SSM Session Manager

1. **SSM Session Manager permite iniciar una terminal segura en instancias EC2 o servidores On-Premises sin necesidad de SSH**, eliminando la necesidad de usar el puerto 22 o claves SSH.

2. **El uso de Session Manager mejora la seguridad**, ya que se puede cerrar completamente el puerto 22, reduciendo así la superficie de ataque de las instancias.

3. **La comunicación se realiza a través del agente de SSM instalado en la instancia**, que se conecta con el servicio de Systems Manager, permitiendo la ejecución de comandos sin conexiones externas directas.

4. **Este método es compatible con sistemas Linux, Windows y macOS**, ofreciendo flexibilidad para distintos entornos operativos.

5. **Los registros de las sesiones pueden enviarse automáticamente a Amazon S3 o CloudWatch Logs**, lo que garantiza trazabilidad y cumplimiento de auditorías.

6. **Para utilizar Session Manager, la instancia EC2 necesita un perfil de instancia IAM**, que incluya permisos para comunicarse con Systems Manager.

7. **La política adecuada es `AmazonSSMManagedInstanceCore`**, la cual permite al agente interactuar con el servicio SSM y recibir comandos.

8. **No es necesario asociar un par de llaves SSH al lanzar la instancia**, ya que todo el acceso se realizará por medio del agente SSM.

9. **El grupo de seguridad puede estar completamente cerrado**, sin reglas de entrada, lo que permite ejecutar la instancia sin exposición pública.

10. **Una vez la instancia está en ejecución y tiene la política IAM adecuada**, aparece como "nodo administrado" en la consola de Fleet Manager de Systems Manager.

11. **Desde Session Manager se puede iniciar una sesión shell con un clic**, sin necesidad de terminal local ni de configuración de red compleja.

12. **Los comandos dentro de la terminal SSM funcionan como en cualquier shell**, por ejemplo, se puede usar `ping`, `hostname`, `ls`, `cd`, y otros comandos estándar de Linux o Windows.

13. **La dirección IP privada de la instancia se refleja en el entorno del shell**, confirmando que el acceso es directo a través del canal privado y seguro de SSM.

14. **Session Manager se convierte así en una tercera alternativa para conectarse a EC2**, junto con SSH tradicional y EC2 Instance Connect, siendo la única que no requiere el puerto 22 abierto.

15. **EC2 Instance Connect sigue requiriendo abrir el puerto 22**, aunque no necesite claves previamente generadas, ya que AWS sube claves temporales para la sesión.

16. **Session Manager se considera el método más seguro**, ya que no requiere reglas de acceso externas, y puede auditarse completamente con registros guardados.

17. **La sesión puede cerrarse manualmente desde la consola**, y el historial de la sesión queda registrado con detalles como duración, usuario y comandos ejecutados.

18. **Al finalizar, se recomienda terminar la instancia EC2 utilizada**, para evitar costos innecesarios y mantener el entorno limpio.

# 137. SSM Parameter Store

1. **SSM Parameter Store es un servicio serverless que permite almacenar configuraciones y secretos de manera centralizada y segura**, eliminando la necesidad de gestionar servidores o bases de datos para este propósito.

2. **Es ideal para guardar valores como claves API, contraseñas, rutas de configuración y parámetros de aplicaciones**, lo que permite separar la lógica del código de los valores sensibles.

3. **El acceso a cada parámetro se gestiona mediante políticas IAM**, lo que garantiza que solo usuarios o servicios autorizados puedan leer o modificar ciertos parámetros.

4. **Se pueden almacenar valores en texto plano o como cadenas seguras (SecureString)**, las cuales se cifran automáticamente utilizando AWS KMS (Key Management Service).

5. **Los parámetros pueden ser versionados**, lo que facilita rastrear cambios a lo largo del tiempo y permite revertir a versiones anteriores si es necesario.

6. **El servicio es altamente escalable y duradero**, capaz de manejar múltiples solicitudes simultáneas sin pérdida de rendimiento ni disponibilidad.

7. **Existen dos niveles de uso: Standard y Advanced**, siendo el nivel Standard gratuito y suficiente para la mayoría de los casos de uso, mientras que el nivel Advanced ofrece funcionalidades adicionales como límites extendidos y etiquetas.

8. **Los tipos de parámetros disponibles incluyen `String`, `StringList` y `SecureString`**, cada uno con aplicaciones distintas según el tipo de dato que se desee guardar.

9. **Ejemplo de parámetro tipo String simple**:

   ```json
   {
     "Name": "/app/config/endpoint",
     "Type": "String",
     "Value": "https://api.miapp.com"
   }
   ```

10. **Ejemplo de parámetro tipo SecureString cifrado con KMS**:

   ```json
   {
     "Name": "/app/secret/apiKey",
     "Type": "SecureString",
     "KeyId": "alias/aws/ssm",
     "Value": "MI-API-KEY-SECRETA"
   }
   ```

11. **Los parámetros pueden ser recuperados desde scripts, instancias EC2, Lambda o contenedores**, haciendo uso del SDK de AWS o comandos de la CLI, como:

   ```bash
   aws ssm get-parameter --name "/app/config/endpoint" --with-decryption
   ```

12. **Parameter Store permite establecer etiquetas (tags) para organizar y categorizar parámetros**, especialmente útil en ambientes con múltiples aplicaciones o entornos (dev, test, prod).

13. **Cada parámetro tiene un historial de versiones**, visible en la consola, lo cual facilita entender cuándo y cómo han cambiado ciertos valores críticos.

14. **La interfaz es sencilla y permite crear, editar o eliminar parámetros en pocos clics**, lo cual lo convierte en una solución práctica tanto para usuarios técnicos como para administradores de sistemas.

15. **Desde una perspectiva de arquitectura, Parameter Store promueve buenas prácticas de seguridad y mantenimiento**, separando claramente los secretos del código y centralizando la gestión de configuraciones.

# 138. Deployment Summary

1. **CloudFormation es la herramienta de AWS para definir infraestructura como código (IaC)**, permitiendo crear plantillas reutilizables para aprovisionar recursos de manera consistente en distintas regiones y cuentas.

2. **Estas plantillas de CloudFormation son compatibles con casi todos los servicios de AWS**, lo que permite una automatización completa del entorno de infraestructura sin intervención manual.

3. **Elastic Beanstalk es una plataforma como servicio (PaaS) que facilita el despliegue de aplicaciones web**, utilizando configuraciones conocidas como balanceadores de carga, instancias EC2 y bases de datos RDS.

4. **Beanstalk está limitado a ciertos lenguajes de programación y Docker**, pero permite una experiencia simplificada y estandarizada para desarrolladores que solo desean subir su código y ejecutar.

5. **CodeDeploy es un servicio híbrido que permite desplegar y actualizar aplicaciones en servidores EC2 o infraestructura On-Premises**, ideal para equipos que tienen entornos mixtos.

6. **Systems Manager también es un servicio híbrido**, usado para aplicar parches, ejecutar comandos y configurar servidores a gran escala, tanto dentro de AWS como en instalaciones locales.

7. **CodeCommit es el servicio de control de versiones de AWS basado en Git**, que permite a los equipos almacenar su código en repositorios privados con gestión de accesos y seguridad integrada.

8. **CodeBuild permite compilar, probar y generar artefactos en AWS de forma serverless**, eliminando la necesidad de gestionar servidores de construcción.

9. **CodeDeploy, además de su uso en despliegue general, se considera también parte del conjunto de herramientas para desarrolladores**, por su integración natural en los flujos CI/CD.

10. **CodePipeline orquesta las distintas etapas del ciclo de vida del software**, desde el commit del código hasta el despliegue, incluyendo pasos intermedios como pruebas, validaciones y provisión de recursos.

11. **Un pipeline típico con CodePipeline incluye CodeCommit → CodeBuild → CodeDeploy**, lo cual garantiza un flujo automatizado desde el desarrollo hasta la producción.

12. **CodeArtifact permite almacenar dependencias, librerías y paquetes de software**, actuando como un repositorio central que facilita la gestión de versiones y la seguridad en la distribución de artefactos.

13. **El servicio es compatible con herramientas como npm, pip, Maven y NuGet**, integrándose perfectamente con los entornos de desarrollo comunes.

14. **AWS CDK (Cloud Development Kit) permite definir infraestructura usando lenguajes de programación conocidos**, como TypeScript, Python o Java, los cuales luego son convertidos en plantillas de CloudFormation.

15. **El uso del CDK mejora la experiencia del desarrollador al ofrecer tipado, estructuras reutilizables, y lógica condicional dentro del código de infraestructura**, lo que facilita el mantenimiento y la escalabilidad de los entornos.

16. **La combinación de estas herramientas permite implementar soluciones CI/CD completas en AWS**, desde el almacenamiento de código hasta el despliegue automatizado, todo gestionado dentro del ecosistema de servicios de la nube.

17. **Los servicios de despliegue como CloudFormation, Beanstalk y CodeDeploy se complementan con los servicios para desarrolladores**, formando una plataforma robusta para DevOps y automatización.

18. **Dominar estas herramientas permite construir, probar, desplegar y escalar aplicaciones en AWS con rapidez, seguridad y confiabilidad**, aprovechando al máximo el enfoque moderno de infraestructura como código y entrega continua.