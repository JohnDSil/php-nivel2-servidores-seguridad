# PHP - Nivel 2: Servicios Web, Ficheros y Seguridad
> Documentación basada en los módulos de Características y Seguridad de [php.net/manual/es/](https://www.php.net/manual/es/)

Guía técnica enfocada en la administración de servidores web, persistencia de datos, seguridad del backend e integración con sistemas Linux para 2º de ASIR.

---

## 📑 Índice de Temas

| Nº | Tema | Archivo | Conceptos Clave |
| :---: | :--- | :--- | :--- |
| **01** | Procesamiento Seguro de Formularios | [`01-formularios-y-validacion.md`](temario/01-formularios-y-validacion.md) | `filter_input()`, saneamiento de datos y validación de tipos |
| **02** | Entrada/Salida y Sistema de Ficheros | [`02-gestion-ficheros-io.md`](temario/02-gestion-ficheros-io.md) | `file_get_contents`, `file_put_contents`, permisos en Linux (`www-data`) |
| **03** | Subida Segura de Ficheros | [`03-subida-de-ficheros.md`](temario/03-subida-de-ficheros.md) | Array `$_FILES`, tipos MIME reales (`finfo`), `move_uploaded_file()` |
| **04** | Cabeceras HTTP y Redirecciones | [`04-cabeceras-http.md`](temario/04-cabeceras-http.md) | Función `header()`, redirecciones 301/302, tipos de contenido y códigos de estado |
| **05** | Cookies y Persistencia en el Cliente | [`05-cookies-y-persistencia.md`](temario/05-cookies-y-persistencia.md) | `setcookie()`, flags `HttpOnly`, `Secure` y directiva `SameSite` |
| **06** | Sesiones y Autenticación en el Servidor | [`06-sesiones-y-autenticacion.md`](temario/06-sesiones-y-autenticacion.md) | `session_start()`, ciclo de vida, fijación y secuestro de sesiones |
| **07** | Control de Errores y Excepciones | [`07-manejo-errores-excepciones.md`](temario/07-manejo-errores-excepciones.md) | Directivas de log en Apache, bloques `try...catch...finally`, `Throwable` |
| **08** | Seguridad y Mitigación de Vulnerabilidades | [`08-seguridad-en-el-servidor.md`](temario/08-seguridad-en-el-servidor.md) | Prevención de XSS (`htmlspecialchars`), ocultación de firmas en `php.ini` |
| **09** | Ejecución de Comandos del Sistema | [`09-ejecucion-comandos-so.md`](temario/09-ejecucion-comandos-so.md) | Funciones `exec()` / `shell_exec()`, inyecciones de comandos y `escapeshellarg()` |

---

## 🛠️ Requisitos de Entorno
- Servidor web **Apache 2.4** o **Nginx con PHP-FPM**.
- PHP versión 8.1 o superior.
- Usuario estándar del servidor web: `www-data:www-data` (en Debian/Ubuntu).
