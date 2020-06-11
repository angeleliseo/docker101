# docker101
Este repositorio contiene algunas prácticas de docker

## Instalando un Web Server con Docker

**Como prerequisitos es necesario tener lo siguiente:**

* Máquina Virtual hecha con Virtual Box (opcional)
* Tener Ubuntu o Ubuntu Server Instalado
* Tener Docker instalado en ubuntu
* Tener dado de alta nuestro usuario en el grupo de docker

### Instalando Nginx web server

De forma introductoria, Nginx es un web server similar a Apache, sin embargo éste está diseñado pensando en el alto rendimiento y en la simlpeza a la hora de usarlo.

Se escogió este servidor debido a lo ligero y rápido, así como lo popular que se ha vuelto en herramientas DevOps últimamente.

**Crea una carpeta donde almacenarás los archivos del web server**
Comúnmente esta carpeta se le llama Document Root y es donde los archivos que el servidor web expondrá se almacenarán.
Para esto usa el comando en tu consola de ubuntu:
```shell
mkdir contenido_web
```
métete al directorio con *cd contenido_web*
y ahora ejecuta *pwd* para obtener el path completo de la carpeta. Obtendrás una salida similar a esta.

**/home/user/contenido_web**

#### Para instalar Nginx se tiene que ejecutar lo siguiente.

Ejecutar el siguiente comando en la consola de nuestro Ubuntu:
Remplaza **/home/user/contenido_web** con el path que obtuviste anteriormente

```shell
docker run -p 8080:80 --name web_server -v /home/user/volumes/websrv:/usr/share/nginx/html:ro -d nginx
```

### Validando nuestra instalación de Nginx

A continuación procederemos a crear un archivo HTML de ejemplo y lo accederemos desde nuestro servidor.

Muévete de directorio hacia la carpeta que creaste en el paso anterior, en nuestro caso es **/home/user/contenido_web**

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
Finalmente para probar que nuestro servidor muestra la página web que acabamos de crear haremos lo siguiente:
* Ejecuta el comando *ifconfig* para obtener la IP de tu máquina ubuntu
* Abre un browser en tu computadora personal (donde tienes instalado Virtual Box)
* En la barra de direcciones escribe la IP de tu servidor ubuntu y agrega ":" y el puerto "8080". de la sigiuente manera. **192.168.1.150:8080**
* Verifica que el browser muestre el texto *"Esta es mi pagina de prueba"*

### Facilitando el subir archivos 
Para hacer mucho más sencillo el trabajo de estar subiendo archivos a nuestro servidor web desde nuestra computadora haremos una carpeta compartida en nuestro servidor Ubuntu.
Para ello usaremos el protocolo SMB que es el que por defecto utiliza Windows y el software Samba en Linux.

**Iniciaremos instalando Samba.**
Para ello ejecutaremos el sigiuente comando en nuestra consola de Linux.
```shell
sudo apt install samba
```
Confirmamos y esperamos a que el software esté instalado.
Una vez terminado el proceso de descarga e instalación lo siguiente será comprobar que éste se instaló en nuestro sistema de forma correcta.
A continuación ejecutemos el siguiente comando en la consola para buscar el software instalado.
```shell
whereis samba
```
Debemos de recibir una salida similar a esta:
```shell
samba: /usr/sbin/samba /usr/lib/samba /etc/samba /usr/share/samba /usr/share/man/man7/samba.7.gz /usr/share/man/man8/samba.8.gz
```
Lo siguiente será configurar nuestra carpeta compartida, para esto tenemos que abrir el archivo de configuración de Samba de la siguiente forma:
```shell
sudo nano /etc/samba/smb.conf
```
Y al final del archivo agregar el bloque de código siguiente, éste da de alta una nueva carpeta compartida en nuestro sistema según el path que pasemos. En nuestro caso la carpeta que compartiremos será la misma que creamos para el contenido de nuestro web server. **/home/user/contenido_web**

```
[documentos_web]
    comment = Samba on Ubuntu
    path = /home/user/contenido_web
    read only = no
    browsable = yes
```
Ahora solo resta reiniciar nuestro servicio de Samba para que los cambios de configuración tomen efecto.
```shell
sudo service smbd restart
```

Y como paso final y necesario, tenemos que crear una constraseña para el usuario de Samba, por defecto toma el usuario de tu equipo Ubuntu, pero el password será independiente solo para Samba como un usuario independiente. Usemos el comando a continuación para crear el nuevo password para nuestro usuario de Samba.
```shell
sudo smbpasswd -a username
```

### Validando que ahora podemos subir archivos a nuestro web server por medio de Samba.

* Usa el comando **ifconfig** en tu Ubuntu para obtener la IP de tu equipo
* En tu computadora local (en donde tienes Virutal Box) en el explorador de windows o con las teclas Windows + R en el cuadro de texto escribe *\\192.168.1.150* reemplaza la IP en el ejemplo con la IP de tu equipo.
* Debes de ver una carpeta con el **nombre documentos_web**
* Abre la carpeta, se te pedirá, usuario y contraseña. Usa el usuario de tu máquina Ubuntu y la contraseña que generaste para el usuario Samba.
* Una vez puedas abrir la carpeta, solo hace falta agregar los archivos que quieras subir a tu web server. Por ejemplo sube o crea un hola.html que contenga el mensaje **"Hola, desde Nginx"**
* Y por último comprueba que este archivo es accesible desde tu web server abriendo el web browser en tu máquina local (la que tiene instalado Virtual Box) y poniendo en tu barra de direcciones **http://192.168.1.150:8080/hola.html** reemplaza la IP por la IP que obtuviste en los primeros pasos.
* El mensaje **"Hola, desde Nginx"** debe de ser desplegado en tu navegador.

## Muchas Felicidades, en hora buena
Ya has sido capaz de levantar y configurar un web server Nginx y facilitar su uso mediante una carpeta compartida!!

**Sigue aprendiendo**
