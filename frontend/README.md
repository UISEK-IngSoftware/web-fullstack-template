# React: Pokédex

## Descripción General
Este repositorio contiene tres laboratorios progresivos para construir una aplicación Pokédex completa usando React, Material UI y consumo de APIs REST.

---

## Laboratorio 9: Introducción a React, Componentes y MUI

### Objetivo
Construir el layout de una aplicación Pokédex utilizando React y Material UI. Se trabajará con datos simulados y se crearán componentes reutilizables.

### Requisitos previos
- Node.js y npm instalados
- Editor de código (recomendado: VS Code)
- Navegador actualizado (recomendado: Chrome)
- React (usar Vite)

### Requisitos técnicos
- Material UI (@mui/material, @emotion/react, @emotion/styled)
- Datos simulados (sin consumo de API)

### Estructura del proyecto
```
/src
  /components
    Header.jsx
    PokemonCard.jsx
  /pages
    App.jsx
  /data
    pokemons.js
  main.jsx
```

---

## Laboratorio 10: Llamadas a API REST con Axios

### Objetivo
Integrar llamadas a una API REST generada en Django (laboratorios 5-8 en otro repositorio) para consumir datos de Pokémon en tiempo real.

### Requisitos técnicos
- Axios para realizar peticiones HTTP
- Variables de entorno (.env) para configurar URLs de API
- Servicios reutilizables para las llamadas a API

### Nuevas características
- Consumo de endpoint `/api/pokemons/` para obtener lista de Pokémon
- Configuración de URL base de API en variables de entorno
- Creación de servicios en `pokemonService.js`

### Estructura actualizada
```
/src
  /components
    #### Tus componentes irán aquí
  /pages
    App.jsx
  /services
    #### Tus servicios irán aquí
  .env
  main.jsx
```

### Variables de entorno (.env)
```
VITE_API_BASE_URL=http://localhost:8000
VITE_API_MEDIA_URL=${VITE_API_BASE_URL}/media/
```

---

## Laboratorio 11: Llamadas a API REST con Axios y Gestión de Autorización

### Objetivo
Implementar un sistema completo de autenticación OAuth con Django, incluyendo login, logout y protección de rutas.

### Requisitos técnicos
- Autenticación OAuth con Django
- Gestión de tokens de acceso en localStorage
- Interceptores de Axios para agregar tokens a las peticiones
- Rutas protegidas con React Router
- Formulario de login y creación de Pokémon

### Nuevas características
- Página de login con autenticación OAuth (`/login`)
- Página para agregar Pokémon (`/add-pokemon`)
- Sistema de logout con revocación de token
- Protección de rutas basada en autenticación
- Conversión de imágenes a base64
- Interceptores de Axios para autorización

### Estructura actualizada
```
/src
  /components
    Header.jsx
    PokemonCard.jsx
  /pages
    App.jsx
    Login.jsx
    AddPokemon.jsx
  /services
    pokemonService.js
  .env
  main.jsx
```

### Variables de entorno (.env)
```
VITE_API_BASE_URL=http://localhost:8000
VITE_API_MEDIA_URL=${VITE_API_BASE_URL}/media/
VITE_API_CLIENT_ID=tu_client_id
VITE_API_CLIENT_SECRET=tu_client_secret
```

---

## Instalación del proyecto (con Docker, sin instalar Node)

1. **Clonar el repositorio** (este paso lo hará GitHub Classroom automáticamente).
2. Instalar Docker Desktop (Windows / macOS) o Docker Engine + Compose (Linux).
3. Abrir en VS Code la carpeta del repositorio y, desde la **raíz**, ejecutar:
   ```bash
   docker compose up --build
   ```
4. Abrir <http://localhost:5173>. Vite recarga la página al guardar cambios.

Las dependencias base (React, Material UI, Axios) ya están en `package.json` y se instalan al construir la imagen.

> 💡 **Nota:** El navegador (tu equipo) llama al backend en `http://localhost:8000`, por eso `VITE_API_BASE_URL` usa `localhost` y no el nombre del servicio `backend`.

### Comandos útiles con Docker Compose
Ejecutar siempre desde la raíz del repositorio. Funcionan igual en Linux, macOS y Windows.

- Levantar solo el frontend (y sus dependencias)
    ```bash
    docker compose up frontend
    ```
- Ver logs
    ```bash
    docker compose logs -f frontend
    ```
- Instalar una dependencia nueva (p. ej. React Router, necesario desde el Laboratorio 11)
    ```bash
    docker compose exec frontend npm install react-router-dom
    ```
  Se actualiza `package.json` en tu carpeta, pero si el contenedor se recrea ejecuta `docker compose up -d --build -V frontend`.
- Ejecutar el linter
    ```bash
    docker compose exec frontend npm run lint
    ```
- Generar el build de producción
    ```bash
    docker compose exec frontend npm run build
    ```
- Comprobar versión de dependencias
    ```bash
    docker compose exec frontend npm list
    ```
- Limpiar dependencias y reconstruir
    ```bash
    docker compose build --no-cache frontend
    docker compose up -d -V frontend
    ```
- Abrir una terminal dentro del contenedor
    ```bash
    docker compose exec frontend sh
    ```

### Problemas frecuentes
- **No recarga al guardar (Windows):** en `vite.config.js` agrega `server: { watch: { usePolling: true } }`.
- **Falta un paquete tras hacer `git pull`:** ejecuta `docker compose up -d --build -V frontend`.
- **Puerto 5173 ocupado:** cierra otro Vite local o cambia el mapeo `"5173:5173"`.

### Alternativa sin Docker (opcional)
Requiere Node.js 20.19 o superior:
```bash
cd frontend
npm install
npm run dev
```

### Comandos git
- Verificar los archivos modificados
    ```bash
    git status
    ```
- Agregar archivos al área de preparación
    ```bash
    git add .
    ```
- Realizar un commit
    ```bash
    git commit -m "Laboratorio [9/10/11]: descripción de cambios"
    ```
- Enviar los cambios a github
    ```bash
    git push
    ```
