En este caso instalaremos EMQX Enterprise en el servicio ECS de AWS, y crearemos una base de datos en el servicio RDS de AWS.

1. Creamos el container dentro de ECS. Como requerimiento previo se dieron permisos a nuestros usuarios IAM.

    Se crearon dos para ver los recursos del hardware necesita para correr. Para ambos se usó EC2 en su composición y monitoreo debido a que EMQX no soportaba Fargate. 
      - El primer cluster tenía como instancia predeterminada t3.micro (EC2V2), mientras que el segundo t2.micro (EC2T2MICRO).
        - EC2V2: 2 uCPU y 512 mb de RAM
        - EC2T2MICRO: 1 uCPU y 512 mb de RAM
      - Para ambos le dimos a la aplicación para que use le 100% de los recursos
      
      <img width="1208" alt="image" src="https://user-images.githubusercontent.com/40177903/128656684-e47580e4-13b0-48bc-aba3-81a4cdddcd2e.png">
    
2. Crear un Task definition

    Se creó un task, donde se le daba al container los parámetros del hardaware a utilizar. Se usó una imagen de Docker Hub de la aplicación "EMQX enterprise", con versión 4.3.1. Cabe resaltar que se seleccionó este y no el común debido a que se podía modificar las conexiones de la base de datos, cosa contrario con el común (EMQX broker). Se necesitaba esto ya que se quería conectar con una base de datos creado en RDS. 
    
    Cabe resaltar que la imagen usada no es del mismo EMQX, sino que se subió una imagen modificando los documentos de la aplicación. Esto se hizo en el deployment local, ya que ECS no tiene un CLI para entrar al mismo container (Hay un servicio, sin embargo es muy complejo activarlo). Se modificó el recurso del database creado en RDS, con el fin de tener una base de datos donde se guarden los mensajes recibidos.
    ```
    docker pull marcobailon/emqx-ee
    ```
    - Task definition
    <img width="1190" alt="image" src="https://user-images.githubusercontent.com/40177903/128657082-9e4b010c-3166-413e-9b62-5116ea3035bc.png">
    - Container
    <img width="1096" alt="image" src="https://user-images.githubusercontent.com/40177903/128657095-65745804-1ab9-41f9-ae9e-6e762497c6a6.png">

3. Se une y se ejecuta el task definition a ambos clusters
    Como ejemplo, se muestra del cluster EC2V2
    <img width="1209" alt="image" src="https://user-images.githubusercontent.com/40177903/128657164-c219187f-53a4-447b-b2cd-c01e46e92b16.png">

4. Correr la IP generada
    - Usuario: admin, contraseña: public
    <img width="1030" alt="image" src="https://user-images.githubusercontent.com/40177903/128657596-9435fdaa-2145-46b5-878f-5b470297e510.png">

    <img width="1426" alt="image" src="https://user-images.githubusercontent.com/40177903/128657443-f25ee788-db1c-4905-8154-af97ea76db94.png">
    
5. Configuración de EMQX con nuestras herramientas

    5.1 Rule Engine - Resources
    <img width="1183" alt="image" src="https://user-images.githubusercontent.com/40177903/128657823-f4d997c6-5f21-498d-8663-50c7455bdcc5.png">
   
   5.2 Rule Engine - Rule
    <img width="1321" alt="image" src="https://user-images.githubusercontent.com/40177903/128657848-b6a866ec-5f09-4d4e-9d81-3ac4b15ad8bc.png">
   
   5.3 Tool - Websocket
    <img width="1220" alt="image" src="https://user-images.githubusercontent.com/40177903/128657873-931cf0be-43ae-4a01-8766-ae0ebb9f8cb1.png">
        - Para ver todos los topics del QoS 0
        <img width="1216" alt="image" src="https://user-images.githubusercontent.com/40177903/128657886-63378220-256c-4c12-9c0d-b222d098d903.png">

    
