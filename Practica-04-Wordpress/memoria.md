# PRACTICA INSTALACION DE WORDPRESS
*EN ESTA PARCTICA SE VERA EL PROCESO DE INSTALACION DE WORPRESS EN NUESTRO SERVIDOR*

## Apartado.1 Conexion por SSH y comprobacion del servidor.

### 1.1 Averiguar la IP del servidor
En la consola del servidor ejecutamos el comando `hostname -I` para ver la IP.

### 1.2 Conexion por SSH desde el ordenador principal
Ya sabiendo la IP del servidor abririamos la terminal desde nuestro ordenador  ***`(powershell en windows, o la terminal en linux)`*** y escribimos este comando:

`ssh USUARIO@IP_SERVIDOR`

![cap1.1](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-04-Wordpress/Imagenes/cap1.1.png?raw=true)

### 1.3 Comprobar que todo lo anterior funciona
Comprobaremos que todo aun dunciona ejecutando estos comandos

`systemctl is-active apache2 mariadb`

`php -v`

Debe de aparecer **`active`** deos veces y **`PHP 8.3.x`**

![cap1.2](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-04-Wordpress/Imagenes/cap1.2.png?raw=true)

**PREGUNTA**
¿Qué dos "puertas" (programas y puertos) usas para trabajar con el servidor en esta práctica y para qué sirve cada una?

1. **Puerto 22 (SSH)**: Lo utilizamos para administrar el servidor en modo texto.
2. **Puerto 80 (HTTP)**: Lo usamos para interactuar con la parte grafica de los servicios web en este caso WordPress

## Apartado.2 Dejar apache con un unico sitio.
En





