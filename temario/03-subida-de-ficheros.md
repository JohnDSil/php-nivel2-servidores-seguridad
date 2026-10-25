# 03. Subida Segura de Ficheros

La subida de ficheros es uno de los vectores de ataque más críticos. Subir un archivo con extensión `.php` en una carpeta pública permite a un atacante ejecutar código arbitrario en el servidor.

## Directivas en `php.ini`
- `file_uploads = On` : Habilita la subida de archivos HTTP.
- `upload_max_filesize = 10M` : Tamaño máximo por fichero.
- `post_max_size = 12M` : Debe ser superior a `upload_max_filesize`.

## La Superglobal `$_FILES`
Cuando un formulario usa `enctype="multipart/form-data"`, PHP almacena los metadatos en `$_FILES`:
- `$_FILES['archivo']['name']`: Nombre original enviado por el cliente (inseguro).
- `$_FILES['archivo']['tmp_name']`: Ruta temporal donde PHP alojó el fichero en el servidor.
- `$_FILES['archivo']['size']`: Tamaño en bytes.
- `$_FILES['archivo']['error']`: Código de error (`UPLOAD_ERR_OK` si fue bien).

## Script de Subida con Verificación de Tipo MIME Real

```php
<?php
if ($_SERVER['REQUEST_METHOD'] === 'POST' && isset($_FILES['documento'])) {
    $fichero =$_FILES['documento'];

    // 1. Comprobar errores de subida
    if ($fichero['error'] !== UPLOAD_ERR_OK) {
        die("Error en la subida del fichero.");
    }

    // 2. Limitar el tamaño (máximo 2 MB)
    if ($fichero['size'] > 2 * 1024 * 1024) {
        die("El archivo excede el tamaño máximo permitido (2MB).");
    }

    // 3. Validar el tipo MIME REAL inspeccionando el contenido (no fiarse de la extensión)
    $finfo = new finfo(FILEINFO_MIME_TYPE);$mimeReal = $finfo->file($fichero['tmp_name']);

    $mimesPermitidos = [
        'pdf' => 'application/pdf',
        'png' => 'image/png',
        'jpg' => 'image/jpeg'
    ];

    $extension = array_search($mimeReal,$mimesPermitidos, true);

    if ($extension === false) {
        die("Formato de archivo no permitido.");
    }

    // 4. Renombrar el archivo con un identificador único para evitar sobreescrituras e inyecciones
    $nombreFinal = sprintf('/var/www/uploads/\%s.\%s', bin2hex(random_bytes(16)),$extension);

    if (move_uploaded_file($fichero['tmp_name'],$nombreFinal)) {
        echo "Archivo subido correctamente de forma segura.";
    } else {
        echo "Fallo al mover el archivo al destino.";
    }
}
?>
```
