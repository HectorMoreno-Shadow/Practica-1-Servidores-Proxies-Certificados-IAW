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