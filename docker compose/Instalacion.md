# Instlando docker compose
Docker compose es una herramienta para definir aplicaciones que utilizan multiples contenedores usando archivos de configuracion Yaml.

https://docs.docker.com/compose/install/

### Paso 1
Baja el software del repositorio oficial abajo usando el comando curl

```shell
sudo curl -L "https://github.com/docker/compose/releases/download/1.26.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

### Paso 2

Da permisos de ejecucion sobre el directorio donde bajaste el docker compose de su repositorio.

```shell
sudo chmod +x /usr/local/bin/docker-compose
```

### Valida

Ejecuta el comando docer-compose --version

y valida que como salida obtengas la versio de la herramienta
