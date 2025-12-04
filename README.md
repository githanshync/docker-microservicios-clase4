# Proyecto: Blog simple con microservicios (Clase 4)

Este proyecto despliega blog básico utilizando arquitectura de microservicios con **Docker compose**

Arquitectura: Nginx (como API gateway), Backend Node.js, Redis cache, MongoDB para persistencia, Frontend Nginx estático.

## Diagrama ASCII

[Cliente] --> (8080) Nginx Gateway ├── /api/* --> Backend (Node.js, 5000) │ ├── Redis (6379) [cache] │ └── MongoDB (27017) [persistencia] └── / --> Frontend (Nginx estático)

## Servicios


| Servicio   | Imagen / Build         | Puerto Expuesto | Función                          |
|------------|------------------------|-----------------|----------------------------------|
| Gateway    | nginx (custom conf)    | 8080            | API Gateway y frontend           |
| Backend    | Node.js (Express)      | 5000            | API CRUD de posts                |
| Redis      | redis:7-alpine         | 6379            | Caché de posts                   |
| MongoDB    | mongo:6                | 27017           | Base de datos                    |
| Frontend   | nginx (HTML estático)  | 80 (interno)    | Interfaz web simple              |

## Uso

1. Clonar repositorio:
   ```bash
   git clone https://github.com/githanshync/docker-microservicios-clase4.git
   cd docker-microservicios-clase4
2. docker compose up -d
3. Abrir http://localhost:8080
4. Health: /gateway/health, /api/health

## Endpoints

- GET /api/posts
- GET /api/posts/:id
- POST /api/posts

Respuestas incluyen `source: "cache"` o `source: "database"` para demostrar HIT/MISS.

## Pruebas

- Primer GET /api/posts: source=database (MISS)
- Segundo GET: source=cache (HIT)
- POST /api/posts: invalida cache de listado
- Persistencia: reiniciar contenedores y verificar datos
- Routing: /gateway/health y /api/health retornan ok

