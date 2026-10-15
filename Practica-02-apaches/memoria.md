# Paso.1 preparacion del sistema.
primero actualizamos la lista de paquetes del sistema con estos dos comandos.

- `sudo apt upgrade`
  
   ![sudo update](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-02-apaches/imagenes/parte1.png?raw=true)



- `sudo apt upgrade -y`

   ![sudo upgrade -y](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-02-apaches/imagenes/parte1.1.png?raw=true)

Ahora comprobaremos la vercion del sitema con el comando.

- `lsb_realese -a`

   ![lsb_realese](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-02-apaches/imagenes/parte1.2.png?raw=true)


# Paso.2 Instalacion de Apache.

Para la instalacion de los paquetes de apaches ejecutamos el siguiente comando.

 - `sudo apt install apache2 -y`

   ![apt install apache](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-02-apaches/imagenes/parte2.png?raw=true)

Luego de haber instalado todos los paquetes de apaches podemos comprobar si se hizo correctamente y comprovar la vercion con el comando siguiente.

- `apache2 -v`

  ![vercion apache](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-02-apaches/imagenes/parte2.2.png?raw=true)

**PREGUNTA**
*¿Qué paquetes adicionales se han instalado como dependencias? (pista: revisa la salida de apt).*

se instalan una variedad de paquetes adicionnales como: 

- **apache2-bin:** Contiene los ejecutables y binarios principales del servidor.
- **apache2-data:** Archivos de datos y plantillas del servidor.
- **apache2-utils:** Utilidades adicionales de administración (como htpasswd).
- **libapr1 y libaprutil1:** Librerías en tiempo de ejecución del proyecto Apache (Apache Portable Runtime).
- **mime-support:** Mapeo y definición de tipos de contenido MIME.


# Parte.3 Comprobacion del funcionamiento.


### 3.1 Estado del servicio
siempre que queramos comprobar el estao del servicio de apache2 si esta activo o apagado debemos de ejecutar este comando:

- `sudo sytemctl status apache2`

  ![status](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-02-apaches/imagenes/parte3.2.png?raw=true)


### 3.2 Puertos en escucha
En este apartado no enfocamos en saber y qu muestre que puertos del servidor estan preparados y listos para recibir peticiones web usando el siguiente comando:

- `sudo ss -tulpn | grep apache2`

![puertos](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-02-apaches/imagenes/parte3.3.png?raw=true)
*debes de tener el apaches encendido en el servidor para poder ver esto al ejecutar el comando*


### 3.3 Pruebas desde la terminal y navegador.
en este punto ejecutamos una prueba en la terminal y avegador para ver de que todo funciona correctamente para hacer las pruebas debemos utilizar el siguiente comando.

- `curl -I http://localhost`: *con esto comprobamos que el servidor si se puede ver en la red*

![pruba](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-02-apaches/imagenes/parte3.png?raw=true)


Ahora comprobaremos en el navegador y para ello abrimos el navegador y buscamos el siguiente enlace `http://IP_DEL_SERVIDOR` que en mi caso seria `http://192.168.56.21` y al buscar debe de salir esto.

![pagina red](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-02-apaches/imagenes/parte3.1.png?raw=true)


### 3.4 Comprobar si Firewall esta activo.
Para comprobar si el firewall esta activo usamos el comando `sudo ufw status` y deberia de salir si esta activo o no y tambien usaremos este otro comando `sudo afw allow 'Apache'` el cual hara que el firewall deje pasar las peticiones de apache

**PREGUNTA**
*¿Qué diferencia hay entre los perfiles Apache, Apache Full y Apache Secure?*

las diferencias entre los tres Apache son:

|Tipos Apache     |                                    Que hacen                                      |
|-----------------|-----------------------------------------------------------------------------------|
|Apache           | Apache solo activa el puerto 80 para trafico **HTTP NO CIFRADO**                  |
|Apache Secure    |Apache Secure activa el puerto 443 para trafico **HTTP CIFRADO**                   |
|Apache Full      |Apache Full abre ambos tipos de puertos permitiedo trafico cifrado como no cifrado |


# Paso.4 Comandos principales de administracion
Aqui se veran los comandos base para poder administrar el servidor con Apaches

- `sudo systemctl start apache2`: *este comando lo que hace es iniciar el servicio de Apaches*

![start apache](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-02-apaches/imagenes/parte4.png?raw=true)



- `sudo systemctl stop apache2`: *este comando se usa para parar/detener el servicio de Apache*

![stop apache](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-02-apaches/imagenes/parte4.1.png?raw=true)


- `sudo systemctl restart apache2`: *este comando lo que hace re reiniciar el servidor cortando el servicio para recargar paquetes o cambios en el servidor.*

-  `sdo systemctl reload apache2`: *este comando reinicia el servidor pero sin cortar el servicio permitiendo la recarga de paquetes o cambios en el servidor.*

-  `sudo systemctl enable apache2`: *al ejecutar el comando hace que al arrancar el servidor el servicio de Apache se ejecute automaticamente*.

![enable apache](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-02-apaches/imagenes/parte4.2.png?raw=true)

- `sudo systemctl disable apache2`: *este comando desactiva el arranque automatico del servicio de Apache al iniciar el servidor*.
  
![disable apache](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-02-apaches/imagenes/parte4.3.png?raw=true)

- `apache2ctl configtest`: *este comando comprueba si hay errores de sintaxis de la configuracion*.

![configtest](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-02-apaches/imagenes/parte4.4.png?raw=true)

- `apache2ctl -S`: *este comando sirve para ver los sitios web (virtual host) cargados*

![apache2ctl s](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-02-apaches/imagenes/parte4.5.png?raw=true)

- `apache2ctl -M`: *este comando permite ver la lista de modulos cargados del servidor*.

![apache2ctl m](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-02-apaches/imagenes/parte4.6.png?raw=true)

- `a2enmod / a2dismod`: *estos dos comandos permiten activar y desactivar modulos*.

- `a2ensite / a2dissite`: *estos dos comandos permiten activar y desactivar sitios web*.

- `a2enconf / a2disconf`: *estos dos comandos permiten activar y desactivar fragmentos de configuraciones*.

# Paso.5 Ficheros y Directorios importantes

En esta ruta de ficheros `ls -l /etc/apache2/` es donde se encontraran todos los ficheros de configuracion e importantes algunos de ellos siendo los siguientes.

|   ruta                                         |   Descripcion                                            |
|------------------------------------------------|----------------------------------------------------------|
|/etc/apache2/apache2.conf                       |Fichero de configuración principal                        |
|/etc/apache2/ports.conf                         |Puertos en los que escucha Apache                         |
|/etc/apache2/sites-available/                   |Sitios disponibles (definidos, no necesariamenteactivos)  |
|/etc/apache2/sites-enabled/                     |Sitios activos (enlaces simbólicos a sites-available)     |
|/etc/apache2/mods-available/ y mods-enabled/    |Módulos disponibles y activos                             |
|/etc/apache2/conf-available/ y conf-enabled/    |Fragmentos de configuración disponibles y activos         |
|/etc/apache2/envvars                            |Variables de entorno (usuario y grupo de ejecución,etc.)  |
|/var/www/html/                                  |Directorio raíz por defecto (DocumentRoot)                |
|/var/log/apache2/access.log                     |Registro de accesos                                       |
|/var/log/apache2/error.log                      |Registro de errores                                       |

Captura del contenido de `/etc/apache2/`.

![ruta](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-02-apaches/imagenes/parte5.png?raw=true)

**PREGUNTA**
¿Por qué Apache usa enlaces simbólicos entre los directorios *-available y *-enabled?

Apache usa enlaces simbolicos porque permiten una organizacion mas limpia y sencilla ya que en `*-available` guarda la configuracion de sitios y modulos pero para activarlos o desactivarlos solo hace falta borrar o crear su enlacesimbolico en `*-enable` y asi nunca se pierde la configuracion.


# Paso.6 Modificaciones tipìcas del servicio.
aqui haremos una modificaciones basicas al servicio pero primero haremos una copia de seguridad de la configuracion del servicio con el comando `sudo cp /etc/apache2/apache2.conf /etc/apache2/apache2.conf.bak`

### 6.1 cambiar la pagina de inicio
para cambiar el el titulo de la pagina de inicio usaremos este comando `echo "<h1>Servidor de TU NOMBRE</h1>" | sudo tee /var/www/html/index.html` y quedaria asi

![server](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-02-apaches/imagenes/parte6.5.png?raw=true)

### 6.2 cambiar el puerto de escucha
aqui debemos de entrar a un fichero y editar unos parametros 

1. primero entramos a este fichero `sudo nano /etc/apache2/ports.conf` y editamos el puerto de 80 a 8080

   ![escucha](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-02-apaches/imagenes/parte6.png?raw=true)

3. segundo entraremos a este segundo fichero `sudo nano /etc/apache2/sites-available/000-default.conf` y haremos lo mismo que en el anterior

   ![escucha2](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-02-apaches/imagenes/parte6.1.png?raw=true)

despues se guarda la configuracion y se reiniciara el servicio para aplicar los cambios
`sudo apache2ctl configtest`
`sudo systemctl reload apache2`

y asi se veria el resultado final al ejecutar el comando `curl -I http://localhost:8080`

![resutfinal](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-02-apaches/imagenes/parte6.2.png?raw=true)

 *despues se vuelve a dejar la configuracion normal ya que esto solo es un ejemplo practico*

 ### 6.3 cambiar el correo de administracion 
 
para cambiar el correo de administracion vamos a esta ruta  `sudo nano /etc/apache2/sites-available/000-default.conf` y en la linia de ServerAdmin ponemos el correo

![correo](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-02-apaches/imagenes/parte6.3.png?raw=true)

### 6.4 personalizar pagina de error

para cambiar el mensaje de error de la pagina usarmos este comando `echo "<h1>Error 404 - Pagina no encontrada en el servidor de TU NOMBRE</h1>" | sudo tee /var/www/html/404.html` y luego deberemos darle salida llendo nuevamente al fichero `sudo nano /etc/apache2/sites-available/000-default.conf` y añadieros la linea `ErrorDocument 404 /404.html` justo debajo de donde cambiamos el correo y el la pagina quedaria asi

![error](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-02-apaches/imagenes/parte6.4.png?raw=true)


# Paso.7 Modulos de Apache.
aqui se veran desitintos modulos que hay de apache

|Modulo               |Funcion                                                                                            |
|---------------------|---------------------------------------------------------------------------------------------------|
|mod_rewrite          |Permite la reescritura de URLs mediante reglas personalizadas                                      |
|mod_headers          |Permite manipular y añadir cabeceras de respuesta y petición HTTP                                  |
|mod_ssl              |Añade soporte para conexiones cifradas HTTPS mediante certificados SSL/TLS                         |
|mod_dir              |Gestiona los archivos índice por defecto                                                           |
|mod_alias            |Permite crear redirecciones de URL y alias hacia carpetas fuera de la raíz del sitio               |
|mod_auth_basic       |Permite restringir el acceso a directorios mediante autenticación HTTP básica                      |
|mod_status           |Genera una página web con estadísticas e información detallada sobre el estado del servidor.       |
|mod_php / PHP-FPM    |Ejecución de PHP                                                                                   |


**PREGUNTA**

*¿Qué ocurre si desactivas un módulo del que depende otro?*

Al desactivar un modulo clave del que dependen otros apache detectara un error de sintaxis fallando al reiniciar o recargar el servidor y seguira dando error hasta que se vuelva a activar el modulo.


# Paso.8 Varios sitios en el mismo servidor (Virtual Host).
aqui veremos la creacion de varios sitios web en el servicio de apache

### 8.1 crear dos sitios web
para crear los dos sitios usaremos el comando siguiente `sudo mkdir -p /var/www/sitio1 /var/www/sitio2` y procederemos a modificar su pagina de inicio como hicimos con la pagina principal usando 
`echo "<h1>Bienvenido a SITIO 1</h1>" | sudo tee /var/www/sitio1/index.html`
`echo "<h1>Bienvenido a SITIO 2</h1>" | sudo tee /var/www/sitio2/index.html`
quedando asi en consola del servidor.

![sitios](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-02-apaches/imagenes/parte8.png?raw=true)


### 8.2 creacion de fichero de modificacion
aqui debemos de crear un fichro de modificacion mismo que en el caso anterior usamos para modificar la pagina principal y para crearlo usaremos este comando `sudo nano /etc/apache2/sites-available/sitio1.conf` y ya estando dentro se pondrian los parametros quedando tal que asi

![sitios1](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-02-apaches/imagenes/parte8.2.png?raw=true)

*esto se aplica y se hace con ambos sitios solo cambiando el nombre de cada sitio respectivamente*

### 8.3 habilitacion de los sitios
ahora procederemos a activar los sitios web ejecutando el comando para activarlo que vismos en puntos mas arriba siendo este para activar ambos sitios en simultaneo `sudo a2ensite sitio1.conf sitio2.conf` despues comprobariamos la sintaxis de que todo esta bien con `sudo apache2ctl configtest` y recargariamos el servicio para aplicar los cambios.

*en caso que quieras desabilitar el sitio por defecto de apache se ejecutaria este `sudo a2dissite 000-default.conf` comando despues de activar los dos sitios web*

y para finalisar usariamos el comando `apache2ctl -S` para ver los sitios activos y ver que funcionan correctamente.

![sitios2](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-02-apaches/imagenes/parte8.4.png?raw=true)

### 8.4 resolucion de nombre 
ahora debemos de añadir una forma en que el servidor pueda traducir o saber aque pagina nos referimos al buscarla, para ello debemos de ir al fichero `sudo nano /etc/hosts` y añadir los dos nues sitios como `127.0.0.1   sitio1.local sitio2.local`

![sitios3](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-02-apaches/imagenes/parte8.5.png?raw=true)

luego comprobamos que se vean en consola usando estos dos comandos `curl http://sitio1.local` `curl http://sitio2.local` y deberia de verse asi.

![sitios4](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-02-apaches/imagenes/parte8.6.png?raw=true)

**PREGUNTA**

*¿Qué diferencia hay entre virtual hosts basados en nombre y basados en IP? ¿Cuál has usado?*

- **Virtual Host por nombre:** en este tipo multiples nombres o dominios web comparten la misma direccion ip y el mismo puerto, apache distingue a que sitio quiere acceder el cliente leyendo la cabecera HTTP que envia el navegador en la peticion.

-  **Virtual Host por IP:** en este tipo cada web tiene una direccion IP distinta en la misma maquina, apache redirige en funcion de la IP.

Y en este caso hemos usado el Virtual Host por nombre para sitio 1 y 2.


# Paso.9 Permisos y Propietarios.
en apache se pueden poner distintos niveles de permisos con comandos como estos 
`sudo chown -R www-data:www-data /var/www/sitio1` 
`sudo find /var/www/sitio1 -type d -exec chmod 755 {} \;` 
`sudo find /var/www/sitio1 -type f -exec chmod 644 {} \;`

y un ejemplo de *error 403* seria si al archivo `index.html` se le añadieran los permisos `chmod 000`

![error403](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-02-apaches/imagenes/parte9.png?raw=true)


- **Que sifnifican 755 y 644**
     - **755:** Para directorios, Propietario `rwx` (lectura, escritura, ejecución), Grupo `r-x` (lectura y ejecución), Otros `r-x` (lectura y ejecución). Permite a Apache y a los visitantes web entrar al directorio y listar su contenido.
     - **644:** Para archivos Propietario, `rw-` (lectura y escritura), Grupo `r--` (solo lectura), Otros `r--` *(solo lectura)*. Permite a Apache leer el archivo para servirlo en la web, pero evita que los visitantes o usuarios sin privilegios modifiquen su contenido.

- **Por qué NUNCA se deben usar permisos 777 en un servidor web:**
     - El permiso 777 da acceso completo de lectura, escritura y ejecución a todos los usuarios del sistema operativo. Si un sitio web sufre una vulnerabilidad de inyección de código, un atacante podría crear, modificar o borrar cualquier archivo web del servidor o ejecutar scripts maliciosos.
 
- **Por qué el contenido web no debe pertenecer a root ni ser escribible por todos:**
     - Si los archivos pertenecen al usuario root y se ejecutan procesos con sus privilegios, cualquier fallo de seguridad en una aplicación web le otorgaría al atacante control absoluto sobre todo el sistema operativo.
     - Tampoco deben ser escribibles por todos para evitar que un usuario sin privilegios o un servicio comprometido altere el código de las páginas web servidas.





