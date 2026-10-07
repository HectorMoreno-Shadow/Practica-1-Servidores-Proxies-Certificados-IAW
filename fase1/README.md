# Fase 1

## DockerFile

```
FROM httpd:2.4

COPY ./vhosts/00-default.conf /usr/local/apache2/conf/00-default.conf
COPY ./vhosts/marca1.conf /usr/local/apache2/conf/marca1.conf
COPY ./vhosts/marca2.conf /usr/local/apache2/conf/marca2.conf

RUN echo "Include conf/00-default.conf" >> /usr/local/apache2/conf/httpd.conf
RUN echo "Include conf/marca1.conf" >> /usr/local/apache2/conf/httpd.conf
RUN echo "Include conf/marca2.conf" >> /usr/local/apache2/conf/httpd.conf

EXPOSE 80

CMD ["httpd", "-D", "FOREGROUND"]
```

`FROM httpd:2.4`: Define la imagen base de **Apache HTTP Server (versión 2.4)** sobre la que se construirá nuestro contenedor.

`COPY ./vhosts/00-default.conf /usr/local/apache2/conf/00-default.conf`: Copia la regla por defecto desde nuestra máquina local al directorio de configuración interno de Apache dentro del contenedor.

`COPY ./vhosts/marca1.conf /usr/local/apache2/conf/marca1.conf`: Copia la configuración del Virtual Host de **Marca 1** al contenedor.

`COPY ./vhosts/marca2.conf /usr/local/apache2/conf/marca2.conf`: Copia la configuración del Virtual Host de **Marca 2** al contenedor.

`RUN echo "Include conf/00-default.conf" >> /usr/local/apache2/conf/httpd.conf`: Añade al final del archivo principal de Apache (`httpd.conf`) la directiva para incluir la regla por defecto.

`RUN echo "Include conf/marca1.conf" >> /usr/local/apache2/conf/httpd.conf`: Vincula el archivo de **Marca 1** en el `httpd.conf` principal para que Apache reconozca el dominio `marca1-i.practica`.

`RUN echo "Include conf/marca2.conf" >> /usr/local/apache2/conf/httpd.conf`: Vincula el archivo de **Marca 2** en el `httpd.conf` principal para que Apache reconozca el dominio `marca2-p.practica`.

`EXPOSE 80`: El contenedor escuchará peticiones en el puerto HTTP estándar (puerto 80).

`CMD ["httpd", "-D", "FOREGROUND"]`: Comando principal para arrancar el proceso de Apache en primer plano (*foreground*), evitando que el contenedor Docker se apague tras iniciarse.

## Docker-compose.yml

```
version: '3.8'

services:
  servidor_hector_apache:
    build: .
    container_name: contenedor_nimbusdocs
    ports:
      - "8080:80"
    volumes:
      - ./web_marca1_i:/usr/local/apache2/htdocs/marca1
      - ./web_marca2_p:/usr/local/apache2/htdocs/marca2
    restart: unless-stopped
```

`version: '3.8'`: Especifica la versión del formato del archivo de Docker Compose.

`services:`: Define la lista de contenedores y servicios que forman parte de la aplicación.

`servidor_hector_apache:`: Nombre interno del servicio dentro de la red de Docker Compose.

`build: .`: Indica a Docker Compose que debe construir la imagen utilizando el `Dockerfile` ubicado en el directorio actual (`.`).

`container_name: contenedor_nimbusdocs`: Asigna un nombre fijo e identificable al contenedor en el sistema.

`ports:`: Mapea los puertos entre la máquina anfitriona y el contenedor.

  `"- 8080:80"`: Redirige el puerto `8080` de la máquina local al puerto `80` interno de Apache en el contenedor.

`volumes:`: Mapea carpetas de la máquina local hacia el contenedor para permitir la edición de la web en tiempo real sin reiniciar el servicio.

  `- ./web_marca1_i:/usr/local/apache2/htdocs/marca1`: Monta el contenido del sitio web de Marca 1.

  `- ./web_marca2_p:/usr/local/apache2/htdocs/marca2`: Monta el contenido del sitio web de Marca 2.

`restart: always` -> El contenedor se reiniciará siempre que se detenga (por un error, un reinicio del sistema o incluso si lo detienes tú manualmente con docker stop y luego reinicias el servicio de Docker)