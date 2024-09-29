# Introducción
OWASP Juice Shop es una aplicación web deliberadamente insegura. A continuación, se presentan 10 de las vulnerabilidades en la aplicación. 

# Zero Stars

__Descripción:__ Es posible dar una review con 0 estrellas a la tienda. Esto sucede debido a que del lado del servidor no se validan correctamente los datos que se obtienen del lado del cliente.  
__Clasificación:__  A03:2021 – Inyección??   
__Pasos:__  
1. Interceptar desde ZAP la solicitud enviada al dar una review a la tienda.

    _imagen_

2. Modificar el valor xx a 0.

    _imagen_

3. Enviar la solicitud.

    _imagen_

__Solución planteada:__  



# Login admin

# Login bender

# View basket
