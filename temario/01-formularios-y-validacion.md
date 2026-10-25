# 01. Procesamiento Seguro de Formularios

Cualquier dato enviado por un usuario a través de `$_GET` o `$_POST` debe considerarse potencialmente malicioso. Jamás se debe confiar en los datos de entrada sin sanitizarlos y validarlos previamente.

## Validación frente a Sanitización
- **Sanitizar:** Limpiar la entrada retirando caracteres no permitidos o peligrosos.
- **Validar:** Comprobar si el dato cumple un formato estricto (por ejemplo, si un puerto es un entero o un email es válido).

## Uso de la API de Filtros (`filter_var` y `filter_input`)

PHP incluye de forma nativa funciones para filtrar datos sin necesidad de complejas expresiones regulares manuales:

```php
<?php
// Recoger y validar un número entero desde POST
$puerto = filter_input(INPUT_POST, 'puerto', FILTER_VALIDATE_INT);

if ($puerto === false || $puerto < 1 \vert{}\vert{}$puerto > 65535) {
    echo "El puerto introducido no es válido.";
} else {
    echo "Puerto validado correctamente: $puerto";
}

// Sanitizar y validar una dirección de correo
$emailRaw =$_POST['email'] ?? '';
$emailLimpio = filter_var($emailRaw, FILTER_SANITIZE_EMAIL);

if (filter_var($emailLimpio, FILTER_VALIDATE_EMAIL)) {
    echo "Correo válido: $emailLimpio";
} else {
    echo "Formato de correo incorrecto.";
}
?>
```
