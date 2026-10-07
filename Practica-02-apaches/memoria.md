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
siempre que queramos comprobar el estao del servicio de apache2 ejecutaremos este comando:

- `sudo sytemctl status apache2`

  ![status](https://github.com/Walfariu/aplicaciones-web-practicas/blob/main/Practica-02-apaches/imagenes/parte3.2.png?raw=true)











