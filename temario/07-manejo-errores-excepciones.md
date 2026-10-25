# 07. Control de Errores y Excepciones

En PHP moderno, la mayoría de los errores fatales se gestionan mediante el árbol de excepciones bajo la interfaz `Throwable`.

## Estructura `try...catch...finally`

```php
<?php
function conectarServicio(string $host, int$puerto) {
    if ($puerto <= 0 \vert{}\vert{}$puerto > 65535) {
        throw new InvalidArgumentException("El puerto $puerto está fuera de rango.");
    }
    // Lógica simulada de conexión...
    return true;
}

try {
    conectarServicio("192.168.1.1", 99999);
} catch (InvalidArgumentException $e) {
    // Se captura si los datos son incorrectos
    echo "Error en los parámetros: " . $e->getMessage();
} catch (Exception $e) {
    // Excepciones genéricas
    echo "Ocurrió un error inesperado: " . $e->getMessage();
} finally {
    // Se ejecuta SIEMPRE, haya o no excepción
    echo "\nFin de la comprobación.";
}
?>
```

## Registro de Errores en Logs de Servidor

En entornos reales de administración, nunca se muestran los errores al usuario (`display_errors = Off`), sino que se escriben en el fichero de log de Apache (`/var/log/apache2/error.log`):

```php
<?php
// Forzar el registro de un evento en el log de errores del sistema
error_log("ALERTA ASIR: Intento de acceso no autorizado desde la IP " . $_SERVER['REMOTE_ADDR']);
?>
```
