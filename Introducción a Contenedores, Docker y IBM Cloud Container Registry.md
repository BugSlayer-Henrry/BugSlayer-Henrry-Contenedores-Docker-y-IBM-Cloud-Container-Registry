# BugSlayer-Henrry-Introducción-a-Contenedores-Docker-y-IBM-Cloud-Container-Registry

## Introducción a Contenedores, Docker y IBM Cloud Container Registry

![cognitiveclass.ai logo](https://github.com/user-attachments/assets/6ebbf608-07b8-470e-94cf-89fbeb2a39b5)

### Objetivos

En este laboratorio, usted:
- Extraerá una imagen de Docker Hub
- Ejecutará una imagen como un contenedor usando docker
- Construirá una imagen usando un Dockerfile
- Subirá una imagen al IBM Cloud Container Registry

---

## Verificar el entorno y las herramientas de línea de comandos

Abre una ventana de terminal utilizando el menú en el editor: `Terminal > New Terminal`.

**Nota:** Si la terminal ya está abierta, por favor omite este paso.

![image](https://github.com/user-attachments/assets/6ebbf608-07b8-470e-94cf-89fbeb2a39b5)

### Verifica que Docker CLI esté instalado
```sh
docker --version
```
Deberías ver la siguiente salida, aunque la versión puede ser diferente:

![image](https://github.com/user-attachments/assets/218d2781-90d7-4683-80ee-f8104b489c02)

### Verifica que IBM Cloud CLI esté instalado
```sh
ibmcloud version
```
Deberías ver la siguiente salida, aunque la versión puede ser diferente:

![image](https://github.com/user-attachments/assets/419e5a85-b444-41e2-86bc-196f907090b8)

---

## Cambia a la carpeta de tu proyecto

**Nota:** Si ya estás en la carpeta `/home/project`, por favor omite este paso.
```sh
cd /home/project
```

Clona el repositorio de Git que contiene los artefactos necesarios para este laboratorio, si no existe ya:
```sh
[ ! -d 'CC201' ] && git clone https://github.com/ibm-developer-skills-network/CC201.git
```

![image](https://github.com/user-attachments/assets/28eaff62-3c1f-49b8-8283-689a603d63f4)

Cambia al directorio de este laboratorio:
```sh
cd CC201/labs/1_ContainersAndDocker/
```
Enumera el contenido del directorio:
```sh
ls
```
![image](https://github.com/user-attachments/assets/0e53ad28-cc0b-4685-b22f-e37bd5abef83)

---

## Extraer una imagen de Docker Hub y ejecutarla como un contenedor

Lista tus imágenes:
```sh
docker images
```
Deberías ver una tabla vacía:

![image](https://github.com/user-attachments/assets/e7d004a5-f666-41b9-95da-138f979b2f14)

Descarga la imagen `hello-world` de Docker Hub:
```sh
docker pull hello-world
```

![image](https://github.com/user-attachments/assets/19bbdede-f8b6-4156-a042-943d4d739d59)

Lista las imágenes nuevamente:
```sh
docker images
```
![image](https://github.com/user-attachments/assets/1714a526-2539-4a89-9a15-e380a5538f87)

Ejecuta la imagen como un contenedor:
```sh
docker run hello-world
```
![image](https://github.com/user-attachments/assets/d900745f-09a7-4853-b4ef-7319b426da0a)

Lista los contenedores:
```sh
docker ps -a
```
![image](https://github.com/user-attachments/assets/5302aea5-34ad-4612-87b3-285d526d970d)

Elimina el contenedor (reemplaza `<container_id>` con el ID obtenido anteriormente):
```sh
docker container rm <container_id>
```
![image](https://github.com/user-attachments/assets/6375746d-a3f2-4f62-8f4d-c19317366697)

Verifica la eliminación:
```sh
docker ps -a
```
![image](https://github.com/user-attachments/assets/73b31f81-cf79-4658-b1d2-c202fe22f1d1)

---

## Construir una imagen usando un Dockerfile

El directorio contiene una aplicación de Node.js. Los archivos necesarios son:
- `app.js` (aplicación principal)
- `package.json` (dependencias)
- `Dockerfile` (instrucciones para construir la imagen)

![image](https://github.com/user-attachments/assets/c5c468a2-e433-4b88-97af-f7c77c690f5a)

Construye la imagen:
```sh
docker build . -t myimage:v1
```
![image](https://github.com/user-attachments/assets/8ed4ec79-c094-4655-982b-e9b957618f9e)

Lista las imágenes para verificar:
```sh
docker images
```
![image](https://github.com/user-attachments/assets/40e23363-54bc-474c-bb32-b4d8d8709948)

Ejecuta la imagen como un contenedor:
```sh
docker run -dp 8080:8080 myimage:v1
```
![image](https://github.com/user-attachments/assets/5e1cfd7e-9ebe-48c8-a1b6-96696ee71dbf)

Prueba la aplicación:
```sh
curl localhost:8080
```
![image](https://github.com/user-attachments/assets/0cc13f65-c839-47df-9253-14967d26281d)

Detén el contenedor:
```sh
docker stop $(docker ps -q)
```
![image](https://github.com/user-attachments/assets/b58c49fb-dcb4-44dc-bf80-20fe48ba3cf0)

Verifica que se haya detenido:
```sh
docker ps
```
![image](https://github.com/user-attachments/assets/e5869996-2b68-4d03-a178-6160d370e2a6)

---

## Subir la imagen al IBM Cloud Container Registry

Verifica la cuenta de IBM Cloud:
```sh
ibmcloud target
```
![image](https://github.com/user-attachments/assets/347ce4e9-45fe-4a17-a08d-b8e58d40c0c5)

Verifica los espacios de nombres en IBM Cloud Container Registry:
```sh
ibmcloud cr namespaces
```
![image](https://github.com/user-attachments/assets/43be6f5c-c5a8-4c5a-936b-363890246b48)
Deberías ver dos espacios de nombres listados que comienzan con `sn-labs`:

El primero, con tu nombre de usuario, es un espacio de nombres solo para ti. Tienes acceso completo de lectura y escritura a este espacio de nombres.
El segundo espacio de nombres, que es un espacio de nombres compartido, solo te proporciona acceso de lectura.

Asegúrate de que estás apuntando a la región adecuada para tu cuenta en la nube, por ejemplo, la región `us-south` donde residen estos espacios de nombres, como viste en la salida del comando `ibmcloud target`.

```sh
ibmcloud cr region-set us-south
```

![image](https://github.com/user-attachments/assets/8af333a4-ee03-4485-a9dc-d903a7acf614)

Inicia sesión en el daemon de Docker local en IBM Cloud Container Registry para que puedas subir y bajar imágenes del registro.

```sh
ibmcloud cr login
```

![image](https://github.com/user-attachments/assets/56119ec0-d56d-43a3-b381-eab2d4c33e63)

Exporta tu espacio de nombres como una variable de entorno para que pueda ser utilizada en comandos posteriores.

```sh
export MY_NAMESPACE=sn-labs-$USERNAME
```

![image](https://github.com/user-attachments/assets/4c87d656-b649-4a86-a2d3-0e2f870ba1d7)

Etiqueta tu imagen para que pueda ser enviada al Registro de Contenedores de IBM Cloud.

```sh
docker tag myimage:v1 us.icr.io/$MY_NAMESPACE/hello-world:1
```

![image](https://github.com/user-attachments/assets/aa4eb896-8f02-4144-aa77-8342caab8ec0)

Envía la imagen recién etiquetada al Registro de Contenedores de IBM Cloud.

```sh
docker push us.icr.io/$MY_NAMESPACE/hello-world:1
```

![image](https://github.com/user-attachments/assets/bde77cb3-1b03-4c10-aa71-9c95fb527712)

**Nota:** Si has intentado este laboratorio anteriormente, es posible que la sesión anterior aún esté persistente. En tal caso, verás un mensaje de `La capa ya existe` en lugar del mensaje de `Subido` en la salida anterior. Te recomendamos que continúes con los siguientes pasos del laboratorio.

Verifica que la imagen se haya subido correctamente listando las imágenes en el Registro de Contenedores.

```sh
ibmcloud cr images
```

![image](https://github.com/user-attachments/assets/dff0969e-4124-419d-a462-2604aff9dbea)

Opcionalmente, para ver solo imágenes dentro de un espacio de nombres específico:

```sh
ibmcloud cr images --restrict $MY_NAMESPACE
```

![image](https://github.com/user-attachments/assets/5f98b46b-44e4-419e-aaf1-65d14b98d5ea)

Deberías ver el nombre de tu imagen en la salida.

---

¡Felicidades! Has completado el segundo laboratorio del primer módulo de este curso.

© IBM Corporation. Todos los derechos reservados.
