# 09. Ejecución de Comandos del Sistema Operativo

PHP puede interactuar con el shell de Linux para labores administrativas mediante funciones de ejecución externa.

## Funciones Principales

- `exec($comando, $salidaArray, $codigoRetorno)`: Ejecuta el comando y devuelve la última línea, guardando el resto en un array.
- `shell_exec($comando)`: Devuelve la salida completa del comando como una cadena.

```php
<?php
// Comprobar el espacio libre en disco del servidor
$espacioDisco = shell_exec("df -h /");
echo "<pre>" . htmlspecialchars($espacioDisco) . "</pre>";
?>
```

## Peligro Crítico: Inyección de Comandos

Si se concatena una entrada de usuario sin filtrar directamente en el comando, un atacante puede ejecutar órdenes arbitrarias en el servidor con los permisos de `www-data`.

### Ejemplo Inseguro:
```php
// INSEGURO: Si el usuario pasa "127.0.0.1; rm -rf /", se ejecutarán ambos comandos
$ip =$_POST['ip'];
system("ping -c 1 " . $ip);
```

### Mitigación con `escapeshellarg()`:
Envuelve el argumento entre comillas simples y escapa caracteres de control de shell:

```php
<?php
$ip =$_POST['ip'] ?? '127.0.0.1';

// Escapar de forma estricta el parámetro
$ipSegura = escapeshellarg($ip);

$salida = [];$resultado = 0;
exec("ping -c 1 " . $ipSegura, $salida,$resultado);

if ($resultado === 0) {
    echo "El host responde correctamente.";
} else {
    echo "El host no es alcanzable.";
}
?>
```
