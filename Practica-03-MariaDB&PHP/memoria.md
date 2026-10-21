# Practica de Instalacion de MariaDB & PHP
En esta practica intslaremos MariaDB y PHP en el servidor de Apache


## Apartado-1 Preparacion.
Para preparar nuestro servidor web de apaches debemos de actualizar los paquetes del servidor y comprovar que apaches esta activo ejecutando estos coamdos en secuencia.

`sudo apt update`
`sudo apt upgrade -y`
`systemctl is-active apache2`

![cap1.2](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-03-MariaDB&PHP/Imagenes/captura1.2.png?raw=true)

*debe de salir el mensaje de activo*

## Apartado-2 Instalacion de MariaDB
Ahora procederemos a instalar MariaDB en sel server ejecutando el comando `sudo apt install mariadb-server -y`.
despues verificamos la vercion instalada con el comando `mariadb --version`

![cap1.3](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-03-MariaDB&PHP/Imagenes/captura1.3.png?raw=true)

## Apartado-3 Comprobacion del funcionamiento.

### Apartado-3.1 Estado del servicio
Comprobamos el estado del servicon de MariaDB con este comando `sudo systemctl status mariadb` y verificamos de que este activo con este otro `systemctl is-enabled mariadb`.

en el primero debe de salir **active (running)** y en el segundo debe de salir **enable**

![cap1.4](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-03-MariaDB&PHP/Imagenes/captura1.4.png?raw=true)

### Apartado-3.2 Entrar en la base de datos.
para acceder a la base de datos debemos de usar el comando `sudo mariadb` y si todo funciona bien debe de salir algo como esto:

![cap1.5](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-03-MariaDB&PHP/Imagenes/captura1.5.png?raw=true)

*escribe `exit` y pulsa enter para salir*

## Apartado-4 Comandos Principales.
Estos son los comandos que se usaran para manejar el servicio.

|Comando                                 |Funcion                       |
|----------------------------------------|------------------------------|
|sudo systemctl start mariadb            |Arranca el servicio           |
|sudo systemctl stop mariadb             |Detiene el servicio           |
|sudo systemctl restart mariadb          |Reinicia el servicio          |
|sudo systemctl status mariadb           |Muestra su estado             |
|sudo systemctl enable mariadb           |Arranca el servicio automaticamente al encender el equipo   |

## Apartado-5 ¿Que pasa si MariaDB esta parado?
Cuando instalemos Wordpress puede salir el error **"Error al establecer una conexión con la base de
datos"** y lo que deberemos hacer ew comprobar si MariaDB esta funcionando.

Por eso haremos una simulacion de que pasaria para ello pararemos MariaDB y veremos que pasa.

- **Paso.1:** Pararemos el servicio de MariaDB con el comando `sudo systemctl stop mariadb`
  
- **Paso.2:** Intentaremos acceder a la base de datos usando `sudo mariadb` y deberia de salirnos un error similar a este `ERROR 2002 (HY000): Can't connect to local server through socket '/run/mysqld/mysqld.sock' (2)` esto quiere decir que MariaDB no esta en marcha.

![cap.1.7](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-03-MariaDB&PHP/Imagenes/captura1.7.png?raw=true)

- **Paso.3:** Volvemos a poner en marcha MariaDB con `sudo systemctl start mariadb` y comprobamos que si se activo `systemctl is-active mariadb`

![cap.1.8](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-03-MariaDB&PHP/Imagenes/captura1.8.png?raw=true)

**PREGUNTA**
*Si WordPress no pudiera conectar con la base de datos, ¿qué dos comandos de este apartado usarías para saber si el problema es que MariaDB está parado?*

los comando que usaria seria `systemctl is-active mariadb`, `sudo systemctl status mariadb`  o `sudo mariadb` y me fijaria en el error para saver si es que esta inactivo.

## Apartado-6 Instalacion PHP.
aqui instalaremos PHP y las piesas necesarias para instalar WordPress donde debemos ejecutar este comando

    sudo apt install php libapache2-mod-php php-mysql php-curl php-gd php-mbstring php-xml php-zip php-intl -y

y verificamos la vercion con `php -v` y debe de mostrar `PHP 8.3.X`

![cap.2.1](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-03-MariaDB&PHP/Imagenes/captura2.1.png?raw=true)

despues de instalar PHP reiniciamos apache y comprobamos que ha cargado php con estos comandos.

`sudo systemctl restart apache2`
`sudo apache2ctl -M | grep -E "mpm|php"`

y debes de ver `mpm_perfork_module` y `php_module`

ahora comprobamos que PHP puede hablar con MariaDB ejecutando el cmando `php -m | grep -i mysql`.
y ahora devemos de ver que aparece `mysqli`, `mysqlnd` y `pdo_mysql` que son las piesas que usa WordPress.

![cap.2.2](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-03-MariaDB&PHP/Imagenes/captura2.2.png?raw=true)

Ahora comprobamos que Apache ejecuta PHP usando este comando `echo '<?php phpinfo();' | sudo tee /var/www/html/info.php` y ahora abrimeo el avegador web y buscamos  `http://IP_DE_TU_SERVIDOR/info.php` y debe de salir esto:

![cap.2.3](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-03-MariaDB&PHP/Imagenes/captura2.3.png?raw=true)

**IMPORTANTE**
Debemos de borrar el fichero de prueba usando `sudo rm /var/www/html/info.php` y comprobamos `ls /var/www/html`

**Pregunta**
*¿Por qué es peligroso dejar info.php en un servidor real? ¿Para qué sirve el paquete php-mysql?*

 -Es peligroso dejar info.php porque expone públicamente información confidencial y técnica del servidor (versiones exactas de PHP, sistema operativo, rutas absolutas, variables de entorno y módulos cargados), lo cual facilita enormemente la labor a atacantes para buscar vulnerabilidades específicas.

 -El paquete php-mysql es el conector indispensable que permite a las aplicaciones escritas en PHP (como WordPress) enviar consultas y comunicarse correctamente con el servidor de bases de datos MariaDB.


aqui veremos para que sirve cada paquete:

|Paquete            |Funcion               |
|-------------------|----------------------|
|php                |El lenguaje PHP       |
|libapache2-mod-php |Permite a Apache ejecutar PHP |
|php-mysql          |Permite a PHP hablar con MariaDB (imprescindible) |
|php-curl           |Conexiones a otros servidores (actualizaciones, plugins) |
|php-gd             |Tratamiento de imágenes |
|php-mbstring       |Manejo de textos con tildes y caracteres especiales |
|php-xml            |Lectura y escritura de ficheros XML |
|php-zip            |Instalación y actualización de plugins y temas |
|php-intl           |Idiomas y formatos regionales |

## Apartado-7 Comprobacion Final.
ahora comrpobaremos que todo este bien al usar este comando `systemctl is-active apache2 mariadb` y luego `php -v`

`is-active` debe de responder active dos veces.

![cap.2.5](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-03-MariaDB&PHP/Imagenes/captura2.5.png?raw=true)
 



