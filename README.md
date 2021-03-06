# Taller Modelado y Validación de Arquitectura
ESPECIALIZACIÓN EN ARQUITECTURA EMPRESARIALDE SOFTWARE
Some projects related with Architecture Modeling and Validation Course given at Pontificia Universidad Javeriana - Bogotá - Colombia
Integrantes:
- Guillermo De Mendoza
- Federico Urbina
- Omar Noriega



# 1. Arquitectura

![DOCKER](https://github.com/memoodm/tallerMVA/blob/master/images/ImgArquitectura.png)

## Decisiones de Arquitectura
	- DEC001 Servicio basado en el patrón intermediate Routing:
	- DEC002 Uso de FUSE como Broker
	- DEC003 Uso de Docker para contenerización de servicios
	- DEC004 Uso de RAML para la definición del contrato
	- DEC005 Dispatcher usado en el API
	- DEC006 Uso de Domain Inventory
	- DEC007 REST como servicio único interno

## Descripcion de decisiones:

### DEC001 Servicio basado en el patrón intermediate Routing:
Problema -> Dado lo compleja que puede ser la composición de los servicios, es difícil anticipar y diseñar todos los posibles escenarios en tiempo de ejecución
Solución -> Usar un enrutamiento lógico intermedio para determinar las diferentes rutas de mensaje

![img1](https://github.com/memoodm/tallerMVA/blob/master/images/img01.png)

### DEC002 Uso de Fuse para realizar la composición de servicios (not just ESB)
Problema: De que manera componer y construir servicios permitiendo el despliegue de sistemas distribuidos y permitiendo escalabilidad
Solución: Usando Jboss fuse se refleja la flexibilidad, escalabilidad y re-usabilidad en el sistema y se aleja de la idea de manejar una integración monolítica.

![img2](https://github.com/memoodm/tallerMVA/blob/master/images/img02.png)

### DEC003 Uso de Docker para contenerización de servicios
Problema: Despliegues de soluciones monolíticas  pueden llevar a una reducción significativa del desempeño 
Solución: Los servicios son desplegados independientemente, como unidades autónomas que son empacadas y gestionadas autonamente en imágenes contenidas.

![img3](https://github.com/memoodm/tallerMVA/blob/master/images/img03.png)

### DEC004 Dispatcher usado en el API
Problema: Cuando el sistema usa servicios distribuidos sobre una red, un cliente debe poder usar un servicio independiente de la locación del proveedor del servicio
Solución: el componente dispatcher actua como una capa intermedia entre cliente y servidor implementando una serie de servcio que permite a los clientes referenciar a los servidores por nombres en vez de locaciones físicas

![img4](https://github.com/memoodm/tallerMVA/blob/master/images/img04.png)

### DEC005 Uso de Domain Inventory
Problema: Establecer un inventario de servicios único en las empresas puede ser inmanejable
Solución: Los servicios pueden ser agrupados en inventarios de servicios específicos que pueden ser estandarizados y gobernados independientemente

![img5](https://github.com/memoodm/tallerMVA/blob/master/images/img05.png)

### DEC006 Uso de RAML para la definición del contrato
Problema: Como construir todo un API rápidamente y ver exactamente como se ve y probarlo
Solución: RAML permite usar un formato genérico y entendible que permite a cualquiera ver la documentación de los cambios.

![img6](https://github.com/memoodm/tallerMVA/blob/master/images/img06.png)

### DEC007 REST como servicio único interno
Problema: Como se pueden proveer estándares entre sistemas en la web haciendo mas fácil la comunicación entre ellos?
Solución: Usando REST se pueden implementar cliente y servidor independientemente sin que estos se conozcan entre si.

![img7](https://github.com/memoodm/tallerMVA/blob/master/images/img07.png)


  
# 2. Artefactos del proyecto
| Artefacto | Descripcion | Contenedor | Tipo | Port |
| ------ | ------ | ------ | ------ | ------ |
| apache-camel-jaxrs | Logica del bus, la cual realiza la coreografia de servicios | jbossFuse | jar | 9000
| API-Servicios | Selector de consumo de servicios segun los parametros del artefacto ROUTING dependiendo de los parametros y el servicio seleccionado | glassfish | war | 8080
| FRONT | Prueba grafica de la coreografia del bus al enviarle solicitudes | glassfish | war | 8080
| ROUTING | servicio contenedor de las rutas y administrador de subscripciones de los servicios expustos | docker | docker | 8888
| W1-REST-Service | servicio que expone un end point en rest | docker | docker | 8081
| W1-SOAP-Service | servicio que expone un end point en soap | docker | docker | 8082

# 3. Servidores del proyecto

| Servidor | Version | Link |
| ------ | ------ | ------ |
| JBoss Fuse ESB | 6.3.0 | https://developers.redhat.com/products/fuse/download/?sc_cid=701f2000000tvTBAAY&gclid=CjwKCAjwopTYBRAzEiwAnU4kb-fScsat-ciAaqvl86B1gJ9-p5gzD7eClFWvAhIVZUuW2rhKmEFtYRoCUR8QAvD_BwE&gclsrc=aw.ds |
| GlassFish | 4.1.1 | http://www.oracle.com/technetwork/es/middleware/glassfish/overview/index.html |
| Docker | 18.03.1-ce | https://www.docker.com/ |



# 4. Project start

Ejecutar el archivo de comando:
```sh
start.bat
```
El cual esta compuesto por los comandos:
```sh
start servers\jboss-fuse-6.3.0.redhat-187\bin\fuse.bat
start servers\GlassFish_Server\glassfish\bin\startserv.bat
docker pull memoodm/services:service_1_rest
docker pull memoodm/services:service_2_soap
docker pull memoodm/services:apiSelector
docker run -d -p 8081:8080 memoodm/services:service_1_rest
docker run -d -p 8082:8080 memoodm/services:service_2_soap
docker run -d -p 8888:8080 memoodm/services:Routing
```

# 5. Descripcion de start
Inicia el servidor de jboss, el cual incluye el artefacto de apache camel
```sh
start servers\jboss-fuse-6.3.0.redhat-187\bin\fuse.bat
```
Inicia el servidor de glassfish el cual contiene los api's de seleccion de servicios y el testing grafico del aplicativo
```sh
start servers\GlassFish_Server\glassfish\bin\startserv.bat
```
Descarga los docker de servicios utilizados por el proyecto
```sh
docker pull memoodm/services:service_1_rest
docker pull memoodm/services:service_2_soap
docker pull memoodm/services:apiSelector
```
Ejecuta los servicios en docker
```sh
docker run -d -p 8081:8080 memoodm/services:service_1_rest
docker run -d -p 8082:8080 memoodm/services:service_2_soap
docker run -d -p 8888:8080 memoodm/services:Routing
```



