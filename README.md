# Configuración varias características nginx | Sergio De La Iglesia Lorenzo

1. Restricción de acceso por IP

La denegación de acceso por IP se debe hacer en el archivo "nginx.conf", en el que hay que añadir varias opciones. En el apartado "server" y "http", hay que poner las siguientes líneas:

```
auth_basic "Secure area - Authentication required";
auth_basic_user_file /etc/nginx/.htpasswd;

```

Esto lo que hace es mostrar el mensaje entrecomillado por pantalla a la hora de realizar el inicio de sesión y establecer el archivo con el hash en el que se van a recoger las contraseñas.

2. Autenticación básica

La autenticación básica de nginx consiste en crear una serie de cuentas de usuario a partir de las cuales solo se puede acceder al contenido del servidor. En caso de que este sea fallido, no se mostrará nada por pantalla. para añadir los usuarios al nginx, se hace creando un fichero "htpasswd" para un nombre, estableciendo el nombre de usuario y la contraseña. Para hacerlo de forma correcta, el comando se ejecuta de la siguiente forma: 

```
htpasswd -c /etc/nginx/.htpasswd name_user
```
Tras introducir este comando, se nos pedirá la contraseña, a la cual se le aplicará un hash para hacerla oculta. Para añadir más cuentas, se usa el mismo comando sin el parámetro "-c" y con el nuevo nombre de usuario junto a la contraseña a posteriori.

Lo siguiente que hay que hacer es cambiar la configuración del servidor, insertando la siguiente información tanto en el apartado "http" como en el apartado "server ... location" para que se aplique correctamente.
```
auth_basic "Mensaje de la autenticación requerida";
auth_basic_user_file /etc/nginx/.htpasswd;
```
Por último, queda reiniciar el servicio

3. Redirección del error 404

Para redirigir el error 404 a otra página, hace falta añadir información de forma directa en el fichero "/etc/nginx/sites-avaliable/default" y añadir la siguiente información a este:

```
error_page 404 http://pagina_a_la_que_se_redirige;
```
