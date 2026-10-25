# 05. Cookies y Persistencia en el Cliente

Una cookie es un pequeño fragmento de texto que el servidor ordena almacenar en el navegador del usuario y que este devuelve en cada petición sucesiva.

## Configuración Segura con `setcookie()`

Al igual que las cabeceras HTTP, las cookies deben emitirse antes de cualquier salida de texto.

```php
<?php
$opcionesCookie = [
    'expires'  => time() + (86400 * 30), // Expira en 30 días
    'path'     => '/',                    // Disponible en todo el sitio web
    'domain'   => '',                     // Dominio actual
    'secure'   => true,                   // Solo viaja si la conexión es HTTPS
    'httponly' => true,                   // INACCESIBLE por JavaScript (Protege contra robo por XSS)
    'samesite' => 'Strict'                // Protección estricta contra ataques CSRF
];

setcookie('preferencia_tema', 'dark', $opcionesCookie);
?>
```

## Lectura y Eliminación

```php
<?php
// Leer el valor recibido desde el cliente
if (isset($_COOKIE['preferencia_tema'])) {
    echo "Tema seleccionado: " . htmlspecialchars($_COOKIE['preferencia_tema']);
}

// Para eliminar una cookie, se vuelve a configurar con una fecha en el pasado
setcookie('preferencia_tema', '', time() - 3600, '/');
?>
```
