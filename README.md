## Pipeline ALM de Contenedores de prueba

A continuacion se enumeran los pasos para crear una pipeline de creacion de contenedores en el stack alm en entorno local.

1. Crear un repositorio Git nuevo o una carpeta nueva en un repo existente 
2. Configurar en los settings generales de jenkins las credenciales para acceder al repositorio nexus donde publicaremos la imagen una vez esté construida  
- kind: username password
- Scope: global
- username: admin
- password: admin123
- id:  sandbox-repository-credentials

3. Configurar en los settings generales de jenkins las credenciales para acceder al repo git que contendrá el jenkinsfile y dockerfile con sus dependencias.
- kind: username password
- Scope: global
- username: usuario_git
- password: pass_git
- id: caas-helloworld-credentials

4. En la consola de jenkins, pulsar en "Nueva tarea jenkins" y elegir el tipo de tarea "pipeline".

5. Configurar la seccion  Build trigger si queremos que se construya la pipeline cuando haya cambios en el git.  
- Elegir "consultar SCM" especificando la siguiente regla de polling  H/5 * * * *
- Seleccionar "Pipeline script" from scm
- Seleccionar "Git" como scm
- Especificar la url del repo git con el jenkinsfile
- Especificar credenciales: caas-helloworld
- Especificar rama del repo git que contiene el jenkinsfile para hacer el checkout
- Especificar como script path la ruta donde está el jenkinsfile dentro del repositorio git.
Para este ejemplo ( provisioning\server-template\caas-alm-helloworld\Jenkinsfile )

6. Crear jenkinsfile y dockerfile con sus dependencias.  
El contenido del fichero jenkinsfile contiene las siguientes etapas definidas
- clone: Realiza el git clone del repo configurado previamente en la pipeline
- build: Construye la imagen del contenedor a partir del Dockerfile
- test: Etapa específica para validar que el build es correcto. 
- push: Push de la imagen del contenedor en nuestro docker registry privado, para ello se especifica la url del repo sandbox de nuestro nexus creado en el stack alm, y las credenciales creadas previamente para conectar con el repositorio.

7. Hacer commit y push del repo git conteniendo el fichero jenkinsfile
8. En el mismo path donde se ubica el Jenkinsfile, poner el resto de ficheros para la construccion de la imagen del contenedor de ejemplo. 
- Dockerfile
- main.js
- package.json

Esta imagen en concreto hereda de una imagen base ( node:7-onbuild ) que es un contenedor con un servidor Node.js que levanta el servidor al inicio. 

9. Ejecutar la pipeline y revisar los logs en la vista console de jenkins
10. Una vez ejecutada con exito la pipeline, ir a la url de nuestro repositorio nexus https://nexus.demo.com/#browse/search/docker y verificar que tenemos la imagen caixabank-caas/helloworld publicada.


