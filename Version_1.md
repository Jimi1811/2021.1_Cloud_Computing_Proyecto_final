En este caso instalaremos EQMX Enterprise en Docker dentro de una laptop física, y crearemos una base de datos en AWS.

1. Obtenemos la imagen
    - Desde **[Docker Hub](https://hub.docker.com/r/emqx/emqx)**

        ```
        $ docker pull emqx/emqx:v4.0.0
        ```
2. Descargamos la imagen de Docker desde **[emqx.io o](https://www.emqx.io/downloads/broker?osType=Linux)** **[Github](https://github.com/emqx/emqx/releases) y lo corremos de forma manual**

        ```
        $ wget -O emqx-docker.zip https://www.emqx.io/downloads/broker/v4.0.0/emqx-docker-v4.0.0-alpine3.10-amd64.zip
        $ unzip emqx-docker.zip
        $ docker load < emqx-docker-v4.0.0
        ```

3. Iniciamos el contenedor docker

    ```
    $ docker run -d --name emqx -p 1883:1883 -p 8081:8081 -p 8083:8083 -p 8883:8883 -p 8084:8084 -p 18083:18083 emqx/emqx:v4.0.0
    ```
