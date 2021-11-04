## Crear el proyecto y el contenedor de prueba

Crear el repositorio y clonarlo:

```bash
git clone https://github.com/egibide-dam/practica-docker.git
```

Crear la carpeta y la página web:

```bash
mkdir -p website
echo '¡Hola mundo!' > website/index.html
```

Descargar la [imagen de Ubuntu 20.04](https://hub.docker.com/_/ubuntu/):

```bash
docker pull ubuntu:20.04
```

Arrancar el contenedor con la carpeta montada y el puerto expuesto:

Linux & macOS:

```bash
docker run --name ubuntu --rm -it -p 80:80 -v "$PWD/website:/web" ubuntu:20.04
```

Windows:

```bash
docker run --name ubuntu --rm -it -p 80:80 -v "%CD%/website:/web" ubuntu:20.04
```

## Instalar apache y PHP8

Actualizar los paquetes disponibles:

```bash
apt update
```

Evitar [que el paquete tzdata se quede parado](https://stackoverflow.com/a/44333806/14378620):

```bash
DEBIAN_FRONTEND=noninteractive apt-get install -y tzdata
ln -fs /usr/share/zoneinfo/Europe/Madrid /etc/localtime
dpkg-reconfigure --frontend noninteractive tzdata
```

Instalar Apache y PHP:

```bash
apt install software-properties-common -y
add-apt-repository ppa:ondrej/php -y
apt install php8.0 libapache2-mod-php8.0 -y
```

Instalar [extensiones adicionales](https://www.php.net/manual/es/extensions.alphabetical.php):

```bash
apt install php8.0-mysql php8.0-gd -y
```

## Arranque del servicio

Arrancar el servicio en segundo plano, cuando estamos en modo interactivo:

```bash
service apache2 start
```

Hacer que el contenedor [arranque el servicio](https://stackoverflow.com/a/44377561/14378620) en primer plano.

## Publicar en Docker Hub

[Referencia](https://www.tutorialspoint.com/publishing-a-docker-image-on-dockerhub).

```bash
docker login
docker images | grep apache
docker tag practica-docker_apache widemos/practica-docker:latest
docker push widemos/practica-docker:latest
```

[Imagen publicada](https://hub.docker.com/r/widemos/practica-docker).
