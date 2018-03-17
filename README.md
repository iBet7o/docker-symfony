Docker Symfony (PHP7.1-FPM - NGINX - MySQL)
=====

## Dependencias

 * [docker-composer (>= 1.7)][1]

[1]: https://docs.docker.com/compose/

## Instalación

1. Crear un archivo llamado `.env` en base al archivo `.env.dist` y cambiar el valor de las variables.

    ```bash
    cp .env.dist .env
    ```

2. Crear el archivo vhost en base al archivo `nginx/vhosts/my-site-dev.conf.dist` y ajustar el contenido al sitio.

    ```bash
    cp nginx/vhosts/my-site-dev.conf.dist nginx/vhosts/my-site.dev.conf
    ```

3. Construir y ejecutar los contenedores en segundo plano.

    ```bash
    $ docker-compose build

    $ docker-compose up -d
    ```

4. Actualizar el archivo hosts del sistema anfitrión.

    ```bash
    # Obtener la ip asignada a docker
    $ docker network inspect bridge | grep Gateway

    $ sudo echo "171.17.0.1 my-app.dev" >> /etc/hosts
    ```

## Comandos útiles

```bash
# Entrar a un contenedor por bash
$ docker-compose exec [php|nginx] bash

# Composer (ej. composer update)
$ docker-compose exec php composer update

# Recuperar una dirección IP (ej. el contenedor nginx)
$ docker inspect --format '{{ .NetworkSettings.Networks.dockersymfony_default.IPAddress }}' $(docker ps -f name=nginx -q)
$ docker inspect $(docker ps -f name=nginx -q) | grep IPAddress

# MySQL cli
$ docker-compose exec db mysql -uroot -p"root"

# Comprobar el consumo de la CPU
$ docker stats $(docker inspect -f "{{ .Name }}" $(docker ps -q))
```
