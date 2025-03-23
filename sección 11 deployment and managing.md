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