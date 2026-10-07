# Servicio de préstamos de equipos de laboratorio

API con FastAPI y SQLModel. Se puede ejecutar localmente con `uv` o en contenedores con Docker Compose y PostgreSQL 18.

## Arranque local

```bash
uv sync
uv run uvicorn prestamos.servidor:app --port 9000
```

`uv sync` crea el entorno `.venv` con las versiones exactas de `uv.lock`, incluidas las de desarrollo. `requirements.txt` lista solo las dependencias de ejecución, para quien instale con `pip`.

Sin `PRESTAMOS_DB_URL`, la app usa SQLite en `datos/prestamos.db` y crea la carpeta `datos/` al arrancar.

## Pruebas

```bash
uv run pytest
```

## Docker

Imagen de producción (multietapa, usuario no root, `HEALTHCHECK` sobre `/salud`):

```bash
docker build -t prestamos .
docker run --rm -p 9000:9000 prestamos
```

Sin variables de entorno, el contenedor usa SQLite en `/app/datos/prestamos.db`.

## Docker Compose con PostgreSQL

```bash
docker compose up -d --build   # levanta app y PostgreSQL 18
docker compose ps              # ambos servicios quedan healthy
docker compose down            # conserva los datos (volumen pgdata)
docker compose down -v         # borra también los datos
```

`PRESTAMOS_DB_URL` se define en `compose.yaml` y apunta al servicio `db`. La app espera a que PostgreSQL esté `healthy` (`pg_isready`) antes de arrancar. Los datos viven en el volumen nombrado `pgdata`, montado en `/var/lib/postgresql`.

## URL

- Salud: <http://127.0.0.1:9000/salud> → `{"estado":"ok"}`
- Motor en uso: <http://127.0.0.1:9000/diagnostico> → `{"motor":"sqlite"}` o `{"motor":"postgresql"}`
- Documentación interactiva: <http://127.0.0.1:9000/docs>
- Registro y listado: `POST` y `GET` en <http://127.0.0.1:9000/prestamos>, con cuerpo `{"equipo": "...", "solicitante": "..."}`
