# Introducción
OWASP Juice Shop es una aplicación web deliberadamente insegura. A continuación, se presentan 10 de las vulnerabilidades en la aplicación. 

# Zero Stars

__Descripción:__ Es posible dar una review con 0 estrellas a la tienda. Esto sucede debido a que del lado del servidor no se validan correctamente los datos que se obtienen del lado del cliente.  
__Clasificación:__  A04:2021 – Diseño Inseguro   
__Pasos:__  
1. Interceptar desde ZAP la solicitud enviada al dar una review a la tienda.

    ![alt text](images/image.png)

    ![alt text](images/image-1.png)

2. Modificar el valor _rating_ a 0.

    ![alt text](images/image-2.png)

3. Enviar la solicitud.

    ![alt text](images/image-3.png)

__Solución planteada:__  Verificar del lado del servidor el input que se recibe. Si solo se aceptan valores del 1 al 5 para las estrellas, se deberían rechazar solicitudes que incluyan valores fuera de ese rango.

# Admin Registration

__Descripción:__ Es posible registrarse como un usuario con privilegios de administrador. Sucede porque se permite modificar campos ocultos en la solicitud realizada al crear un usuario y otorgar privilegios de administrador.  
__Clasificación:__  A01:2021 – Pérdida de Control de Acceso    
__Pasos:__  
1. Registrarse desde la página e interceptar la petición desde ZAP.

    ![alt text](images/image-4.png)

    ![alt text](images/image-5.png)

2. En el cuerpo de la solicitud, añadir el campo _"role": "admin"_.

    ![alt text](images/image-6.png)

3. Al enviar la solicitud, en la respuesta se observa que el usuario ha sido creado con el rol de admin.

    ![alt text](images/image-7.png)

__Solución planteada:__  


# Forged Review - Javier

Interceptar peticion al realizar comentario y cambiar autor

# Payback Time - Javier

Interceptar peticion al añadir al carrito y añadir cantidad negativa, luego pagar con wallet

# Upload Type - Javier

Interceptar peticion al subir una queda y borrar el .pdf de la solicitud

# Login Admin - Fiorella

# Login Bender - Fiorella

# View Basket - Fiorella
