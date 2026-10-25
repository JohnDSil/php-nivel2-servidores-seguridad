# 08. Seguridad en el Servidor y Mitigación de Vulnerabilidades

## 1. Protección contra Cross-Site Scripting (XSS)

Ocurre cuando un atacante inyecta scripts maliciosos (generalmente JavaScript) en una página web vista por otros usuarios.

- **Mitigación:** Toda cadena de texto devuelta al navegador debe ser escapada mediante `htmlspecialchars()`:

```php
<?php
$entradaUsuario = "<script>alert('Ataque XSS');</script>";

// FORMA INSEGURA (ejecutará el script en el navegador de la víctima):
// echo $entradaUsuario;

// FORMA SEGURA (convierte los caracteres <, >, &, \" a entidades HTML inofensivas):
echo htmlspecialchars($entradaUsuario, ENT_QUOTES | ENT_SUBSTITUTE, 'UTF-8');
?>
```

## 2. Ocultación de Firmas de Software en `php.ini`

Los servidores no deben revelar las versiones exactas que ejecutan, ya que facilita a los atacantes buscar vulnerabilidades conocidas (*exploits*).

Modifique su `/etc/php/8.x/apache2/php.ini`:
```ini
; Oculta la cabecera 'X-Powered-By: PHP/8.x' en las respuestas HTTP
expose_php = Off

; Desactiva la salida visual de errores hacia el cliente
display_errors = Off
display_startup_errors = Off

; Habilita el guardado en log local
log_errors = On
```
