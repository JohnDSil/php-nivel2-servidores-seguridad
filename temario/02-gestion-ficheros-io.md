# 02. Entrada/Salida y Sistema de Ficheros

La lectura y escritura de ficheros es una de las tareas más comunes en administración de sistemas (generación de logs, lectura de configuraciones, etc.).

## Permisos en Linux
En sistemas Debian/Ubuntu, el servidor Apache se ejecuta habitualmente bajo el usuario y grupo `www-data`.
- Para que PHP pueda escribir en un directorio o archivo, este debe pertenecer a dicho usuario o tener los permisos adecuados:

```bash
sudo chown -R www-data:www-data /var/www/html/logs/
sudo chmod 750 /var/www/html/logs/
```

## Operaciones Rápidas de Ficheros

### 1. Lectura completa (`file_get_contents`)
```php
<?php
$fichero = "/var/log/app_status.txt";

if (file_exists($fichero) && is_readable($fichero)) {
    $contenido = file_get_contents($fichero);
    echo nl2br(htmlspecialchars($contenido));
} else {
    echo "El fichero no existe o no tiene permisos de lectura.";
}
?>
```

### 2. Escritura y Registro de Logs (`file_put_contents`)
La bandera `FILE_APPEND` evita sobreescribir el fichero, añadiendo el nuevo texto al final:

```php
<?php
$rutaLog = "/var/www/html/logs/servicios.log";
$entrada = date('Y-m-d H:i:s') . " - Servicio Apache reiniciado\n";

// El flag LOCK_EX evita escrituras simultáneas corruptas
if (file_put_contents($rutaLog,$entrada, FILE_APPEND | LOCK_EX) !== false) {
    echo "Evento registrado con éxito.";
} else {
    echo "Error al escribir en el fichero de log.";
}
?>
```
