# Documentación de la API (Laravel 12 REST API)

Este documento contiene las explicaciones técnicas y decisiones arquitectónicas de las herramientas utilizadas para construir esta API. Su objetivo es mantener la simplicidad y asegurar la comprensión de cada componente de Laravel.

---

## 1. Introducción
El proyecto es una API RESTful construida con Laravel 12. Sigue un enfoque simple (sin over-engineering) donde las rutas están definidas exclusivamente en `routes/api.php` y se comunica mediante formato JSON.

*(Este archivo se irá actualizando a medida que avancemos por las fases del plan de implementación).*

## 2. Autenticación con Laravel Sanctum
Laravel Sanctum es un paquete oficial muy liviano que proporciona un sistema de autenticación sencillo para SPAs (Single Page Applications) o APIs simples.

**¿Por qué Sanctum y no JWT u OAuth (Passport)?**
Sanctum es mucho más simple de configurar, no requiere claves de encriptación complejas y en una API básica cumple perfectamente almacenando los tokens (`plainTextToken`) que luego verifica a través del Header `Authorization: Bearer <token>`.

**Implementación de Tokens:**
- **Modelo de Usuario:** Se debe añadir el trait `HasApiTokens` a la clase `User`. Este trait es el encargado de brindarnos el método `createToken()`.
- **Registro:** Recibe los datos, los valida (usando la función `validate()`), guarda el usuario en la BD encriptando el password mediante `Hash::make()` y retorna el usuario registrado en formato JSON.
- **Login:** Busca al usuario, verifica la contraseña y en caso de éxito, ejecuta `$user->createToken('auth_token')->plainTextToken`. Este token es el que entregaremos en nuestras peticiones subsiguientes.
