# PymesDataStrategy

**Plataforma inteligente de gestión, procesamiento y análisis de datos para pequeñas y medianas empresas.**

PymesDataStrategy es una plataforma tecnológica que integra procesos ETL, inteligencia artificial y herramientas de análisis para facilitar la gestión y transformación de información empresarial.

La solución permite cargar conjuntos de datos, ejecutar procesos de procesamiento, identificar anomalías, aplicar transformaciones y supervisar los resultados mediante mecanismos de validación humana (*Human-in-the-Loop*).

---

## Características principales

- **Gestión de datasets:** carga, organización y consulta de conjuntos de datos.
- **Procesamiento ETL:** extracción, transformación y procesamiento de información.
- **Detección de anomalías:** identificación de inconsistencias y valores atípicos.
- **Inteligencia artificial:** asistencia para el análisis y transformación de datos.
- **Human-in-the-Loop:** revisión, aprobación, corrección y descarte de anomalías.
- **Procesamiento automatizado:** ejecución y seguimiento de trabajos ETL.
- **Analítica empresarial:** consulta de estadísticas e información de las empresas.
- **Almacenamiento de archivos:** gestión de archivos mediante MinIO.
- **Actualizaciones de procesos:** seguimiento de trabajos y estados de ejecución.

## Arquitectura general

El sistema utiliza una arquitectura modular organizada en diferentes servicios:

| Componente | Responsabilidad |
|---|---|
| Frontend | Interfaz web para interactuar con la plataforma. |
| API Gateway | Gestión de solicitudes y lógica principal del backend. |
| Worker ETL | Procesamiento y transformación de datos. |
| PostgreSQL | Persistencia de información del sistema principal. |
| Redis | Gestión de colas y comunicación de trabajos. |
| MinIO | Almacenamiento de archivos y objetos. |
| Dashboard Backend | Servicios de autenticación, empresas, carga y estadísticas. |
| Dashboard Agent | Agente de inteligencia artificial para consultas y análisis. |
| Dashboard Database | Persistencia de información del dashboard. |
| MinIO Init | Inicialización de la configuración de almacenamiento. |

## Tecnologías utilizadas

**Frontend**
- Next.js 16.
- React.
- TypeScript.
- Tailwind CSS.
- Zustand.
- React Query.
- NextAuth.

**Backend**
- Node.js 22.
- TypeScript.
- Prisma.
- PostgreSQL.
- Redis.
- BullMQ.

**Procesamiento e inteligencia artificial**
- Python 3.12.
- FastAPI.
- Uvicorn.
- Polars / Pandas.
- Google Gemini.

**Infraestructura**
- Docker.
- Docker Compose.
- MinIO.

## Estructura del repositorio

```text
ProyectoFinal/
│
├── backend/
│   ├── api/
│   │   ├── src/
│   │   └── Dockerfile.dev
│   │
│   ├── worker/
│   │   ├── src/
│   │   ├── pyproject.toml
│   │   └── Dockerfile.dev
│   │
│   ├── docker/
│   │   └── postgres/
│   │       └── init.sql
│   │
│   ├── docker-compose.yml
│   ├── package.json
│   ├── .env.example
│   └── README.md
│
├── frontend/
│   ├── public/
│   ├── src/
│   ├── package.json
│   ├── next.config.ts
│   ├── Dockerfile.dev
│   └── README.md
│
├── services/
│   └── dashboard-pymes/
│       ├── backend/
│       └── agent/
│
├── mockups/
│
└── README.md
```

## Requisitos previos

Para ejecutar la plataforma mediante contenedores se necesita:

- Git.
- Docker Desktop.
- Docker Compose v2.
- Conexión a internet para descargar imágenes y dependencias.

No es necesario instalar PostgreSQL, Redis, Node.js o Python para iniciar la plataforma completa mediante Docker.

## Instalación y ejecución

### 1. Clonar el repositorio

```bash
git clone https://github.com/JuanSebastianRojasRivera/ProyectoFinal.git
```

### 2. Ingresar al proyecto

```bash
cd ProyectoFinal
```

### 3. Ingresar al directorio del backend

El archivo Docker Compose se encuentra dentro de `backend/`. Por esta razón, los comandos de Docker Compose deben ejecutarse desde ese directorio.

```bash
cd backend
```

### 4. Configurar las variables de entorno

Crear el archivo `.env` a partir de la plantilla:

**Linux / macOS:**

```bash
cp .env.example .env
```

**Windows PowerShell:**

```powershell
Copy-Item .env.example .env
```

Revisar el archivo `.env` y completar las variables necesarias para el entorno local.

Para utilizar las funcionalidades de inteligencia artificial, configurar la clave `GEMINI_API_KEY` cuando corresponda.

No publicar el archivo `.env` ni compartir credenciales privadas.

### 5. Construir e iniciar la plataforma

Desde el directorio `backend/` ejecutar:

```bash
docker compose up --build -d
```

Este comando construye las imágenes e inicia los servicios definidos en `docker-compose.yml`.

### 6. Verificar los contenedores

```bash
docker compose ps
```

Comprobar que los servicios se encuentren en ejecución y revisar su estado.

### 7. Consultar los registros

Para consultar los registros de todos los servicios:

```bash
docker compose logs -f
```

Para consultar un servicio específico:

```bash
docker compose logs -f api
```

El nombre del servicio debe coincidir con el definido en el archivo Docker Compose.

---

## Direcciones de acceso

| Servicio | Dirección |
|---|---|
| Aplicación web | http://localhost:3001 |
| API Gateway | http://localhost:3000 |
| Documentación Swagger | http://localhost:3000/api/docs |
| Health Check API | http://localhost:3000/health |
| Worker ETL | http://localhost:8000 |
| MinIO API | http://localhost:9000 |
| MinIO Console | http://localhost:9001 |
| Dashboard Backend | http://localhost:3002 |
| Dashboard Agent | http://localhost:8001 |
| Dashboard Agent Docs | http://localhost:8001/docs |

La base de datos del dashboard funciona como servicio interno y no requiere una interfaz web para utilizar la plataforma.

## Verificación del funcionamiento

Después de iniciar los contenedores, se recomienda realizar las siguientes comprobaciones:

1. Consultar el estado de los contenedores con `docker compose ps`.
2. Abrir la aplicación web en el navegador.
3. Consultar la documentación Swagger de la API.
4. Verificar el endpoint `/health` de la API principal.
5. Verificar el endpoint `/health` del Worker ETL.
6. Consultar el endpoint `/api/health` del Dashboard Backend.
7. Verificar el endpoint `/health` del Dashboard Agent.
8. Comprobar que MinIO esté disponible en el puerto 9001.

La disponibilidad de cada servicio depende de su configuración y de que haya iniciado correctamente.

## Detener la plataforma

Para detener los servicios sin eliminar los datos persistentes:

```bash
docker compose down
```

Para reconstruir las imágenes:

```bash
docker compose up --build -d
```

**Importante:** el siguiente comando elimina también los volúmenes y puede borrar las bases de datos y los archivos almacenados:

```bash
docker compose down -v
```

Utilizarlo únicamente cuando se quiera reiniciar el entorno y se tenga claro que se perderán los datos persistentes.

## Configuración de inteligencia artificial

La plataforma integra Google Gemini para determinadas funcionalidades de análisis y asistencia mediante inteligencia artificial.

Para habilitar las funciones que dependen de este proveedor, configurar la variable correspondiente:

```env
GEMINI_API_KEY=tu_clave_de_google_gemini
```

La variable `GEMINI_MODEL` permite configurar el modelo cuando el servicio la contempla.

Sin una clave válida, los servicios pueden iniciar, pero las operaciones que dependen de Gemini no estarán disponibles correctamente.

## Documentación adicional

- [Documentación del Backend](backend/README.md)
- [Documentación del Frontend](frontend/README.md)

## Repositorio

https://github.com/JuanSebastianRojasRivera/ProyectoFinal

---

**PymesDataStrategy**  
Plataforma de procesamiento inteligente y análisis de datos empresariales.
