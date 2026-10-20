# Backend PokeDex: Django 5.2 + DRF + PostgreSQL

Este laboratorio permite aprender y reforzar Python, modelos de Django y bases de datos relacionales (PostgreSQL) construyendo la API de una PokeDex.

## Ruta de aprendizaje

Los laboratorios avanzan de lo tradicional a lo moderno:

1. **Esquema tradicional MVT / MVC:** se parte de **rutas** (`urls.py`), **templates** (HTML renderizado en el servidor) y **views**. Django implementa el patrón **MVT** (Model–View–Template), su variante del clásico **MVC**.
2. **Operaciones en base de datos:** modelos, migraciones y consultas con el ORM (crear, leer, actualizar y eliminar).
3. **Django REST Framework (DRF):** serializadores, vistas y endpoints que entregan JSON, consumido luego por el frontend en React.
4. **Seguridad con OAuth 2.0 y JWT:** autenticación y autorización de la API para cerrar el ciclo.

> 💡 **Nota de aprendizaje:** Cada etapa se apoya en la anterior. Entender las rutas, vistas y el ORM hace mucho más sencillo comprender cómo DRF y la autenticación por tokens funcionan "por debajo".

## Objetivos
- Reconocer y aplicar conceptos de POO: clases, objetos, atributos y métodos.
- Comprender el patrón MVT/MVC y el flujo petición → ruta → vista → template/respuesta.
- Modelar datos relacionales (`ForeignKey` y `ManyToManyField`) con el ORM de Django.
- Exponer los datos mediante una API REST y protegerla con OAuth 2.0 y JWT.

## Tareas a realizar
1. Rutas, views y templates (MVT/MVC): lista y detalle de Pokémon y entrenadores.
2. Generación de modelos `Type`, `Pokemon` y `Trainer`.
3. Generación de migraciones y operaciones en base de datos con el ORM.
4. Registro de modelos en el panel de administración.
5. Serializadores y endpoints REST con Django REST Framework.
6. Autenticación con OAuth 2.0 y JWT.

## Entorno con Docker (sin instalar nada en tu equipo)

### Requerimientos
- Docker Desktop (Windows / macOS) o Docker Engine + Compose (Linux).

No necesitas Python, entornos virtuales ni PostgreSQL locales: todo corre en contenedores dentro de la red `pokedex_framework`.

### Levantar el entorno
Desde la **raíz del repositorio**:
```bash
docker compose up --build
```
El backend queda en <http://localhost:8000>. Al iniciar, ejecuta `migrate` automáticamente. El código está montado como volumen, así que **los cambios en tu editor se recargan solos**.

### Ejecutar comandos de Django
Se usa `docker compose exec backend ...` (con los contenedores encendidos). Es idéntico en Linux, macOS y Windows.

| Tarea | Comando |
|---|---|
| Crear una app | `docker compose exec backend python manage.py startapp <nombre>` |
| Generar migraciones | `docker compose exec backend python manage.py makemigrations` |
| Aplicar migraciones | `docker compose exec backend python manage.py migrate` |
| Crear superusuario | `docker compose exec backend python manage.py createsuperuser` |
| Shell de Django | `docker compose exec backend python manage.py shell` |
| Ejecutar tests | `docker compose exec backend python manage.py test` |

> 💡 **Nota:** Si el backend está apagado puedes usar `docker compose run --rm backend python manage.py <comando>`.

### Instalar nuevas dependencias
1. Agrega el paquete a [requirements.txt](./requirements.txt) (con su versión fija) o instálalo y congela:
   ```bash
   docker compose exec backend pip install <paquete>
   docker compose exec backend pip freeze > requirements.txt
   ```
2. Reconstruye la imagen para que quede permanente:
   ```bash
   docker compose up -d --build backend
   ```

### Logs y reinicio
```bash
docker compose logs -f backend
docker compose restart backend
```

### Base de datos
- Motor: **PostgreSQL 17** (la versión más reciente soportada oficialmente por Django 5.2).
- Los datos persisten en el volumen `pgdata`. Para reiniciar la base desde cero:
  ```bash
  docker compose down -v
  ```
- **pgAdmin:** <http://localhost:5050> (`admin@pokedex.com` / `admin`). Registra el servidor con Host `db`, Puerto `5432`, BD `pokedex`, Usuario `pokedex`, Contraseña `pokedex`.
- Consola SQL directa:
  ```bash
  docker compose exec db psql -U pokedex -d pokedex
  ```

### Configuración en `settings.py`
La conexión lee variables de entorno definidas en [docker-compose.yml](../docker-compose.yml):

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': os.environ.get('POSTGRES_DB', 'pokedex'),
        'USER': os.environ.get('POSTGRES_USER', 'pokedex'),
        'PASSWORD': os.environ.get('POSTGRES_PASSWORD', 'pokedex'),
        'HOST': os.environ.get('POSTGRES_HOST', 'localhost'),
        'PORT': os.environ.get('POSTGRES_PORT', '5432'),
    }
}
```

> 💡 **Nota de aprendizaje:** Dentro de Docker el host es `db` (el nombre del servicio), no `localhost`.

## Problemas frecuentes
- **`connection refused` a la base de datos:** espera a que `db` esté *healthy* (`docker compose ps`).
- **Puerto 8000 ocupado:** detén el proceso que lo usa o cambia el mapeo `"8000:8000"` en el compose.
- **Windows, error `^M` o `exec format error`:** los archivos quedaron con finales de línea CRLF; configura `git config core.autocrlf input` y vuelve a clonar.
- **Cambié `requirements.txt` y no lo ve:** reconstruye con `docker compose up -d --build backend`.

## Alternativa sin Docker (opcional)
Requiere Python 3.12 y una instancia de PostgreSQL propia (ajusta `POSTGRES_HOST`).

```bash
python3 -m venv .venv            # Windows: py -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt
python manage.py runserver
```

# Nota
Para los siguientes pasos se deberán seguir las instrucciones del docente en clase. Puedes contactarlo a <paperez@puce.edu.ec> o a <pablo.perez@uisek.edu.ec> según la institución donde te encuentres.

# Ejemplos de cadenas de conexión para Django

### PostgreSQL (psycopg 3, ya incluido en `requirements.txt`)
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'nombre_de_tu_base_de_datos',
        'USER': 'tu_usuario',
        'PASSWORD': 'tu_contraseña',
        'HOST': 'localhost',
        'PORT': '5432',
    }
}
```

### MySQL (`pip install mysqlclient`)
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'nombre_de_tu_base_de_datos',
        'USER': 'tu_usuario',
        'PASSWORD': 'tu_contraseña',
        'HOST': 'localhost',
        'PORT': '3306',
    }
}
```

### SQLite
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```
