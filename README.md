# BugSlayer-Henrry-Introducci-n-a-Contenedores-Docker-y-IBM-Cloud-Container-Registry

Introducción a Contenedores, Docker y IBM Cloud Container Registry
cognitiveclass.ai logo
Objetivos
En este laboratorio, usted:

Extraerá una imagen de Docker Hub
Ejecutará una imagen como un contenedor usando docker
Construirá una imagen usando un Dockerfile
Subirá una imagen al IBM Cloud Container Registry
Nota: Por favor, complete el laboratorio en una sola sesión sin interrupciones, ya que el laboratorio puede entrar en modo offline y causar errores. Si enfrenta algún problema/error durante el proceso del laboratorio, por favor cierre sesión en el entorno del laboratorio. Luego limpie la caché y las cookies de su sistema e intente completar el laboratorio.![image](https://github.com/user-attachments/assets/dc2287c7-bcf8-4c0b-9eb7-14d44f00ff8b)


Importante:
Es posible que ya tenga una cuenta de IBM Cloud y que incluso tenga un espacio de nombres en el IBM Container Registry (ICR). Sin embargo, en este laboratorio no estará utilizando su propia cuenta de IBM Cloud ni su propio espacio de nombres de ICR. Estará utilizando una cuenta de IBM Cloud que ha sido generada automáticamente para usted para este ejercicio. El entorno del laboratorio no tendrá acceso a ningún recurso dentro de su cuenta personal de IBM Cloud, incluidos los espacios de nombres y las imágenes de ICR.

Verificar el entorno y las herramientas de línea de comandos
Abre una ventana de terminal utilizando el menú en el editor: Terminal > New Terminal.
Nota: Si la terminal ya está abierta, por favor omite este paso.



Verifica que docker CLI esté instalado.
docker --version
Deberías ver la siguiente salida, aunque la versión puede ser diferente:



Verifica que ibmcloud CLI esté instalado.
ibmcloud version
Deberías ver la siguiente salida, aunque la versión puede ser diferente:



Cambia a la carpeta de tu proyecto.
Nota: Si ya estás en la carpeta ‘/home/project’, por favor omite este paso.

cd /home/project
Clona el repositorio de git que contiene los artefactos necesarios para este laboratorio, si no existe ya.
[ ! -d 'CC201' ] && git clone https://github.com/ibm-developer-skills-network/CC201.git


Cambia al directorio de este laboratorio ejecutando el siguiente comando. cd cambiará el directorio de trabajo/actual al directorio con el nombre especificado, en este caso CC201/labs/1_ContainersAndDcoker.
cd CC201/labs/1_ContainersAndDocker/
Enumera el contenido de este directorio para ver los artefactos de este laboratorio.
ls


Extraer una imagen de Docker Hub y ejecutarla como un contenedor
Usa la docker CLI para listar tus imágenes.
docker images
Deberías ver una tabla vacía (con solo encabezados) ya que aún no tienes ninguna imagen.



Descarga tu primera imagen de Docker Hub.
docker pull hello-world


Lista las imágenes nuevamente.
docker images
Ahora deberías ver la imagen hello-world presente en la tabla.



Ejecuta la imagen hello-world como un contenedor.
docker run hello-world
Deberías ver un mensaje de ‘¡Hola desde Docker!’.

También habrá una explicación de lo que Docker hizo para generar este mensaje.



Lista los contenedores para ver que tu contenedor se ejecutó y salió con éxito.
docker ps -a
Entre otras cosas, para este contenedor deberías ver un ID de contenedor, el nombre de la imagen (hello-world), y un estado que indica que el contenedor salió exitosamente.



Toma nota del ID DE CONTENEDOR de la salida anterior y reemplaza la etiqueta <container_id> en el comando a continuación con este valor. Este comando elimina tu contenedor.
docker container rm <container_id>


Verifica que el contenedor haya sido eliminado. Ejecuta el siguiente comando.
docker ps -a


¡Felicidades por haber descargado una imagen de Docker Hub y por ejecutar tu primer contenedor! Ahora intentemos construir nuestra propia imagen.

Construir una imagen usando un Dockerfile
El directorio de trabajo actual contiene una aplicación simple de Node.js que ejecutaremos en un contenedor. La aplicación imprimirá un mensaje de saludo junto con el nombre del host. Los siguientes archivos son necesarios para ejecutar la aplicación en un contenedor:
app.js es la aplicación principal, que simplemente responde con un mensaje de hola mundo.
package.json define las dependencias de la aplicación.
Dockerfile define las instrucciones que Docker utiliza para construir la imagen.
Usa el Explorador para ver los archivos necesarios para esta aplicación. Haz clic en el ícono del Explorador (parece una hoja de papel) en el lado izquierdo de la ventana, y luego navega hasta el directorio de este laboratorio: CC201 > labs > 1_ContainersAndDocker. Haz clic en Dockerfile para ver los comandos requeridos para construir una imagen.
Dockerfile en el Explorador

Puedes refrescar tu comprensión de los comandos mencionados en el Dockerfile a continuación:

La instrucción FROM inicializa una nueva etapa de construcción y especifica la imagen base sobre la que se construirán las instrucciones posteriores.

El comando COPY nos permite copiar archivos a nuestra imagen.

La instrucción RUN ejecuta comandos.

La instrucción EXPOSE expone un puerto particular con un protocolo especificado dentro de un contenedor Docker.

La instrucción CMD proporciona un valor predeterminado para ejecutar un contenedor, o en otras palabras, un ejecutable que debería ejecutarse en tu contenedor.

Ejecuta el siguiente comando para construir la imagen:
docker build . -t myimage:v1
Como se vio en los videos del módulo, la salida crea una nueva capa para cada instrucción en el Dockerfile.



Lista las imágenes para ver tu imagen etiquetada como myimage:v1 en la tabla.
docker images


Ten en cuenta que, en comparación con la imagen hello-world, esta imagen tiene un ID de imagen diferente. Esto significa que las dos imágenes constan de diferentes capas; en otras palabras, no son la misma imagen.

Ejecutar la imagen como un contenedor
Ahora que tu imagen está construida, ejecútala como un contenedor con el siguiente comando:
docker run -dp 8080:8080 myimage:v1


La salida es un código único asignado por docker para la aplicación que estás ejecutando.

Ejecuta el comando curl para hacer ping a la aplicación como se indica a continuación.
curl localhost:8080


Si ves la salida como la anterior, indica que ‘¡Tu aplicación está en funcionamiento!’.

Ahora, para detener el contenedor, usamos docker stop seguido del id del contenedor. El siguiente comando utiliza docker ps -q para pasar la lista de todos los contenedores en ejecución:
docker stop $(docker ps -q)


Verifica si el contenedor se ha detenido ejecutando el siguiente comando.
docker ps


Sube la imagen al IBM Cloud Container Registry
El entorno ya debería haberte iniciado sesión en la cuenta de IBM Cloud que ha sido generada automáticamente para ti por el entorno de Skills Network Labs. El siguiente comando te dará información sobre la cuenta que estás utilizando:
ibmcloud target


El entorno también creó un espacio de nombres de IBM Cloud Container Registry (ICR) para ti. Dado que el Container Registry es multiusuario, se utilizan espacios de nombres para dividir el registro entre varios usuarios. Usa el siguiente comando para ver los espacios de nombres a los que tienes acceso:
ibmcloud cr namespaces


Deberías ver dos espacios de nombres listados que comienzan con sn-labs:

El primero, con tu nombre de usuario, es un espacio de nombres solo para ti. Tienes acceso completo de lectura y escritura a este espacio de nombres.
El segundo espacio de nombres, que es un espacio de nombres compartido, solo te proporciona acceso de lectura.
Asegúrate de que estás apuntando a la región adecuada para tu cuenta en la nube, por ejemplo, la región us-south donde residen estos espacios de nombres, como viste en la salida del comando ibmcloud target.
ibmcloud cr region-set us-south


Inicia sesión en el daemon de Docker local en IBM Cloud Container Registry para que puedas subir y bajar imágenes del registro.
ibmcloud cr login


Exporta tu espacio de nombres como una variable de entorno para que pueda ser utilizada en comandos posteriores.
export MY_NAMESPACE=sn-labs-$USERNAME


Etiqueta tu imagen para que pueda ser enviada al Registro de Contenedores de IBM Cloud.
docker tag myimage:v1 us.icr.io/$MY_NAMESPACE/hello-world:1


Envía la imagen recién etiquetada al Registro de Contenedores de IBM Cloud.
docker push us.icr.io/$MY_NAMESPACE/hello-world:1


Nota: Si has intentado este laboratorio anteriormente, es posible que la sesión anterior aún esté persistente. En tal caso, verás un mensaje de ‘La capa ya existe’ en lugar del mensaje de ‘Subido’ en la salida anterior. Te recomendamos que continúes con los siguientes pasos del laboratorio.

Verifica que la imagen se haya subido correctamente listando las imágenes en el Registro de Contenedores.
ibmcloud cr images


Opcionalmente, para ver solo imágenes dentro de un espacio de nombres específico.

ibmcloud cr images --restrict $MY_NAMESPACE


Deberías ver el nombre de tu imagen en la salida.

¡Felicidades! Has completado el segundo laboratorio del primer módulo de este curso.

© IBM Corporation. Todos los derechos reservados.
