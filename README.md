# caas-alm-helloworld

1.- Crear un repositorio Git nuevo o una carpeta nueva en un repositorio  existente. En este repositorio se ubicará la pipeline (Jenkinsfile) y los ficheros necesarios para construir la imagen.  

2.- Configurar credenciales en jenkins para acceder al repositorio nexus.
	- Acceder a la consola de admin de jenkins http://jenkins_host:8090  
	
	Configura las siguientes opciones accediendo a través de : Credentials, System, Global Credentials, Add Credentials  

	kind: username with password
	Scope: global
	Username: admin
	Password: admin123
	Id:  sandbox-registry

3.-Configurar otras credenciales en para acceder al repositorio git con la carpeta que contiene la pipeline y los ficheros de construccion de imagen.

	Kind: username with password
	Scope: global
	Username: usuario_git
	Password: pass_git
	Id:  alm-repository
	


4.- En la consola de admin de Jenkins, pinchar en "Nueva tarea" :
	- Especificar nombre de la pipeline.
	- Seleccionar el tipo de tarea pipeline de la lista
	- Pulsar OK
	- En la opcion "Build triggers" seleccionar "Consultar repositorio SCM" y especificar la planificación "H/5 * * * *""
	- En el apartado en el desplegable de definicion seleccionar "Pipeline script from SCM"
	- En SCM, seleccionar "Git" del desplegable.  
	- En Repositories, especificar los valores del repositorio Git, seleccionar las credenciales  del repositorio git.
	- Especificar las ramas del repositorio a descargar.
	- En el campo Script path especificar ruta donde está ubicado el fichero  jenkinsfile dentro del repositorio.
	
5.- Subir al repositorio Git los ficheros jenkinsfile y dockerfile con sus dependencias, ubicados en esta carpeta. 
	
7.- Ejecutar la pipeline y revisar los logs en la vista console de jenkins