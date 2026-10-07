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

-  `sudo systemctl enable apache2`: *al ejecutar el comando hace que al arrancar el servidor el servicio de Apache se ejecute automaticamente*

![enable apache](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-02-apaches/imagenes/parte4.2.png?raw=true)

- `sudo systemctl disable apache2`: *este comando desactiva el arranque automatico del servicio de Apache al iniciar el servidor*

- `apache2ctl configtest`: *este comando comprueba si hay errores de sintaxis de la configuracion*























