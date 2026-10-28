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
En la practica anterior creamos dos sitios web y desactivamos el por defecto ahora debemos desactivar estos dos sitios web y volver a activar el por defecto

### 2.1 Cambiar los sitios activos
usaremos los comando de configuracion.

![cap2.1](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-04-Wordpress/Imagenes/cap2.1.png?raw=true)

*`configtest` debe de responder con `syntax ok`*

en caso de que salga un error como este:

`Could not reliably determine the server's fully qualified domain name, using fd17:625c:f037:2:a00:27ff:fedc:449. Set the 'ServerName' directive globally to suppress this message `

se soluciona asi

![cap2.2](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-04-Wordpress/Imagenes/cap2.2.png?raw=true)

### 2.2 comprobar que solo hay un sitio activo
usaremos los comandos
`ls /etc/apache2/sites-enabled/ `
`grep DocumentRoot /etc/apache2/sites-enabled/000-default.conf `

![cap.2.3](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-04-Wordpress/Imagenes/cap2.3.png?raw=true)

*La primera orden debe mostrar solo 000-default.conf, y la segunda, DocumentRoot /var/www/html. *

y asi deberia de verse la pagina de apache:

![cap2.4](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-04-Wordpress/Imagenes/cap2.4.png?raw=true)

**PREGUNTA**
¿Qué diferencia hay entre desactivar un sitio con a2dissite y borrar sus ficheros? ¿Por qué ahora ves la página por defecto de Apache y antes veías sitio1? 

- Desactivar un sitio solo hace que apache no lea su fichero pero borrarlos elimina su contenido total.
- Ahora vemos la pagina default porque esta en la ruta principal de `DocumentRoot` mientras que antes el trafico estaba dirigido a sitio1.

## Apartado.3 crear la base de datos de WordPress
Worpress necesita un sitio donde almacenar todo y esa sera la base de datos de MariaDB

|QUE CREAMOS  |NOMBRE       |PARA QUE        |
|-------------|-------------|----------------|
|El armario (base de datos) |wordpress |Donde WordPress guardará todos sus datos |
|La llave (usuario) |wp_user |Con lo que WordPress entrará en el armario |
|El permiso |Todos, pero solo sobre wordpress |Esa llave abre ese armario y ningún otro |

Ahora entraremos a MariaDB ejecutando `sudo mariadb ` y despues ponemos esto dentro:

`CREATE DATABASE wordpress; `:Crea el armario 

`CREATE USER 'wp_user'@'localhost' IDENTIFIED BY 'ClaveSegura_2026'; `:Crea la llave y le pone contraseña 

`GRANT ALL ON wordpress.* TO 'wp_user'@'localhost'; `:Da permiso a la llave para usar solo el armario wordpress 

![cap3.1](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-04-Wordpress/Imagenes/cap3.1.png?raw=true)

Comprobamos que la llave funciona. Entrando con el nuevo usuario (pedirá la contraseña):

![cap3.2](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-04-Wordpress/Imagenes/cap3.2.png?raw=true)

**PREGUNTA**
 ¿Por qué crees que WordPress usa su propio usuario wp_user con permiso solo sobre su base y no el usuario root de MariaDB?

-Por motivos de seguridad (principio de mínimo privilegio). Si un atacante llegara a comprometer las credenciales de la base de datos de WordPress, solo tendría acceso a la base de datos wordpress y no al usuario root, que tiene control total sobre todo el sistema de bases de datos del servidor.

## Apartado.4 descargar e instalar WordPress

### 4.1 descargar y descomprimir wordpress

`sudo apt install wget -y `: Instala la herramienta wget

`cd /tmp `: Cambia el directorio de trabajo actual a la carpeta temporal del sistema (/tmp)

`wget https://wordpress.org/latest.tar.gz `: Descarga el paquete oficial con la última versión de WordPress comprimido en formato .tar.gz directamente desde los servidores de WordPress.org a nuestro directorio actual (/tmp).

`tar -xzf latest.tar.gz `: Descomprime y extrae el archivo comprimido latest.tar.gz

`ls wordpress `: Lista el contenido de la carpeta recién extraída (wordpress)

wget descarga el fichero, y tar -xzf lo descomprime y crea la carpeta wordpress. Verás ficheros como index.php, wp-admin, wp-content y wp-config-sample.php.

### 4.2 colocar la carpeta en su sitio
Primero quitamos la página de bienvenida de Apache (si no, tendría prioridad sobre WordPress) y después copiamos todo el contenido de WordPress:

`sudo rm /var/www/html/index.html `: Elimina con permisos de superusuario el archivo HTML por defecto que Apache crea en su carpeta raíz

`sudo cp -a /tmp/wordpress/. /var/www/html/ `: Copia todo el contenido (.) de la carpeta descomprimida de WordPress que estaba en temporal (/tmp/wordpress/) hacia la carpeta raíz pública de Apache (/var/www/html/)

`rm -r /tmp/wordpress /tmp/latest.tar.gz `: Limpia el sistema borrando la carpeta temporal de WordPress y el archivo comprimido .tar.gz que descargamos en /tmp

`ls /var/www/html `: Lista el contenido de la carpeta raíz de Apache (/var/www/html) para verificar visualmente que todos los archivos de WordPress

### 4.3 darle los permisos correctos.
 Apache se ejecuta con el usuario www-data, y WordPress necesita poder escribir (para guardar imágenes, instalar plugins, crear su configuración...):

 `sudo chown -R www-data:www-data /var/www/html `

 `sudo find /var/www/html -type d -exec chmod 755 {} \; `

 `sudo find /var/www/html -type f -exec chmod 644 {} \; `

 `ls -ld /var/www/html `

 ![cap4.45](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-04-Wordpress/Imagenes/cap4.5.png?raw=true)

*permisos de wordpress*

![cap4.3](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-04-Wordpress/Imagenes/cap4.3.png?raw=true)

*contenido de la carpeta wordpress*

**PREGUNTA**
¿Por qué hay que cambiar el propietario de la carpeta a www-data? ¿Qué podría fallar si siguiera siendo tu usuario?

-Hay que cambiar el propietario a www-data porque es el usuario con el que corre el servidor web Apache. Si se quedara con tu usuario de SMR, WordPress no podría escribir en el disco (lo que impediría subir imágenes, instalar plugins o actualizar temas de forma automática desde la interfaz gráfica).

## Apartado.5 terminar la instalacion en el navegador

|Pantalla     |Que hacer     |
|-------------|--------------|
|1. Idioma |Elige Español y pulsa Continuar |
|2. Bienvenida |Pulsa ¡Vamos a ello! |
|3. Datos de la base de datos |Rellena la tabla de abajo y pulsa Enviar |
|4. Todo en orden |Pulsa Iniciar la instalación |
|5. Información necesaria |Rellena la segunda tabla y pulsa Instalar WordPress |
|6. Éxito |Pulsa Iniciar sesión |

**Pantalla 3: datos de la base de datos**

![cap5.1](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-04-Wordpress/Imagenes/cap5.1.png?raw=true)

**Pantalla 5: datos de tu sitio**

![cap5.2](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-04-Wordpress/Imagenes/cap5.2.png?raw=true)

**Escritorio WordPress navegador**

![cap5.3](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-04-Wordpress/Imagenes/cap5.3.png?raw=true)
 
**PREGUNTA**
¿Qué partes de la instalación has hecho en la terminal y cuáles en el navegador? ¿Por qué el servidor no necesita tener interfaz gráfica?

-En la terminal preparamos el entorno (servicios, descarga de ficheros, creación de base de datos y permisos). En el navegador completamos el asistente visual de configuración y la gestión diaria de contenidos
-servidor no necesita interfaz gráfica porque consume muchos recursos innecesarios; las órdenes se ejecutan por texto y las páginas web se envían al navegador del cliente para que sea este el que dibuje la interfaz visual

## Apartado.6 Comprobar que wordpress funcione

### 6.1. La web pública. 
Abrimos http://IP_DEL_SERVIDOR. Debe verse el sitio con el título que elegimos. 

### 6.2. El panel de administración.
Abre http://IP_DEL_SERVIDOR/wp-admin e inicia sesión.

### 6.3. Publica una entrada de prueba.
En el panel: Entradas → Añadir nueva. Escribe un título y un texto (por ejemplo, tu nombre y la fecha), pulsa Publicar y comprueba que aparece en la web pública. 

![cap6.1](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-04-Wordpress/Imagenes/cap6.1.png?raw=true)

### 6.4. ¿Dónde ha guardado WordPress tu entrada?
 En la base de datos lo podemos comprobar en la terminal ejecutando `sudo mariadb wordpress -e "SHOW TABLES;" `

 ![cap6.2](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-04-Wordpress/Imagenes/cap6.2.png?raw=true)

### 6.5. Dónde está cada cosa:

|Que          |Donde            |
|-------------|-----------------|
|Los ficheros de WordPress |/var/www/html/ |
|La conexión con la base de datos |/var/www/html/wp-config.php (no lo compartas nunca: contiene la contraseña) |
|Temas, plugins e imágenes subidas |/var/www/html/wp-content/ (carpetas themes, plugins y uploads) |
|Las entradas, usuarios y ajustes |En la base de datos wordpress de MariaDB |

**PREGUNTA**
 ¿Dónde guarda WordPress las entradas que escribes: en ficheros del servidor o en la base de datos? ¿Cómo lo has comprobado? 

 -Las entradas se guardan en la base de datos (específicamente en la tabla wp_posts)

## Apartado.7 para que no se rompa si se cambia de IP.
WordPress apunta en su base de datos la dirección con la que lo instalaste. Si la IP de la máquina virtual cambia (por ejemplo, al reiniciarla), WordPress seguirá usando la antigua: la web se verá sin diseño y el panel te llevará a una dirección que ya no existe.

Con dos líneas le decimos: "usa siempre la dirección con la que te estén visitando"

`sudo nano /var/www/html/wp-config.php `

Busca la línea que empieza por /* That's all, stop editing! y encima suyo separado con una linea en blanco ponemos esto:

`define( 'WP_HOME',    'http://' . $_SERVER['HTTP_HOST'] ); `

`define( 'WP_SITEURL', 'http://' . $_SERVER['HTTP_HOST'] ); `

![cap7.1](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-04-Wordpress/Imagenes/cap7.1.png?raw=true)

guardamos y ahora ejecutamos esto `php -l /var/www/html/wp-config.php `

![cap7.2](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-04-Wordpress/Imagenes/cap7.2.png?raw=true)

## Apartado.8 Dos ajustes para trabajar cómodamente
Antes de empezar a trabajar con WordPress conviene hacer dos ajustes. Sin ellos, aparecen dos problemas muy típicos

### 8.1. Direcciones "bonitas" (enlaces permanentes).
primero hacemos que apaches pueda leer el fichero de configuracion.

`sudo nano /etc/apache2/conf-available/wordpress.conf `

y dentro colocamos estas lineas

      <Directory /var/www/html> 
      
          AllowOverride All 
      
      </Directory> 

![cap8.1](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-04-Wordpress/Imagenes/cap8.1.png?raw=true)

ahora activamos el modulo rewire (que ya deberia de estar activado)

![cap8.2](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-04-Wordpress/Imagenes/cap8.2.png?raw=true)

Después, en el panel de WordPress: Ajustes → Enlaces permanentes → Nombre de la entrada → Guardar cambios.

![cap8.3](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-04-Wordpress/Imagenes/cap8.3.png?raw=true)

y ahora se deveria de ver la direccion bonita

![cap8.4](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-04-Wordpress/Imagenes/cap8.4.png?raw=true)

### 8.2. Poder subir ficheros grandes. 
PHP, por defecto, solo deja subir ficheros de 2 MB. Muchos temas y plugins pesan más, y las fotos también. Subimos el límite a 64 MB:

    printf 'upload_max_filesize = 64M\npost_max_size = 64M\n' | sudo tee /etc/php/8.3/apache2/conf.d/99-wordpress.ini 
    sudo systemctl restart apache2 

![cap8.5](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-04-Wordpress/Imagenes/cap8.5.png?raw=true)

**PREGUNTA**
¿Por qué las direcciones "bonitas" dan error 404 si Apache no tiene permitido leer el fichero .htaccess? 

- Porque las direcciones bonitas dependen de que WordPress reescriba las URLs mediante el fichero .htaccess. Si Apache tiene prohibido leer o aplicar las reglas de ese archivo (AllowOverride None), ignorará las directivas de redirección y devolverá un error 404

## Apartado.9 ¿Qué pasa si MariaDB se para?
WordPress depende de MariaDB: sin base de datos no puede mostrar nada. Vamos a verlo. Detén MariaDB a propósito: 

`sudo systemctl stop mariadb `

Ahora entramos a la pagina web y saldra esto

![cap9.1](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-04-Wordpress/Imagenes/cap9.1.png?raw=true)

Ahora volvemos a arrancar  MariaDB

`sudo systemctl start mariadb `

`sudo systemctl start mariadb `

![cap9.2](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-04-Wordpress/Imagenes/cap9.2.png?raw=true)

![cap9.3](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-04-Wordpress/Imagenes/cap9.3.png?raw=true)


**PREGUNTA**
Cuando WordPress muestra "Error al establecer una conexión con la base de datos", ¿qué dos cosas comprobarías y en qué orden? 

1. Comprobaría si el servicio MariaDB está activo ejecutando systemctl is-active mariadb (y lo iniciaría con sudo systemctl start mariadb si está parado).
2. Comprobaría que las credenciales de acceso (usuario y contraseña) configuradas en el fichero wp-config.php siguen siendo correctas conectándome manualmente con mariadb -u wp_user -p wordpress.


### Apartado.10 Comprobacion final

![cap10.1](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-04-Wordpress/Imagenes/cap10.1.png?raw=true)

|Pieza     |Programa       |Estado         |
|----------|---------------|---------------|
|Linux |Ubuntu 24.04 |Instalado |
|Apache |Apache2, con un único sitio |Funcionando |
|MariaDB |MariaDB 10.11 |Funcionando, con la base wordpress |
|PHP |PHP 8.3 |Funcionando |
|WordPress |Instalado en /var/www/html |Funcionando y accesible desde el navegador |

|Dato        |Valor        |
|------------|-------------|
|Dirección de la web |http://IP_DEL_SERVIDOR |
|Panel de administración |http://IP_DEL_SERVIDOR/wp-admin |
|Usuario administrador |El que creaste en el apartado 5 |
|Contraseña |La que creaste (guárdala en privado) |


