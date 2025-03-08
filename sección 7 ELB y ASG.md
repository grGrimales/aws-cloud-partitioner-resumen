## 65. High Availability, Scalability, Elasticity**

1. **Escalabilidad y Alta Disponibilidad** son conceptos clave en la computación en la nube. La escalabilidad permite manejar mayores cargas de trabajo ajustando los recursos, mientras que la alta disponibilidad garantiza la continuidad del servicio incluso ante fallos.

2. **Escalabilidad Vertical:** Consiste en aumentar la capacidad de una instancia, por ejemplo, cambiando de una instancia EC2 t2.micro a una t2.large. Es útil en sistemas no distribuidos como bases de datos, pero tiene límites físicos.

3. **Ejemplo de Escalabilidad Vertical:** En un centro de llamadas, se podría mejorar la capacidad de un operador junior ascendiéndolo a operador senior, permitiéndole manejar más llamadas simultáneas.

4. **Escalabilidad Horizontal:** Aumenta la cantidad de instancias o nodos en un sistema distribuido. En un centro de llamadas, esto equivaldría a contratar más operadores en lugar de mejorar uno existente.

5. **Ejemplo de Escalabilidad Horizontal:** Si un sistema tiene una sola instancia EC2 manejando tráfico web, la escalabilidad horizontal agregaría más instancias para distribuir la carga y mejorar la capacidad de respuesta.

6. **Alta Disponibilidad:** Implica ejecutar la aplicación en múltiples zonas de disponibilidad (AZ) dentro de AWS. Si una zona falla, otra puede seguir operando sin interrupciones.

7. **Ejemplo de Alta Disponibilidad:** Un centro de llamadas con oficinas en Nueva York y San Francisco podría continuar operando si Nueva York sufre un apagón, derivando las llamadas a San Francisco.

8. **AWS y Alta Disponibilidad:** Se logra ejecutando instancias en diferentes zonas de disponibilidad y utilizando balanceadores de carga para distribuir el tráfico.

9. **Escalabilidad en EC2:** Se puede escalar verticalmente aumentando el tamaño de la instancia (scale up/down) o horizontalmente añadiendo o eliminando instancias (scale out/in).

10. **Auto Scaling Groups y Load Balancing:** AWS proporciona Auto Scaling Groups para administrar la escalabilidad horizontal y Elastic Load Balancing para distribuir el tráfico entre instancias.

11. **Ejemplo de Auto Scaling:** Un e-commerce que experimenta alta demanda en eventos como Black Friday puede configurar un Auto Scaling Group para aumentar instancias automáticamente y reducirlas cuando la demanda baja.

12. **Elasticidad en la Nube:** Permite que un sistema ajuste automáticamente sus recursos en función de la carga. Es un concepto clave en AWS, optimizando costos al pagar solo por los recursos utilizados.

13. **Ejemplo de Elasticidad:** Un servidor web que experimenta picos de tráfico en horarios específicos puede usar Auto Scaling para añadir instancias cuando sea necesario y eliminarlas cuando la demanda disminuye.

14. **Diferencia entre Escalabilidad y Elasticidad:** La escalabilidad es la capacidad de un sistema de aumentar su capacidad, mientras que la elasticidad automatiza este proceso para responder a cambios en la demanda.

15. **Agilidad en AWS:** Se refiere a la capacidad de desplegar rápidamente nuevos recursos con unos pocos clics, reduciendo el tiempo de aprovisionamiento de semanas a minutos, facilitando la iteración rápida en proyectos.

16. **Ejemplo de Agilidad:** Un equipo de desarrollo puede lanzar nuevas instancias de bases de datos en AWS en minutos en lugar de esperar semanas para la compra y configuración de servidores físicos.

17. **Conclusión:** AWS permite implementar estrategias de alta disponibilidad, escalabilidad y elasticidad para optimizar el rendimiento, minimizar el tiempo de inactividad y reducir costos mediante un uso eficiente de los recursos en la nube.

## 66. Elastic Load Balancing (ELB) Overview**

1. **Elastic Load Balancing (ELB)** es un servicio gestionado por AWS que distribuye el tráfico entrante entre múltiples instancias EC2 para mejorar la escalabilidad y disponibilidad de las aplicaciones.

2. **Funcionamiento del Load Balancer:** Actúa como un punto de entrada único para los usuarios, recibiendo las peticiones y distribuyéndolas entre las instancias EC2 disponibles en el backend.

3. **Balanceo de Carga Dinámico:** A medida que aumenta el número de usuarios, ELB distribuye la carga de manera equitativa entre las instancias, evitando sobrecargas en un solo servidor.

4. **Manejo de Fallos:** Realiza chequeos de salud en las instancias EC2 y deja de dirigir tráfico a aquellas que fallen, garantizando la disponibilidad continua del servicio.

5. **Soporte para SSL/TLS:** ELB permite la terminación de SSL, lo que facilita la configuración de HTTPS para sitios web y aplicaciones seguras.

6. **Alta Disponibilidad:** Puede operar en múltiples zonas de disponibilidad (AZ), lo que garantiza la continuidad del servicio incluso si una región experimenta fallos.

7. **Tipos de Load Balancers en AWS:** AWS ofrece cuatro tipos principales de balanceadores de carga: Application Load Balancer (ALB), Network Load Balancer (NLB), Gateway Load Balancer (GWLB) y Classic Load Balancer (CLB).

8. **Application Load Balancer (ALB):** Opera en la capa 7 (HTTP/HTTPS), permite enrutamiento basado en contenido y es ideal para aplicaciones web y microservicios.

9. **Ejemplo de ALB:**
   ```
   aws elbv2 create-load-balancer \
     --name my-load-balancer \
     --subnets subnet-abc123 subnet-def456 \
     --security-groups sg-12345
   ```

10. **Network Load Balancer (NLB):** Opera en la capa 4 (TCP/UDP), es altamente eficiente y capaz de manejar millones de solicitudes por segundo con baja latencia.

11. **Ejemplo de NLB:**
   ```
   aws elbv2 create-load-balancer \
     --name my-network-load-balancer \
     --type network \
     --subnets subnet-xyz789
   ```

12. **Gateway Load Balancer (GWLB):** Opera en la capa 3, enruta tráfico a firewalls gestionados en instancias EC2, permitiendo análisis de paquetes y detección de intrusos.

13. **Ejemplo de GWLB:**
   ```
   aws elbv2 create-load-balancer \
     --name my-gateway-load-balancer \
     --type gateway \
     --subnets subnet-123456
   ```

14. **Classic Load Balancer (CLB):** Antigua generación de balanceadores que combinaban características de ALB y NLB, pero han sido reemplazados en favor de soluciones modernas.

15. **Comparación de Load Balancers:** ALB es ideal para HTTP/HTTPS, NLB para tráfico de alto rendimiento en TCP/UDP, y GWLB para seguridad y análisis de paquetes.

16. **ELB y Direcciones IP:** NLB proporciona una IP estática mediante Elastic IPs, mientras que ALB proporciona un DNS para acceder a la aplicación.

17. **Seguridad y Firewall:** ELB se puede integrar con AWS WAF (Web Application Firewall) para proteger aplicaciones contra ataques comunes como inyección SQL y XSS.

18. **Optimización de Costos:** ELB permite pagar solo por el tráfico manejado, optimizando costos y mejorando la eficiencia de los recursos en la nube.

## 67. Application Load Balancer (ALB) Hands On**

1. **Creación de instancias EC2:** Se lanza un conjunto de instancias EC2 en AWS para recibir tráfico de prueba. Se utilizan dos instancias con Amazon Linux 2 y tipo t2.micro.

2. **Configuración de la red:** Se asigna un grupo de seguridad existente que permite tráfico HTTP y SSH. Esto garantiza que las instancias puedan recibir solicitudes desde internet.

3. **Uso de datos de usuario en EC2:** Se configura un script de "EC2 user data" para automatizar la configuración de las instancias al iniciar, permitiendo que ejecuten un servidor web simple.

4. **Verificación de instancias:** Se copian las direcciones IPv4 de cada instancia y se accede a ellas desde el navegador, mostrando un mensaje de "Hello World".

5. **Necesidad de un Load Balancer:** Para distribuir la carga entre múltiples instancias, se implementa un Application Load Balancer (ALB), permitiendo acceso desde una sola URL.

6. **Creación del Application Load Balancer (ALB):** Se selecciona el tipo de load balancer adecuado (ALB para tráfico HTTP/HTTPS) y se configura con un esquema de internet-facing.

7. **Configuración de zonas de disponibilidad:** Se implementa el ALB en múltiples zonas de disponibilidad para garantizar alta disponibilidad y redundancia.

8. **Asignación de un grupo de seguridad al ALB:** Se crea un nuevo grupo de seguridad, permitiendo solo tráfico HTTP en el puerto 80.

9. **Configuración de reglas de acceso:** Se define una regla de inbound en el grupo de seguridad que permite tráfico HTTP desde cualquier origen, asegurando accesibilidad al ALB.

10. **Listeners y routing:** Se configura un listener en el puerto 80 que redirige el tráfico entrante a un "Target Group" que contiene las instancias EC2.

11. **Creación del Target Group:** Se define un grupo de destino llamado "demo-tg-alb", configurado para recibir tráfico HTTP en el puerto 80.

12. **Registro de instancias en el Target Group:** Se agregan ambas instancias EC2 al grupo de destino, permitiendo que el ALB distribuya el tráfico entre ellas.

13. **Creación del Load Balancer:** Una vez configurado el ALB y el grupo de destino, se procede a su creación y aprovisionamiento.

14. **Verificación del Load Balancer:** Se obtiene el DNS del ALB y se accede a él desde el navegador. Las respuestas provienen alternadamente de ambas instancias EC2.

15. **Prueba de balanceo de carga:** Al refrescar la página varias veces, se observa que el ALB distribuye las solicitudes entre las instancias registradas.

16. **Prueba de manejo de fallos:** Se detiene una de las instancias EC2. El ALB detecta que está inactiva y deja de enviarle tráfico.

17. **Verificación de estado en el Target Group:** Se revisa el estado de las instancias en el grupo de destino y se confirma que la instancia detenida aparece como "unhealthy".

18. **Reactivación de la instancia:** Se inicia nuevamente la instancia EC2 y se espera a que pase los chequeos de salud del ALB.

19. **Observación de recuperación automática:** Una vez que la instancia pasa los chequeos, el ALB reanuda el envío de tráfico a ambas instancias.

20. **Conclusión:** El ALB permite balancear carga automáticamente y manejar fallos sin intervención manual, mejorando la disponibilidad y rendimiento de aplicaciones en AWS.

