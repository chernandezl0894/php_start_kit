# Agent Context & Guidelines - CLI Template APP

## Project Overview

This is a modern PHP 8.2+ template built for CLI applications, designed to be extensible to HTTP endpoints, Twig rendering, and AI integrations (via Prism PHP).

## Architecture & Design Patterns

- **Dependency Injection**: Managed via PHP-DI (`core/ContainerFactory.php`).
- **Commands**: Handled by Symfony Console (`src/*/Commands/`).
- **Database / ORM**: SQLite with raw PDO / Repository pattern (`src/*/Repositories/`).
- **Logging**: PSR-3 Logger with Monolog (`core/LoggerService.php`).
- **Modular Structure**: Domain code is located in `src/{DomainName}/` containing Commands, Controllers, Services, Entities, DTOs, and Repositories.

## Tech Stack & Commands

- **PHP**: ^8.2
- **Testing**: `vendor/bin/phpunit`
- **Static Analysis**: `vendor/bin/phpstan analyse`
- **Code Style**: `vendor/bin/php-cs-fixer fix`
- **CLI Entry Point**: `./bin/app` or `php consoles/app.php`

## Rules for AI Assistants

1. **Strict Typing**: Always use declare(strict_types=1); in PHP files.
2. **Architecture Integrity**: Keep business logic inside Services/Entities, not inside Console Commands.
3. **Dependency Injection**: Always inject dependencies via constructor using PHP-DI interfaces.
4. **Future Extensions Roadmap**:
    - **HTTP**: Keep Slim Framework / PSR-7 handlers inside `src/{Domain}/Controllers/`.
    - **Twig**: Views should go into `templates/{domain}/`.
    - **Prism PHP**: Place AI service wrappers inside `src/Shared/Services/AI/` or domain-specific services.

```bash
php_start_kit/
├── config/                  # Archivos de configuración PHP (devuelven arrays)
│   ├── app.php              # Ajustes generales (zona horaria, entorno)
│   ├── database.php         # Parámetros de conexión a SQLite/PDO
│   ├── logging.php          # Niveles y rutas de archivos de log
│   └── telegram.php         # Tokens y Chat ID de la API de Telegram
├── consoles/
│   └── app.php              # Punto de entrada ejecutable de la CLI (Symfony Console)
├── core/                    # Núcleo e infraestructura reutilizable
│   ├── Config.php           # Gestor de configuración con sintaxis dot-notation
│   ├── ContainerFactory.php # Factoría que inicializa el contenedor PHP-DI
│   ├── Database.php         # Wrapper PDO para SQLite y auto-creación de esquema
│   ├── Event.php            # Despachador de eventos del sistema
│   ├── Kernel.php           # Definición de tareas programadas del Scheduler
│   ├── LoggerService.php    # Servicio centralizado de logs con Monolog
│   └── Scheduler.php        # Planificador ligero de comandos tipo Cron
├── database/
│   ├── database.sqlite      # Base de datos SQLite local
│   └── schema.sql           # Script DDL inicial de la base de datos
├── docker/
│   └── Dockerfile           # Imagen de PHP-CLI con extensiones (PDO, etc.)
├── src/                     # Código de dominio de la aplicación
│   ├── Greet/               # Dominio de saludo (ejemplo básico)
│   ├── Heartbeat/           # Dominio de verificación de estado
│   ├── Shared/              # Servicios y comandos transversales
│   │   ├── Commands/        # ScheduleRunCommand (ejecutor del ciclo del Scheduler)
│   │   └── Services/        # TelegramService (cliente para la API de Telegram)
│   └── Tasks/               # Dominio principal GTD
│       ├── Commands/        # CreateTaskCommand, ListTasksCommand, ProcessRemindersCommand
│       ├── DTOs/            # CreateTaskData (Objeto de transferencia de datos)
│       ├── Entities/        # Task (Entidad pura de dominio)
│       ├── Repositories/    # TaskRepository (Acceso a datos con PDO)
│       └── Services/        # TaskService (Lógica de negocio y recordatorios)
├── bootstrap.php            # Carga de Composer, variables .env y constantes globales
├── composer.json            # Gestión de dependencias PHP
└── docker-compose.yml       # Definición de contenedores Docker
```
