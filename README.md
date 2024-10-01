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

__Solución planteada:__  No permitir que el cliente envie información relacionada con los roles de los usuarios. La asignación de roles debe estar controlada por el backend. Al registrar un usuario, el servidor debe asignar automáticamente el rol correcto.

# Forged Review - Javier

Interceptar peticion al realizar comentario y cambiar autor

__Descripción:__  
__Clasificación:__  
__Pasos:__  
__Solución planteada:__  


# Payback Time - Javier

Interceptar peticion al añadir al carrito y añadir cantidad negativa, luego pagar con wallet  
__Descripción:__  
__Clasificación:__  
__Pasos:__  
__Solución planteada:__  


# Upload Type - Javier

Interceptar peticion al subir una queja y borrar el .pdf de la solicitud  
__Descripción:__  
__Clasificación:__  
__Pasos:__  
__Solución planteada:__  


# Login Admin 
__Descripción:__  Existe una vulnerabilidad de inyección SQL en el sistema de inicio de sesión, ya que permite acceder como administrador sin conocer la contraseña, debido a que no se sanitizan correctamente los datos ingresados por el usuario.

__Clasificación:__  A03:2021 – Inyección 

__Pasos:__  
1. Acceder a la pantalla de iniciar sesión.

2. En el campo de "Email", ingresar la siguiente inyección sql:

    ![alt text](images/inyeccionSql.png)
   
Esta inyección funciona porque el operador OR true hace que la condición en la consulta SQL siempre sea verdadera. El uso del comentario (--) omite la validación de la contraseña que sigue a continuación.

4. En el campo de "Password" ingresar cualquier cosa.
   
5. Presionar el botón de Log in.
   
   ![alt text](images/loginAdmin.png)
   
   
__Solución planteada:__  Para prevenir inyecciones SQL, es fundamental utilizar Prepared Statements. Los Prepared Statements parametrizan las consultas SQL, evitando que el input del usuario se integre directamente en la consulta SQL. De esta manera, cualquier dato ingresado se tratará como valor y no como código ejecutable.

# Login Bender 
__Descripción:__  Existe una vulnerabilidad de inyección SQL en el sistema de inicio de sesión, ya que permite acceder como el Bender sin conocer su contraseña, debido a que no se sanitizan correctamente los datos ingresados por el usuario. Además se está exponiendo información sensible en la página, ya que el correo del mismo estaba visible públicamente.

__Clasificación:__  A03:2021 – Inyecciones  
Acceder a la pantalla de inicio de sesión.

__Pasos:__  
1. Acceder a la sección de About Us.
2. Revisar las opiniones de los clientes.
   
![alt text](images/AboutUs.png)

3. Copiar el mail del bender.
4. Acceder a la pantalla de inicio de sesión.
5. En el campo de "email", ingresar la siguiente inyección SQL con el correo de Bender:
   
![alt text](images/inyec.png)

Esta inyección funciona debido al uso del comentario SQL (--), que omite la validación de la contraseña y fuerza a la consulta a tratar el usuario como si hubiera sido autenticado correctamente.

6. En el campo de "password", ingresar cualquier valor, ya que no será validado por la inyección.

7. Hacer clic en "Iniciar sesión".

![alt text](images/loginBender.png)

**Riesgo adicional:**
Además de la vulnerabilidad de inyección SQL, la exposición pública del correo electrónico de Bender constituye un riesgo de seguridad, ya que puede ser utilizado por atacantes para facilitar otros ataques, como phishing.

__Solución planteada:__

**Sanitización de las entradas:** Utilizar Prepared Statements para evitar la inyección SQL, ya que estos parametrizan las consultas, lo que impide que el input del usuario sea tratado como código ejecutable.

**Ocultar información sensible:** Evitar la exposición de correos electrónicos u otros datos personales de los usuarios en la interfaz pública.

# View Basket 
__Descripción:__ Existe una vulnerabilidad de pérdida de control de acceso, en la vista del carrito, ya que se pueden ver carritos de otros usuarios.

__Clasificación:__  A01:2021 – Broken Access Control

__Pasos:__  
1. Ingresar a la pantalla del Basket.
2. Abrir las herramientas de desarrollador del navegador (F12).
3. Acceder al Session Storage.
   
![alt text](images/cookiesSession.png)

4. Modificar el id del carrito (bid)
   
![alt text](images/cambioId.png)

5. Visualizar el carrito de otro usuario.
   
![alt text](images/cestaAjena.png)

![alt text](images/view.png)

__Solución planteada:__
Se debe realizar el control de acceso del lado del servidor, donde el atacante no puede modificarlo. El sistema debe verificar que el ID del carrito corresponde al usuario autenticado antes de permitir cualquier visualización o modificación.



# Manipulate Basket - Javier

Añadir item a carrito sin basketId, se va a generar objeto de item añadido con id pero no esta asociado a ningun carrito, realizar put request y añadir a carrito de otra persona  

__Descripción:__  
__Clasificación:__  
__Pasos:__  
__Solución planteada:__  

