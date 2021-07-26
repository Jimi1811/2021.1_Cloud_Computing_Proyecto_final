# Cloud_Computing_Proyecto_final
Este repositorio se usará para presentar el proyecto final del curso "Cloud computing" de la "Universidad de Ingeniería y Tecnología (UTEC)" en el ciclo 2021-1.
1. Datos del proyecto(Aplicación que se va a usar)

Para la recepción y gestión de datos recibidos de dispositivos IoT se usa el broker EMQX, siendo este la aplicación a desarrollarse. EMQX es un broker de mensajería MQTT (protocolo enfocado en el transporte de datos IoT) distribuido de código abierto y con alta escalabilidad.

1.1. Arquitectura
    EMQX hace posible la comunicación entre un dispositivo IoT hacía una máquina o cliente, siendo este receptor el que decida si se almacena o se gestiona los datos.
![1](https://user-images.githubusercontent.com/40177903/124541985-2b16d580-dde8-11eb-9835-58f4bc264d64.png)
1.2. Funcionalidad
![2](https://user-images.githubusercontent.com/40177903/124541995-310cb680-dde8-11eb-835f-8232cd5148c9.png)
    Publisher - subscriber: Recibe de un dispositivo (publisher) información y envía a un tercero ( Subscriber). Si no hay nadie recibiéndolo, los datos se pierden.

1.3 Referencias
- [https://docs.emqx.io/en/broker/v4.3/](https://docs.emqx.io/en/broker/v4.3/)
- [https://www.emqx.io/?spm=docs](https://www.emqx.io/?spm=docs)
- [https://mqtt.org/software/](https://mqtt.org/software/)
- [https://www.youtube.com/watch?v=hT5VPN6aEJM&t=126s](https://www.youtube.com/watch?v=hT5VPN6aEJM&t=126s)
- [https://www.youtube.com/watch?v=_SDLiqG6Rgo&list=PL8HAlytDEGGhA9kPfTCAlPvTgeoInKE5n](https://www.youtube.com/watch?v=_SDLiqG6Rgo&list=PL8HAlytDEGGhA9kPfTCAlPvTgeoInKE5n)
2. ¿Porque  escogió  esa  aplicación? 

Al ser de otra carrera, Ingeniería Mecatrónica, buscamos una aplicación que se relacione con containers, siendo Internet de las cosas (IoT) un punto de intersección entre ambos campos de la computación y automatización. En términos de escalabilidad, el uso de containers es necesario para poder recibir y poder procesar datos de cientos o miles de gadgets que envíen datos. Este con el fin de poder ahorrar en hardware así como poder gestionar y tener respuestas rápidas ante cualquier evento de tráfico que llegue a ocurrir. Por ese motivo es que elegimos el broker EDMX, ya que se puede aplica con containers y es capaz de gestionar hasta 100000 de conexiones.

3. ¿Qué  características  de  la  computación  en  nube pueden ser integradas en la aplicación? 
    1. On-demand self service

        Nuestro proyecto intentara brindar los recursos necesarios para que la aplicación funcione correctamente todo esto de acuerdo a lo que necesite el usuario (en nuestro caso dependerá de cuantos dispositivos IoT necesite).  

    2. Broad network Acces

        Para mostrar los datos obtenidos por los dispositivos IoT y administrados por EMQX necesitaremos hacer uso de una plataforma web en donde se le haga fácil para el usuario poder ver todos los datos. Está plataforma sería responsiva para poder así hacerla multiplataforma.

    3. Measured service

        Al usar AWS para nuestra base se podría cobrar al cliente por uso de almacenamiento en la nube.

4. Resumen de los pasos necesarios para su ejecución.

En este caso instalaremos EQMX en Docker.

4.1. Obtenemos la imagen
    - Desde **[Docker Hub](https://hub.docker.com/r/emqx/emqx)**

        ```
        $ docker pull emqx/emqx:v4.0.0

        ```
4.2 Descargamos la imagen de Docker desde **[emqx.io o](https://www.emqx.io/downloads/broker?osType=Linux)** **[Github](https://github.com/emqx/emqx/releases) y lo corremos de forma manual**

        ```
        $ wget -O emqx-docker.zip https://www.emqx.io/downloads/broker/v4.0.0/emqx-docker-v4.0.0-alpine3.10-amd64.zip
        $ unzip emqx-docker.zip
        $ docker load < emqx-docker-v4.0.0
        ```

4.3. Iniciamos el contenedor docker

    ```
    $ docker run -d --name emqx -p 1883:1883 -p 8081:8081 -p 8083:8083 -p 8883:8883 -p 8084:8084 -p 18083:18083 emqx/emqx:v4.0.0
    ```
 
5. Descripción del proyecto
- Uso de ESP

    Nos ayudará a emular y enviar datos del vehículo en cuestión.

    - Emulación de datos
        - Combustible
        - Rev.motor
        - Velocidad
        - Emisión de CO2
        - Localización
        - Presión del aceite
- Uso de EMQX
    - Creación de clientes y administración de estos
    - Recepción de datos y distribución a base de datos o AWS
- Uso de aws
    - Gestión de almacenamiento para datos obtenidos de EMQX
    - Se podría usar spark para la gestión de estos datos
- Uso de página web
    - Mostrar los datos en una interfaz a los usuarios
    - Capacidad de interactuar con la información
