# PymesDataStrategy - Backend

El backend de PymesDataStrategy contiene los servicios encargados de gestionar las operaciones principales de la plataforma, procesar información empresarial, coordinar trabajos ETL y proporcionar los mecanismos de comunicación entre los diferentes componentes.

Está compuesto por una API Gateway desarrollada con Node.js y TypeScript, un Worker ETL desarrollado en Python y servicios de infraestructura para persistencia, colas y almacenamiento.

---

## Arquitectura del backend

### 1. API Gateway

Servicio principal desarrollado con Node.js y TypeScript.

Se encarga de recibir y gestionar las solicitudes de la aplicación web, coordinar las operaciones internas y proporcionar los endpoints necesarios para utilizar las funcionalidades de la plataforma.

Entre sus responsabilidades se encuentran:

- Gestión de solicitudes HTTP.
- Administración de operaciones de la plataforma.
- Gestión de conjuntos de datos.
- Integración con PostgreSQL mediante Prisma.
- Coordinación de trabajos de procesamiento.
- Comunicación mediante Redis y BullMQ.
- Sincronización de estados de trabajos.
- Exposición de documentación mediante Swagger / OpenAPI.
- Verificación del estado del servicio.

**Documentación:**

http://localhost:3000/api/docs

**Health Check:**

```http
GET /health
```

### 2. Worker ETL

Servicio desarrollado en Python y FastAPI, encargado de ejecutar procesos de transformación y procesamiento de información.

Sus principales responsabilidades son:

- Consumo de trabajos desde colas Redis mediante BullMQ.
- Procesamiento de conjuntos de datos.
- Transformación de información mediante Polars y Pandas.
- Integración con almacenamiento MinIO.
- Ejecución de procesos ETL.
- Gestión de tareas relacionadas con la transformación de datos.
- Soporte para flujos de validación y asistencia mediante inteligencia artificial.

**Puerto:** `8000`

**Endpoints de salud:**

```http
GET /health
GET /health/live
GET /health/ready
```

### 3. PostgreSQL

Base de datos principal utilizada para almacenar la información estructurada de la plataforma.

El esquema se gestiona mediante Prisma y la inicialización de PostgreSQL incluye las extensiones necesarias para el funcionamiento de la base de datos.

### 4. Redis

Servicio utilizado para gestionar colas de trabajos y mecanismos de comunicación entre los componentes del backend.

BullMQ utiliza Redis para coordinar los procesos de ejecución y seguimiento de tareas.

### 5. MinIO

Sistema de almacenamiento compatible con S3, utilizado para administrar archivos y objetos relacionados con los conjuntos de datos.

Incluye una consola web para administrar el almacenamiento.

### 6. Servicios del Dashboard PYMES

El backend también integra servicios independientes para funcionalidades de analítica empresarial.

**Dashboard Backend**

Desarrollado con Node.js y Express. Incluye:

- Autenticación.
- Carga de archivos.
- Gestión de empresas.
- Consulta de estadísticas.
- Endpoint de verificación de estado.

Rutas principales:

```http
/api/auth
/api/upload
/api/empresas
/api/stats
/api/health
```

**Dashboard Agent**

Servicio desarrollado con Python y FastAPI que proporciona funcionalidades de inteligencia artificial para consultas y análisis empresariales.

Integra Google Gemini y utiliza una configuración de conexión a PostgreSQL.

Endpoints principales:

- Consultas al agente.
- Análisis avanzado.
- Verificación de estado mediante `/health`.

La documentación interactiva se encuentra disponible en:

http://localhost:8001/docs

---

## Tecnologías utilizadas

| Tecnología | Uso |
|---|---|
| Node.js 22 | Entorno de ejecución de la API. |
| TypeScript | Desarrollo del backend principal. |
| Prisma | Acceso y gestión del esquema de datos. |
| PostgreSQL | Base de datos principal. |
| Redis | Colas y comunicación. |
| BullMQ | Gestión de trabajos. |
| Python 3.12 | Procesamiento de datos. |
| FastAPI | Servicios Python. |
| Uvicorn | Servidor ASGI. |
| Polars / Pandas | Procesamiento de datos. |
| MinIO | Almacenamiento de archivos. |
| Google Gemini | Funcionalidades de inteligencia artificial. |
| Docker Compose | Orquestación de servicios. |

## Estructura del directorio

```text
backend/
│
├── api/
│   ├── src/
│   └── Dockerfile.dev
│
├── worker/
│   ├── src/
│   ├── pyproject.toml
│   ├── .python-version
│   ├── Dockerfile.dev
│   └── README.md
│
├── docker/
│   └── postgres/
│       └── init.sql
│
├── docker-compose.yml
├── package.json
├── .env.example
└── README.md
```

Los servicios adicionales del dashboard se encuentran en:

```text
services/
└── dashboard-pymes/
    ├── backend/
    └── agent/
```

## Requisitos previos

Para ejecutar todos los servicios mediante Docker:

- Docker Desktop.
- Docker Compose v2.
- Git.
- Conexión a internet.

Para desarrollar los servicios individualmente:

- Node.js 22.
- pnpm.
- Python 3.12.
- uv.
- PostgreSQL y Redis, si se ejecutan fuera de Docker.

## Configuración de variables de entorno

Desde el directorio `backend/`, crear el archivo `.env`:

**Linux / macOS:**

```bash
cp .env.example .env
```

**Windows PowerShell:**

```powershell
Copy-Item .env.example .env
```

Revisar todas las variables definidas en `.env.example`.

La configuración contempla los parámetros necesarios para conectar los servicios de infraestructura, administrar credenciales y establecer la comunicación entre los componentes.

Entre los valores que deben revisarse se encuentran:

- Configuración de PostgreSQL.
- Configuración de Redis.
- Credenciales de MinIO.
- Parámetros de autenticación.
- Variables de conexión entre servicios.
- Claves de proveedores externos de inteligencia artificial.

### Configuración de Gemini

Para habilitar las funcionalidades de inteligencia artificial que utilizan Google Gemini, configurar la clave correspondiente:

```env
GEMINI_API_KEY=tu_clave_de_google_gemini
```

El agente Python utiliza esta configuración para realizar las consultas y análisis que dependen del proveedor.

Sin una clave válida, las funcionalidades de IA que dependen de Gemini pueden fallar.

No compartir el archivo `.env` ni publicar claves privadas.

---

## Instalación y ejecución con Docker

La ejecución integrada debe realizarse desde el directorio `backend/`, porque las rutas relativas del archivo Docker Compose hacen referencia a directorios del monorepositorio.

### 1. Construir las imágenes

```bash
docker compose build
```

### 2. Iniciar los servicios

```bash
docker compose up -d
```

También es posible construir e iniciar todo en un solo paso:

```bash
docker compose up --build -d
```

### 3. Verificar el estado

```bash
docker compose ps
```

### 4. Consultar los registros

```bash
docker compose logs -f
```

Para consultar únicamente la API principal:

```bash
docker compose logs -f api
```

Para consultar los registros de los demás servicios, utilizar sus nombres exactos definidos en `docker-compose.yml`.

### 5. Reiniciar los servicios

```bash
docker compose restart
```

### 6. Detener los servicios

```bash
docker compose down
```

---

## Puertos y servicios

| Servicio | Puerto |
|---|---:|
| API Gateway | 3000 |
| Worker ETL | 8000 |
| PostgreSQL | 5433 |
| Redis | 6380 |
| MinIO API | 9000 |
| MinIO Console | 9001 |
| Dashboard Backend | 3002 |
| Dashboard Agent | 8001 |

La base de datos del dashboard funciona dentro de la red de Docker y no requiere un puerto público para su funcionamiento integrado.

## Endpoints de verificación

| Servicio | Endpoint |
|---|---|
| API Gateway | `http://localhost:3000/health` |
| Swagger API | `http://localhost:3000/api/docs` |
| Worker ETL | `http://localhost:8000/health` |
| Worker Liveness | `http://localhost:8000/health/live` |
| Worker Readiness | `http://localhost:8000/health/ready` |
| Dashboard Backend | `http://localhost:3002/api/health` |
| Dashboard Agent | `http://localhost:8001/health` |
| Dashboard Agent Docs | `http://localhost:8001/docs` |

## Desarrollo individual del Worker ETL

El Worker utiliza Python 3.12 y el administrador de dependencias `uv`.

Desde el directorio del Worker:

```bash
cd worker
```

Instalar dependencias de desarrollo:

```bash
uv sync --extra dev
```

Iniciar el servidor:

```bash
uv run uvicorn src.main:app --reload --port 8000
```

Ejecutar pruebas:

```bash
uv run pytest
```

## Desarrollo individual de la API

La API principal utiliza Node.js, TypeScript y pnpm.

Las dependencias deben instalarse respetando la configuración del workspace definida en los archivos `package.json` y `pnpm-workspace.yaml` del backend.

Para consultar los scripts disponibles:

```bash
pnpm run
```

La ejecución y compilación deben utilizar los scripts definidos por el proyecto.

## Persistencia de datos

Los servicios de PostgreSQL y MinIO utilizan almacenamiento persistente configurado mediante Docker Compose.

Para detener los contenedores sin eliminar la información:

```bash
docker compose down
```

**Advertencia:** no utilizar `docker compose down -v` si se desea conservar las bases de datos y archivos almacenados. Ese comando elimina los volúmenes asociados.

## Solución de problemas

### Los servicios no inician

Consultar:

```bash
docker compose ps
docker compose logs -f
```

### Error de conexión con PostgreSQL

Comprobar:

- Que el contenedor de PostgreSQL esté activo.
- Que las variables de conexión sean correctas.
- Que los servicios utilicen el hostname interno definido en Docker Compose.
- Que el puerto local no esté ocupado.

### Error de conexión con Redis

Comprobar que Redis esté activo y que la URL de conexión corresponda a la configuración del entorno.

### Fallos en las funcionalidades de inteligencia artificial

Verificar que `GEMINI_API_KEY` esté configurada y que sea válida.

### Cambios en el código que no se reflejan

Reconstruir los servicios correspondientes:

```bash
docker compose up --build -d
```

## Repositorio

https://github.com/JuanSebastianRojasRivera/ProyectoFinal
