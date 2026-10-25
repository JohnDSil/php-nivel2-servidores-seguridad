# 06. Sesiones y Autenticación en el Servidor

A diferencia de las cookies, **los datos de una sesión se guardan en el servidor**. En el navegador únicamente se guarda una cookie con el identificador de sesión (`PHPSESSID`).

En Linux, los datos de sesión se almacenan habitualmente como ficheros en `/var/lib/php/sessions/`.

## Ciclo de Vida de una Sesión

```php
<?php
// Iniciar o retomar la sesión existente (siempre antes de cualquier salida)
session_start();

// Guardar datos en la sesión tras validar login
$_SESSION['usuario'] = "admin";
$_SESSION['rol']     = "administrador_sistemas";
$_SESSION['login_time'] = time();

echo "Sesión iniciada para: " . $_SESSION['usuario'];
?>
```

## Prevención de Secuestro de Sesión (*Session Fixation*)

Siempre que un usuario cambie de nivel de privilegios (por ejemplo, al iniciar sesión con usuario y contraseña), se debe regenerar el identificador:

```php
<?php
session_start();

// Validar credenciales hipotéticas...
$credencialesCorrectas = true;

if ($credencialesCorrectas) {
    // Genera un nuevo ID y borra el fichero temporal antiguo
    session_regenerate_id(true);
    $_SESSION['autenticado'] = true;
}
?>
```

## Cierre y Destrucción de la Sesión (Logout)

```php
<?php
session_start();

// 1. Vaciar el array de variables
$_SESSION = [];

// 2. Destruir la cookie de sesión en el navegador
if (ini_get("session.use_cookies")) {
    $params = session_get_cookie_params();
    setcookie(session_name(), '', time() - 42000,
        $params["path"], $params["domain"],
        $params["secure"], $params["httponly"]
    );
}

// 3. Destruir la sesión en el servidor
session_destroy();

header("Location: login.php");
exit;
?>
```
