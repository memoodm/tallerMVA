# Taller Modelado y Validación de Arquitectura
ESPECIALIZACIÓN EN ARQUITECTURA EMPRESARIALDE SOFTWARE
Some projects related with Architecture Modeling and Validation Course given at Pontificia Universidad Javeriana - Bogotá - Colombia
Integrantes:
- Guillermo De Mendoza
- Federico Urbina
- Omar Noriega

![DOCKER](https://github.com/memoodm/tallerMVA/blob/master/images/ImgArquitectura.png)

### Arquitectura
- Decisiones de Arquitectura
	- DEC001 Servicio basado en el patrón intermediate Routing:
	- DEC002 Uso de FUSE como Broker
	- DEC003 Uso de Docker para contenerización de servicios
	- DEC004 Uso de RAML para la definición del contrato
	- DEC005 Dispatcher usado en el API
	- DEC006 Uso de Domain Inventory
	- DEC007 REST como servicio único interno

- Descripcion de decisiones:

DEC001 Servicio basado en el patrón intermediate Routing:
Problema: Dado lo compleja que puede ser la composición de los servicios, es difícil anticipar y diseñar todos los posibles escenarios en tiempo de ejecución
Solución: Usar un enrutamiento lógico intermedio para determinar las diferentes rutas de mensaje
  
### Artefactos del proyecto
| Artefacto | Descripcion | Contenedor | Tipo | Port |
| ------ | ------ | ------ | ------ | ------ |
| apache-camel-jaxrs | Logica del bus, la cual realiza la coreografia de servicios | jbossFuse | jar | 9000
| API-Servicios | Selector de consumo de servicios segun los parametros del artefacto ROUTING dependiendo de los parametros y el servicio seleccionado | glassfish | war | 8080
| FRONT | Prueba grafica de la coreografia del bus al enviarle solicitudes | glassfish | war | 8080
| ROUTING | servicio contenedor de las rutas y administrador de subscripciones de los servicios expustos | docker | docker | 8888
| W1-REST-Service | servicio que expone un end point en rest | docker | docker | 8081
| W1-SOAP-Service | servicio que expone un end point en soap | docker | docker | 8082

### Project start

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

### Descripcion de start
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



