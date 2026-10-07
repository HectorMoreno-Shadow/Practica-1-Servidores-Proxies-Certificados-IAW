# VHOSTS

## Archivo - 00-default.conf

```
<VirtualHost *:80>
    ServerName default

    <Location "/">
        Require all denied
    </Location>

    ErrorLog "logs/default_error.log"
    CustomLog "logs/default_access.log" combined
</VirtualHost>
```

`<VirtualHost *:80>` -> Le dice a Apache que escuche las peticiones que lleguen por el puerto 80

`ServerName default` -> Nombre interno al servidor virtual. En este caso es el default ya que actuará como la regla por defecto (ya que es el primer archivo que lee apache porque empieza por "00-") para cualquier dominio que no coincida con ***marca1-i.practica*** ni ***marca2-p.practica***

`<Location "/">` -> Aplica la reglas que hay en su interior (en este caso "Require all denied") haciendo que si la ruta no coincide con ***marca1-i.practica*** o ***marca2-p.practica*** quedará bloqueado

`ErrorLog "logs/default_error.log"` -> Archivo donde se guardarán los registros de fallos y errores

`CustomLog "logs/default_access.log" combined` -> Archivo donde se registrarán las visitas y accesos con éxito/fracaso, usando ***combined***, que incluye todos los datos: IP del cliente, fecha/hora, etc.

## Archivo - marca1.conf y marca2.conf

```
<VirtualHost *:80>
    ServerName marca1-i.practica
    DocumentRoot "/usr/local/apache2/htdocs/marca1"

    <Directory "/usr/local/apache2/htdocs/marca1">
        Options -Indexes +FollowSymLinks
        Require all granted
    </Directory>

    ErrorLog "logs/marca1_error.log"
    CustomLog "logs/marca1_access.log" combined
</VirtualHost>
```

```
<VirtualHost *:80>
    ServerName marca2-p.practica
    DocumentRoot "/usr/local/apache2/htdocs/marca2"

    <Directory "/usr/local/apache2/htdocs/marca2">
        Options -Indexes +FollowSymLinks
        Require all granted
    </Directory>

    ErrorLog "logs/marca2_error.log"
    CustomLog "logs/marca2_access.log" combined
</VirtualHost>
```

## Archivo - marca1.conf

`DocumentRoot "/usr/local/apache2/htdocs/marca1"` -> Carpeta raíz dentro del contenedor donde están los archivos de la web (index.html).

`<Directory "/usr/local/apache2/htdocs/marca1">` -> Aplica la configuración de permisos a la carpeta del sitio web.

`Options -Indexes +FollowSymLinks` -> **-Indexes** desactiva el listado de archivos si no hay index.html (seguridad), y **+FollowSymLinks** permite seguir enlaces simbólicos.

`Require all granted` -> Permite el acceso público a todo el contenido general de esta web.

`ErrorLog "logs/marca1_error.log"` -> Archivo donde se guardarán los registros de errores de la Marca 1.

`CustomLog "logs/marca1_access.log" combined` -> Archivo donde se registrarán los accesos con éxito/fracaso usando el formato combined.
