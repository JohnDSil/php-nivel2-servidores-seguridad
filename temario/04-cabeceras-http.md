# 04. Cabeceras HTTP y Códigos de Estado

PHP permite manipular directamente la cabecera que el servidor enviará al navegador mediante la función `header()`.

> **Regla Crítica:** La función `header()` **debe invocarse antes** de que se emita cualquier tipo de salida (HTML, espacios en blanco o llamadas a `echo`). Si ya se envió texto, PHP devolverá el error: *"Headers already sent"*.

## 1. Redirecciones HTTP

```php
<?php
// Redirección temporal estándar (Código HTTP 302)
header("Location: [https://mi-servidor.local/login.php](https://mi-servidor.local/login.php)");
exit; // Es imprescindible terminar el script tras una redirección para evitar ejecuciones posteriores

// Redirección permanente (Código HTTP 301)
header("Location: [https://mi-servidor.local/nueva-zona.php](https://mi-servidor.local/nueva-zona.php)", true, 301);
exit;
?>
```

## 2. Definición del Tipo de Contenido (MIME Type)

Indica al navegador cómo interpretar la información que recibe:

```php
<?php
// Servir una respuesta en formato JSON para una API
header("Content-Type: application/json; charset=UTF-8");

$datos = [
    "estado" => "online",
    "servidores_activos" => 4
];

echo json_encode($datos);
?>
```

## 3. Asignación de Códigos de Estado (`http_response_code`)

```php
<?php
// Responder con un error 404 Not Found
http_response_code(404);
echo "El recurso solicitado no existe en este servidor.";
?>
```
