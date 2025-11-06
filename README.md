# Synapse CIC — Backend utilities (DB, import/export)

Este repositorio incluye utilidades para gestionar la base de datos de jugadores y cuentas para el proyecto "Synapse CIC".

Resumen de artefactos creados:

- `migrations/001_create_players_table.sql` — migración para crear la tabla `jugadores` en PostgreSQL.
- `migrations/002_create_users_table.sql` — migración para crear la tabla `users`.
- `scripts/import_players.py` — importar jugadores desde un XLSX a la tabla `jugadores`.
- `scripts/export_players.py` — exportar `jugadores` a XLSX.
- `scripts/import_accounts.py` — importar cuentas desde `accounts_template.xlsx` a la tabla `users` (hash de contraseñas con bcrypt).
- `scripts/generate_templates.py` — genera `players_template.xlsx` y `accounts_template.xlsx` como ejemplos.
- `.env.example` — ejemplo de configuración (DATABASE_URL).
- `requirements.txt` — dependencias Python.

Recomendación: usar PostgreSQL para producción. Se proporcionan consultas SQL para crear las tablas.

Instalación rápida (Windows / PowerShell):

1. Crear entorno virtual e instalar dependencias

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

2. Copia `.env.example` a `.env` y rellena `DATABASE_URL` con tu conexión PostgreSQL:

```text
DATABASE_URL=postgres://user:password@host:5432/dbname
```

3. Ejecutar migraciones en la base de datos (usando psql o tu cliente preferido):

```powershell
psql $env:DATABASE_URL -f migrations/001_create_players_table.sql
psql $env:DATABASE_URL -f migrations/002_create_users_table.sql
```

4. Generar plantillas XLSX (opcional):

```powershell
python scripts/generate_templates.py
```

5. Importar jugadores de ejemplo:

```powershell
python scripts/import_players.py --file players_template.xlsx
```

6. Importar cuentas:

```powershell
python scripts/import_accounts.py --file accounts_template.xlsx
```

7. Iniciar el servidor admin (Flask) y abrir la interfaz web

```powershell
# Desde la carpeta del proyecto
python backend\app.py
```

Luego abre http://localhost:5000/admin en tu navegador para acceder a la interfaz de altas/bajas/modificaciones.
```

Notas y seguridad
- Nunca subas archivos `.env` con credenciales a repositorios públicos.
- Para producción, considera usar un servidor de base de datos gestionado y copias de seguridad regulares.

Siguientes pasos sugeridos

- Implementar un pequeño backend (Flask/Express) con endpoints para login y APIs para gestionar jugadores (GET/POST/PUT/DELETE).
- Actualizar las páginas HTML (`login_estudiante.html`) para que llamen al backend para autenticación.
- Crear pruebas unitarias para los scripts de import/export.

Uso con XAMPP / MySQL (Windows)
--------------------------------

Si estás usando XAMPP con MariaDB/MySQL localmente (como en esta máquina), aquí tienes pasos rápidos y scripts de ayuda incluidos en `scripts/`.

1) Conectar la extensión de bases de datos en VS Code

- Crea una nueva conexión: Host `127.0.0.1`, Port `3306` (o `3307` si cambiaste el puerto), Username `root` o `synapse_user`, Password (según tu configuración).
- Conéctate y prueba con `SELECT VERSION();`.

2) Ejecutar las migraciones SQL

Opción A — desde la extensión de VS Code: abre `migrations/mysql_all_create_synapse_db_and_user.sql` y ejecútalo con la conexión (Run/Execute). Esto creará la base `synapse_db` y las tablas.

Opción B — desde PowerShell (sin usar redirección '<'):

```powershell
# Ejecuta el SQL usando el cliente mysql de XAMPP (te pedirá la contraseña de root)
Get-Content .\migrations\mysql_all_create_synapse_db_and_user.sql | & 'C:\xampp\mysql\bin\mysql.exe' -u root -p
```

3) Scripts incluidos

- `scripts/create_env.ps1` — asistente interactivo para generar `.env` en la raíz del proyecto (rellena DB_HOST, DB_PORT, DB_USER, DB_PASSWORD, DB_NAME).
- `scripts/backup_db.ps1` — crea un volcado SQL en la carpeta `backups\` usando `mysqldump` de XAMPP.

Ejemplo: crear `.env` (PowerShell):

```powershell
.\scripts\create_env.ps1
```

Ejemplo: crear backup (PowerShell):

```powershell
.\scripts\backup_db.ps1
```

4) Crear usuario limitado para la aplicación (opcional pero recomendado)

Si prefieres no usar `root` para la aplicación, crea el usuario `synapse_user` y asigna privilegios sobre `synapse_db`:

```sql
CREATE DATABASE IF NOT EXISTS synapse_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'synapse_user'@'localhost' IDENTIFIED BY 'TU_CONTRASENA_SEGURA';
GRANT ALL PRIVILEGES ON synapse_db.* TO 'synapse_user'@'localhost';
FLUSH PRIVILEGES;
```

Puedes ejecutar ese bloque desde la extensión o guardarlo como `create_synapse_user.sql` y ejecutarlo con:

```powershell
Get-Content .\migrations\create_synapse_user.sql | & 'C:\xampp\mysql\bin\mysql.exe' -u root -p
```

5) Arrancar el backend (usar `.env` con credenciales correctas)

```powershell
# activar venv y arrancar Flask
.\.venv\Scripts\Activate.ps1
python backend\app.py
```

Si algo falla, copia aquí la salida del log (`C:\xampp\mysql\data\mysql_error.log`) y el resultado de `netstat -ano | findstr ":3306"` para ayudar en el diagnóstico.
