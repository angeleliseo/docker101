# docker101
Este repositorio contiene algunas practicas de docker

### Instalando un Web Server con Docker

**Como prerequisitos es necesario tener lo siguiente:**

* Maquina Virtual hecha con Virtual Box (opcional)
* Tener Ubuntu o Ubuntu Server Instalado
* Tener Docker instalado en ubuntu
* Tener dado de alta nuestro usuario en el grupo de docker

### Instalando Nginx web server

De forma introductoria, Nginx es un web server similar a Apache, sin embargo este esta diseñado penzado en el alto rendimiento y en la simlpeza a la hora de usarlo.

Se escogio este servidor debido a lo ligero y rapido asi como lo popular que se ha vuelto en herramientas DevOps ultimamente.

**Crea una carpeta donde almacenaras los archivos del web server**
Comunmente esta carpeta se le llama Document Root y es donde los archivos que el servidor web expondra se almacenaran.
Para esto usa el comando en tu consola de ubuntu:
```shell
mkdir contenido_web
```
metete al directorio con *cd contenido_web*
y ahora ejecuta *pwd* para obtener el path completo de la carpeta. Obtendras una salida similar a esta.

**/home/user/contenido_web**

#### Para instalar Nginx se tiene que ejecutar el siguiente.

Ejecutar el siguiente comando en la consola de nuestro Ubuntu:
Remplaza **/home/user/contenido_web** con el path que obtuviste anteriormente

```shell
docker run -p 8080:80 --name web_server -v /home/user/volumes/websrv:/usr/share/nginx/html:ro -d nginx
```

### Validando nuestra instalacion de Nginx

A continuacion procederemos a crear un archivo HTML de ejemplo y lo accederemos desde nuestro servidor.

Muevete de directorio hacia la carpeta que creaste en el paso anterior, en nuestro caso es **/home/user/contenido_web**

```shell
cd /home/user/contenido_web
```
Crearemos el archivo index.html de la siguiente manera:
```shell
toch index.html
```
Y finalmente procederemos a poner un texto de prueba
```shell
echo "<h1>Esta es mi pagina de prueba</h1>" >> index.html
```
Finalmente para probar que nuestro servidor muestra la pagina web que acabamos de crear haremos lo siguiente:
* Ejecuta el comando *ifconfig* para obtener la IP de tu maquina ubuntu
* Abre un browser en tu computadora personal (donde tienes instalado Virtual Box)
* En la barra de direcciones escribe la IP de tu servidor ubuntu y agrega ":" y el puerto "8080". de la sigiuente manera. **192.168.1.150:8080**
* Verifica que el browser muestre el texto *"Esta es mi pagina de prueba"*

