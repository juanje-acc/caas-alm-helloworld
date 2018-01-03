# caas-alm-helloworld


**Pipeline ALM de Contenedores de prueba**

A continuacion se enumeran los pasos para crear una pipeline de creacion de contenedores en el stack alm en entorno local.

1. Crear un repositorio Git nuevo o una carpeta nueva en un repo existente 
2. Configurar en los settings generales de jenkins las credenciales para acceder al repositorio nexus donde publicaremos la imagen una vez esté construida  
	kind username password
	Scope global
	username admin
	password admin123
	especificar id:  sandbox-repository

3. Configurar en los settings generales de jenkins las credenciales para acceder al repo git que contendrá el jenkinsfile y dockerfile con sus dependencias.
	kind username password
	Scope global
	username usuario_git
	password pass_git
	especificar id:  caas-helloworld

4. En la consola de jenkins, pulsar en "Nueva tarea jenkins"  y elegir pipeline .
5. Configurar la pipeline.

6. Configurar el Build trigger  "consultar SCM" especificando la siguiente regla:  H/5 * * * *
	Seleccionar "Pipeline script" from scm
	Seleccionar "Git" como scm
	Especificar la url del repo git con el jenkinsfile
	Especificar credenciales: caas-helloworld
	Especificar rama master del repo git para hacer el checkout
	Especificar script path especificar ruta donde está el jenkinsfile
	
7. Editar jenkinsfile y dockerfile con sus dependencias. 
	jenkinsfile contendra las stages 
		clone
		build
		test
		push
		En el push se especifica la url del repo sandbox de nuestro nexus creado en el stack alm, y las credenciales creadas previamente para conectar con el repositorio.

8. Hacer commit y push del repo git conteniendo el fichero	jenkinsfile
9. En el mismo path poner el resto de ficheros
		dockerfile
		dependencias de dockerfile

10. Ejecutar la pipeline y revisar los logs en la vista console de jenkins
