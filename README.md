1. Definición general del proyecto
  - Complejidad media
  
    Consideramos que el proyecto es de complejidad media porque logramos conectar tres instancias las cuales son: un dispositivo físico IOT (ESP8266), la aplicación EMQX enterprise y una base de datos alojada en AWS. Sin embargo, a pesar que logramos una escalabilidad vertical dentro del cluster, no hemos lograr una escalabilidad horizontal.

  - Con un container 
    
  - Procesamiento de datos en el monitoreo y dentro de la aplicación

      La aplicación EMQX es la encargada de administrar tanto los datos de salida como los de entrada. los de entrada en  base a tópicos y los de salida dependiendo de las reglas que se establezcan dentro de la aplicación.

2. Características esenciales

  - Deployment local y en la nube de ECS
  - Modelo integrado en la nube: streaming dentro de EMQX enterprise
  - RDS como almacenamiento dinámico.

3. Escenarios de testeo
    - Soporta escalabilidad dentro del container 
        Al iniciar la aplicación se le otorga al contenedor alrededor de 180 M pero al enviar datos y que los reenvíe a la base de datos la capacidad que se le otorga sube a 185 M  y a lo máximo que llegó fue a 191 M.
        <img width="1113" alt="image" src="https://user-images.githubusercontent.com/40177903/128658187-e473f91d-a43f-4866-a367-ad1185f08884.png">

4. Crear escenarios de testeo con carga

    4.1 Herramientas de monitoreo: Dentro de ECS y dentro de EMQX
   
      La prueba se realizó alrededor de las 8 PM conectando alrededor de 8 dispositivos diferentes a la plataforma de EMQX enviando datos y dándole la orden de que envíe datos de uno de estos dispositivos a la base de datos.
        <img width="1051" alt="Screenshot 2021-08-08 at 20 58 12" src="https://user-images.githubusercontent.com/40177903/128658271-ae4d1bf8-9905-4926-9c0c-4b9d0ef6b191.png">

      Se puede observar en el monitoreo realizado por EC2 de la instancia que hay un incremento en el uso de recursos que necesita el cluster.
        <img width="1131" alt="Screenshot 2021-08-08 at 20 58 45" src="https://user-images.githubusercontent.com/40177903/128658293-c647b273-9d15-49f1-8d79-f77bc186f6cc.png">
      
      En el monitoreo realizado por ECS también podemos observar que hay un incremento notable en el uso de recursos tanto en CPU como en memoria.
        <img width="973" alt="Screenshot 2021-08-08 at 20 57 26" src="https://user-images.githubusercontent.com/40177903/128658322-1350ff90-c23f-48e9-8d89-e784128fb4d5.png">
        <img width="1180" alt="Screenshot 2021-08-08 at 20 56 22" src="https://user-images.githubusercontent.com/40177903/128658331-b76e4937-4946-444b-8899-2e4d91e26841.png">
        <img width="1184" alt="Screenshot 2021-08-08 at 20 56 03" src="https://user-images.githubusercontent.com/40177903/128658337-f6017815-69d8-4917-90d6-3b4f00f72889.png">
  
      En el monitoreo realizado por EMQX podemos observar la cantidad de dispositivos conectados así como los mensajes recibidos y enviados por segundo. Podemos ver que hay varios mensajes caídos posiblemente por la configuración de QOS la cual se puso en 0. La recepción y envío de datos de la aplicación se mantienen en todo momento estables y no se observó problemas en la recepción de estos datos en la base de datos.
        <img width="901" alt="image" src="https://user-images.githubusercontent.com/40177903/128658359-91ca06e6-72ea-42c7-bf48-27fef3a87831.png">

      Por último tenemos el monitoreo de la base de datos el cual nos muestra que no hay saturación en esta y que la llegada de datos es estable. En la prueba se vió un incremento en la gráfica de escritura que es lo que se esperaba porque la aplicación EMQX enviaba datos aproximadamente cada 500 ms.

    4.2 Análisis de dos resultados        
        Los resultados obtenidos son los esperados, en el tema de estabilidad de la aplicación pudimos comprobar que esta puede soportar grandes cargas de datos teniendo ciertas pérdidas que se dan debido a la configuración de QoS pero que no tiene problemas al recibir y enviar instantáneamente los datos. Por parte de escalabilidad comprobamos que nos se hace muy necesario ya que EMQX es muy eficiente en cuanto a utilización de recursos.

5. Características extras (Dentro de EMQX)

    5.1 Seguridad: 
        EMQX nos brinda la posibilidad de restringir el acceso a la aplicación a usuarios y clientes por medio de una base de datos la cual creamos nosotros con los usuarios y clientes a los cuales nosotros queramos dar acceso.
  
    5.2 IoT: 	
        Al usar el protocolo de comunicación MQTT la transferencia de datos entre EMQX y el dispositivo IoT es muy rápida y eficiente ya que este protocolo está diseñado especialmente para realizar este tipo de comunicación.

6. Observaciones

    Al crear la instancia en ECS se nos permitió agregar un puerto para poder ingresar a nuestra aplicación. Sin embargo, al querer conectarnos mediante otro puerto, como el 1883 a EMQX , no podíamos. Esto se debe a que la instancia no tenía estos puertos aperturados, pero la tarea que estaba corriendo en la instancia sí los tenía.
      <img width="897" alt="image" src="https://user-images.githubusercontent.com/40177903/128658849-309c2866-7352-40bc-8678-6e55e5bd801c.png">
  
    Lo que sucedía era que por temas de seguridad EC2 el cual se encarga de la administración de todas las instancias no había generado los puertos que pusimos en la tarea por lo que tuvimos que abrirlas desde el apartado de seguridad de EC2.
      <img width="781" alt="image" src="https://user-images.githubusercontent.com/40177903/128658862-bb760a40-159d-4c7c-a2cd-609a5c75ed2a.png">

7. Entregable
    - Presentación: https://docs.google.com/presentation/d/1VkgT3GGIhv_EhXLhcmW4bn62_-OtVbTPjvwV7NfLxPA/edit#slide=id.ge4912cc5e7_1_0



