# Trabajo en Clase 03: Introducción práctica a Docker y Docker compose con aplicación Java y MySQL

1. **Objetivo**

    * Comprender cómo Docker permite empaquetar aplicaciones y sus dependencias dentro de contenedores, y cómo Docker COmpose facilita la ejecución coordinada de múltiples servicios como una aplciación Java y una base de datos MySQL

    ## Actvidades

    * **Verificar instalación de Docker**

        ```bash
        docker --version
        docker info
        ```

        ![**Docker**](/img/docker.png)

    * **Primer contenedor de prueba**

        * Ejecutar:

            ```bash
            docker run hello-world
            ```
            * Este comadno descarga una imange de prueba, crea un contenedor y muestra un mensaje confirmando que Docker funciona correctamente

        ![**Contenedor Hello world**](/img/imagen_hello_world.png)

    * **Ejecutar un servidro web en un contenedor**

        ```bash
        docker run -d -p 8080:80 --name mi-nginx nginx
        ```
        ![**Ngix container**](/img/nginx_container.png)
    
        * Ingresar

            ```bash
            http://localhost:8080
            ```
        ![**Nginx**](/img/nginx.png)

    
    * **Ejecutar dos contenedores para comprender puertos**

        * Ejecutar

            ```bash
            docker run -d -p 8080:80 --name nginx1 nginx
            docker run -d -p 8089:80 --name nginx2 nginx
            ```
        
            ![**Dos contenedores de Nginx**](/img/dos_contenedores.png)

        * **Probar en navegador**

            ```bash
            http://localhost:8088
            http://localhost:8089
            ```

            ![**Nginx con cambio de puerto**](/img/nginx_en_puerto_8089.png)

            * Ambos contenedores usan internamente el puerto 80, pero el host necesita puertos distintos

    * **Explicacion detallada del mapeo de puertos**

        * En Docker, el mapeo de puertos sigue esta estructura

            ```bash
            puertoHots:puertoContenedor
            ```

            * Ejemplo:

                ```bash
                8080:80
                ```
                * Significa:

                    * **8080** puerto del computador (host)
                    * **80** puerto del contenedor

            * Entoces

                ```bash
                http://localhost:8088  --->  contenedor:80
                ```
                * Esto se hace proque el contenedor está aislado y su peurto interno no es visible automaticamente desde el host. Docker crea una redirección entre el puerto del computador y el puerto del contenedor

            
    ## Preguntas de reflexión

    1. **¿Cuál es la diferencia entre imagen y contendor?**

        * La imagen son la base de los contendores es decir un contenedores es una instacia activa de un contenedor la cual se le puede: 

            * Definir volumenes
            * Mapear puertos
            * Dar un nombre
            * Insertar variables 
            * Definir una red a la que esta 
            * si se ejecuta en modo interactivo o en segundo plano

    2. **¿Qué problema de ingenieria de software ayuda a resolver Docker?**

        * Docker ayuda a resolver problema de garantizar siempre la ejecucion en el mismo entorno tambien permite poder ejecutar independientemente del sistema operativo que se usa aunque al ser docker un vistualizardor de tipo 1 y usar el kernel de linux es poco profecional ejecutarlo desde windows pierdes recursos utilizando wls

    3. **¿Qué ventaja ofrece Docker Compose frente a ejecutar contenedores manualmente?**

        * Permite ejecutar multiples contenedores a traves de un unico comando tambien se supone que se puede definir dependencias y tiempos de arranque para que se levante de forma correcta aplicaciones complejas (microservicios) 

    3. **¿Por qué es importante usar volúmenes cuando se trabaja con bases de datos?**

        * Para poder tener almacenamiento persisitente es decir, si no se define un volumen si se apaga el contenedor este perdera los datos mientras que si se define un volumen se puede apagar y no se perderan los datos almenos que se elimine el volumen. (los volumenes puede ser atraves de docker o por medio de una ruta del host)
    
    5. **¿Qué significa el mapeo de puerto `8080:8080` y `3307:3306`?**

        * **8080:8080** Significa que se esta mapeando el puerto `8080` del contenedor al puerto `8080` del host es decir se esta mapeando el mismo puerto y por ende se esta permitiendo el acceso al contenedor a traves del puerto `8080` del host es decir si se esta ejecutando una aplicacion ejemplo: un backend en SpringBoot se podra acceder a los enpoints definidos en el a traves del host por medio de la ruta mas el puerto `8080`

        * **3307:3306** Esto significa que se esta mapeando el puerto del contenedor `3306` al puerto del host `3307` es decir se puede acceder a la base de datos a traves del puerto **3307** 

    