# Tooldata API Pública — Documentación y Proxy

Proyecto que sirve la documentación interactiva (Redoc) y el proxy reverso para la API pública de Tooldata en `api.tooldata.io`.

## Arquitectura

```
Cliente → api.tooldata.io
            │
            ├── /                          → Redoc (documentación interactiva)
            ├── /openapi.yaml              → Especificación OpenAPI
            ├── /collections               → API backend (proxy)
            ├── /collections/:id/posts     → API backend (proxy)
            ├── /collections/:id/words     → API backend (proxy)
            └── /cualquier-otra-ruta       → Redoc (documentación)
```

Solo las rutas `/collections*` se envían al backend. Todo lo demás sirve la documentación.

## Desarrollo local

Requiere que el stack principal de Tooldata v2 esté corriendo (`deployment.yml`).

```bash
# Levantar (se conecta a la red v2_default del stack principal)
docker compose up -d

# Ver documentación
open http://localhost:8085

# Probar API
curl -H "Authorization: ApiKey <tu_key>" http://localhost:8085/collections
```

## Producción

En producción, el dominio `api.tooldata.io` apunta a este Nginx.
La red Docker debe ser la misma donde corre el contenedor `api` del backend.

Ajustar en `docker-compose.yml`:

```yaml
networks:
  default:
    name: <nombre-red-produccion>
    external: true
```

## Endpoints API

| Método | Endpoint | Descripción |
|--------|----------|-------------|
| GET | `/collections` | Listar proyectos del cliente |
| GET | `/collections/:id/posts` | Posts paginados con filtros |
| GET | `/collections/:id/words` | Nube de palabras |

Autenticación: `Authorization: ApiKey <key>`

Ver documentación completa en `openapi.yaml` o en la interfaz Redoc.
