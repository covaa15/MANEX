# Manex 🗃️ — Guía de despliegue

Manex es una aplicación de gestión empresarial compuesta por dos partes independientes: **`core`** (API REST con Node.js/Express) y **`ui`** (frontend con React + Vite). Ambas se configuran mediante ficheros `.env` y se arrancan por separado.

---

## Requisitos previos

- **Node.js** `>=20.19.0` (requerido tanto por Vite 8 como por Express 5)
- **MariaDB** `>=12.2.2`
- **npm** `>=10`

---

## 1. Base de datos

Crea la base de datos MariaDB y aplica el esquema del proyecto. Las credenciales que uses en este paso son las que irán en el `.env` del backend.

---

## 2. Backend (`core`)

### 2.1 Instalación

```bash
cd core
npm install
```

### 2.2 Variables de entorno

Copia el fichero de ejemplo y rellena los valores:

```bash
cp .env.example .env
```

| Variable  | Descripción                        |
|-----------|------------------------------------|
| `DB_HOST` | Host del servidor MariaDB            |
| `DB_NAME` | Nombre de la base de datos         |
| `DB_USER` | Usuario de MariaDB                   |
| `DB_PASS` | Contraseña de MariaDB                |
| `DB_PORT` | Puerto de MariaDB (normalmente 3306) |

### 2.3 Arranque

```bash
npm start
```

El servidor escucha en `http://localhost:2341`.  
La documentación Swagger estará disponible en `http://localhost:2341/api-docs`.

---

## 3. Frontend (`ui`)

### 3.1 Instalación

```bash
cd ui
npm install
```

### 3.2 Variables de entorno

Copia el fichero de ejemplo y rellena los valores:

```bash
cp .env.example .env
```

| Variable        | Descripción                                              |
|-----------------|----------------------------------------------------------|
| `VITE_BACKEND`  | URL base del backend (ej. `http://localhost:2341`)       |

El resto de variables del `.env.example` son opcionales y están reservadas para sobrescribir rutas concretas de la API si fuera necesario.

### 3.3 Desarrollo

```bash
npm run dev
```

Arranca Vite en modo desarrollo con hot-reload en `http://localhost:5173`.

### 3.4 Producción

```bash
npm run build
npm run preview
```

`build` genera los estáticos en `ui/dist/`. `preview` sirve esa carpeta localmente para verificar el resultado antes de desplegar.  
Para producción real, sirve el contenido de `dist/` con cualquier servidor estático (Nginx, Apache, Vercel, etc.).

---

## 4. Estructura del proyecto

```
/
├── core/          # API REST (Express 5 + MySQL2)
│   ├── API/       # Controladores por recurso
│   ├── index.mjs  # Punto de entrada
│   └── .env.example
└── ui/            # Frontend (React 19 + Vite 8)
    ├── src/
    │   ├── routes/       # Páginas/vistas
    │   ├── components/   # Componentes reutilizables
    │   ├── context/      # Contexto de usuario y autenticación
    │   └── utils/        # Helpers y llamadas a la API
    ├── public/
    │   └── styles/       # CSS global
    └── .env.example
```

---

## 5. Resumen rápido

```bash
# Terminal 1 — Backend
cd core && cp .env.example .env   # edita .env con tus credenciales
npm install && npm start

# Terminal 2 — Frontend
cd ui && cp .env.example .env     # pon VITE_BACKEND=http://localhost:2341
npm install && npm run dev
```
