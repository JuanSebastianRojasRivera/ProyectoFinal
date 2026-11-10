# PymesDataStrategy - Frontend

El frontend de PymesDataStrategy es la aplicación web que permite a los usuarios interactuar con las funcionalidades de gestión, procesamiento y análisis de datos empresariales.

Está desarrollado con Next.js, React y TypeScript, y se comunica con los servicios backend para gestionar conjuntos de datos, supervisar procesos ETL y realizar actividades de revisión y validación de información.

---

## Funcionalidades principales

### 1. Landing Page

Página inicial de la plataforma que presenta sus características y permite acceder a las funcionalidades disponibles.

### 2. Autenticación

Permite acceder a la aplicación mediante los mecanismos de autenticación implementados.

Incluye la integración con NextAuth y los servicios de autenticación correspondientes.

### 3. Dashboard principal

Proporciona una vista general de la información y de las funcionalidades de la plataforma.

Permite acceder a los diferentes módulos de gestión y consultar información relevante.

### 4. Gestión de datasets

Permite trabajar con los conjuntos de datos utilizados por la plataforma.

Entre sus funcionalidades se encuentran:

- Consulta de conjuntos de datos.
- Carga de información.
- Acceso a los datos registrados.
- Interacción con los procesos de análisis y transformación.

### 5. Gestión de trabajos ETL

Permite consultar y supervisar los trabajos de procesamiento.

Incluye mecanismos de seguimiento de estados y actualizaciones de los procesos mediante Server-Sent Events (SSE), según la integración implementada.

### 6. Validación Human-in-the-Loop

Proporciona una interfaz para revisar anomalías y resultados de procesamiento.

Permite participar en los flujos de revisión humana, incluyendo la aprobación, corrección o descarte de los elementos que requieren validación.

### 7. Analítica empresarial

Permite consultar información y métricas relacionadas con los datos procesados y los servicios de analítica.

### 8. Configuración

Proporciona acceso a las opciones de configuración disponibles en la aplicación.

---

## Tecnologías utilizadas

| Tecnología | Función |
|---|---|
| Next.js 16 | Framework de aplicación web. |
| React | Construcción de interfaces. |
| TypeScript | Tipado estático. |
| Tailwind CSS | Estilos de la interfaz. |
| Zustand | Gestión de estado. |
| React Query | Gestión de solicitudes y datos remotos. |
| NextAuth | Autenticación. |
| Vitest | Pruebas automatizadas. |
| Playwright | Pruebas de extremo a extremo. |

## Estructura del directorio

```text
frontend/
│
├── public/
│
├── src/
│
├── package.json
├── next.config.ts
├── Dockerfile.dev
└── README.md
```

## Requisitos previos

Para ejecutar el frontend de manera individual:

- Node.js 22.
- pnpm.
- Git.

Para ejecutarlo integrado con el resto de la plataforma:

- Docker Desktop.
- Docker Compose v2.

## Instalación local

### 1. Ingresar al directorio

Desde la raíz del repositorio:

```bash
cd frontend
```

### 2. Instalar pnpm

Si todavía no está disponible en el entorno:

```bash
npm install -g pnpm
```

### 3. Instalar dependencias

```bash
pnpm install
```

### 4. Iniciar el entorno de desarrollo

```bash
pnpm dev
```

El puerto de desarrollo debe corresponder a la configuración del proyecto. En la ejecución integrada mediante Docker Compose, la aplicación se publica en:

http://localhost:3001

---

## Ejecución integrada mediante Docker

La forma recomendada de iniciar el frontend junto con los servicios backend es utilizar Docker Compose.

El archivo de configuración se encuentra en:

```text
backend/docker-compose.yml
```

Por tanto, los comandos deben ejecutarse desde el directorio `backend/`.

### 1. Ingresar al backend

Desde la raíz del repositorio:

```bash
cd backend
```

### 2. Configurar variables de entorno

Crear el archivo `.env` utilizando la plantilla:

```bash
cp .env.example .env
```

En Windows PowerShell:

```powershell
Copy-Item .env.example .env
```

Revisar la configuración y completar los valores necesarios.

### 3. Construir e iniciar los servicios

```bash
docker compose up --build -d
```

### 4. Verificar el estado

```bash
docker compose ps
```

### 5. Consultar los registros

```bash
docker compose logs -f
```

Una vez iniciado correctamente el frontend, acceder a:

http://localhost:3001

---

## Integración con el backend

El frontend se comunica con los servicios backend para obtener y procesar la información de la plataforma.

### API Gateway

La API principal se encuentra disponible en:

http://localhost:3000

Su documentación interactiva está disponible en:

http://localhost:3000/api/docs

### Worker ETL

Servicio encargado del procesamiento de datos:

http://localhost:8000

### Dashboard Backend

Servicio de gestión de empresas, autenticación, carga y estadísticas:

http://localhost:3002

### Dashboard Agent

Servicio de inteligencia artificial para consultas y análisis:

http://localhost:8001

La comunicación entre contenedores utiliza las direcciones internas y los nombres de servicio definidos en Docker Compose. Las direcciones `localhost` se utilizan para acceder a los servicios desde el navegador o desde el equipo local.

## Compilación para producción

Para generar la compilación de producción:

```bash
pnpm build
```

Para iniciar la aplicación en modo producción:

```bash
pnpm start
```

Estos comandos deben ejecutarse desde el directorio `frontend/` y después de instalar las dependencias.

## Pruebas automatizadas

El proyecto incorpora herramientas para realizar pruebas de software.

### Vitest

Se utiliza para ejecutar pruebas automatizadas de componentes y lógica de la aplicación.

### Playwright

Se utiliza para realizar pruebas de extremo a extremo y validar los flujos de interacción.

Consultar los scripts disponibles en `package.json`:

```bash
pnpm run
```

Ejecutar las pruebas utilizando los scripts definidos en ese archivo.

## Configuración y seguridad

La aplicación utiliza configuraciones de seguridad definidas en `next.config.ts`.

Entre ellas se encuentran:

- Protección contra la inclusión de la aplicación en marcos externos.
- Configuración de políticas de referencia.
- Restricciones de permisos del navegador.
- Política de seguridad de contenido (Content Security Policy).
- Configuración de encabezados HTTP.

Las variables de entorno deben configurarse de acuerdo con los servicios utilizados por el frontend.

No se deben incluir claves privadas ni credenciales sensibles directamente en el código fuente.

## Solución de problemas

### El frontend no inicia

Verificar las dependencias:

```bash
pnpm install
```

Consultar los scripts disponibles:

```bash
pnpm run
```

### Error de comunicación con el backend

Comprobar que los servicios estén activos:

```bash
docker compose ps
```

Verificar que la API principal responda:

http://localhost:3000/health

### Los cambios no se reflejan

Reiniciar el entorno de desarrollo o reconstruir el contenedor:

```bash
docker compose up --build -d
```

### Error de dependencias

Comprobar la versión de Node.js:

```bash
node --version
```

Comprobar la versión de pnpm:

```bash
pnpm --version
```

## Repositorio

https://github.com/JuanSebastianRojasRivera/ProyectoFinal

---

**PymesDataStrategy**  
Interfaz web para la gestión y análisis inteligente de datos empresariales.
