# Configuración varias características nginx | Sergio De La Iglesia Lorenzo

1. Restricción de acceso por IP

La denegación de acceso por IP se debe hacer en el archivo "nginx.conf", en el que hay que añadir varias opciones. En el apartado "server" y "http", hay que poner las siguientes líneas:

```
auth_basic "Secure area - Authentication required";
auth_basic_user_file /etc/nginx/.htpasswd;

```

Esto lo que hace es mostrar el mensaje entrecomillado por pantalla a la hora de realizar el inicio de sesión y establecer el archivo con el hash en el que se van a recoger las contraseñas.
2. Red propia interna para todos los contenedores

Crear una red interna en la que almacenar tanto el servidor como el cliente DNS se hace mediante el archivo de creación de los contenedores de docker-compose. Dentro de este, hay que añadir el apartado "networks", asignarle el nombre que se quiera y la IP de la red. Tras haberla creado, ya en el apartado del cliente, hay que añadir también en el apartado "networks" el nombre de la subred hecha anteriormente. No hace falta asignar una IP ya que estp se hará por DHCP. El resultado sería así: 

```
version: "3.3"
services:
  asir_bind9:
    container_name: asir_bind9
    image: internetsystemsconsortium/bind9:9.16
    volumes:
      - /home/asir2a/Escritorio/SRI/bind9/conf:/etc/bind
      - /home/asir2a/Escritorio/SRI/bind9/zonas:/var/lib/bind
    networks:
      bind9_subnet:
        ipv4_address: 10.1.0.254 
  asir_cliente:
    container_name: asir_cliente
    image: alpine
    networks:
      - bind9_subnet
    stdin_open: true # docker run -i
    tty: true        # docker run -t
    dns:
      - 10.1.0.254   # el contenedor dns server 
networks:
  bind9_subnet:
    external: true
```

3. IP fija en el servidor

Para poner la IP fija en el servidor, hay que asignarla en el apartado "networks", como ya hice en el apartado anterior, siendo en este caso la 10.1.0.254.
