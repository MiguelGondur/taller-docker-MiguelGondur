# Préstamos de laboratorio

API de préstamo de equipos hecha con FastAPI y SQLModel.

## Correr local

    uv sync
    uv run uvicorn prestamos.servidor:app --port 9000

Si no se define PRESTAMOS_DB_URL usa SQLite en datos/prestamos.db (crea la carpeta sola).

## Pruebas

    uv run pytest

## Docker

    docker build -t prestamos .
    docker run -p 9000:9000 prestamos

## Con postgres

    docker compose up -d --build

Levanta la app y postgres 18. La app espera a que la base esté healthy. Los datos quedan en el volumen pgdata, así que `docker compose down` no los borra pero `docker compose down -v` sí.

## URLs

- http://localhost:9000/salud
- http://localhost:9000/diagnostico (dice sqlite o postgresql)
- http://localhost:9000/prestamos (GET y POST, con {"equipo": "...", "solicitante": "..."})
