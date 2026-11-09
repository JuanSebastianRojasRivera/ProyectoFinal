# 🚀 Proyecto PYMES

> Planteamiento de proyecto: Despliegue Dashboards + Visualización de Negocio + Asistente Gemini.

![Estado](https://img.shields.io/badge/Versión-INTEGRADA%20V1-orange)
![API Tests](https://img.shields.io/badge/Backend%20Tests-337%20passed-brightgreen)
![Worker Tests](https://img.shields.io/badge/Worker%20Tests-308%20passed-brightgreen)
![Stack](https://img.shields.io/badge/Stack-Next.js%20|%20Node%20|%20Python-blue)
![IA](https://img.shields.io/badge/IA-Gemini%202.5%20Flash-violet)
![Licencia](https://img.shields.io/badge/Licencia-MIT-blue)

---


## 📁 Estructura del Proyecto

```
PymesDataStrategy-Monorepo/
├── frontend/                # Interfaz Next.js 15
│   ├── app/                 # App Router (Dashboard, Landing, Auth)
│   ├── components/          # UI Neo-Brutalista y features integradas
│   └── hooks/api/           # Integración con Backend y Proxy Analytics
├── backend/                 # Infraestructura de Limpieza
│   ├── api/                 # API Gateway (TypeScript + Hexagonal)
│   ├── worker/              # ETL Processor (Python + Polars)
│   └── prisma/              # Modelado de base de datos HITL
├── services/
│   └── dashboard-pymes/     # Capa de Analítica (Deconstruida)
│       ├── backend/         # Motor de KPIs y Stats (Node.js)
│       └── agent/           # Cerebro estadístico y Gemini (Python)
├── docs/                    # Documentación técnica y logs
├── docker-compose.yml       # Orquestador de 9 servicios
└── README.md                # Este archivo
```

---

## 🚀 Guía de Instalación (Inicio Rápido)

### 1. Prerrequisitos
- Docker Desktop instalado.
- Puertos libres: 3000, 3001, 3002, 5432, 6379, 8000, 8001, 9000, 9001.

### 2. Clonar y Configurar
```bash
git clone https://github.com/JuanSebastianRojasRivera/ProyectoFinalInternetDelasCosas.git
cd ProyectoFinalInternetDelasCosas
```

Crea un archivo `.env` en `backend/` (usa `.env.example` como guía).
**Obligatorio:**
- `GEMINI_API_KEY`: Consíguela en [Google AI Studio](https://aistudio.google.com/app/apikey).
- `NEXTAUTH_SECRET`: Generada con `openssl rand -base64 32`.

### 3. Despliegue con Docker
```bash
cd backend
docker compose up --build -d
```

### 4. Acceso
- **Plataforma:** [http://localhost:3001](http://localhost:3001)
- **API Docs:** [http://localhost:3000/api/docs](http://localhost:3000/api/docs)

---
