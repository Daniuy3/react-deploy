# React Deploy

Aplicación React + TypeScript creada con Vite. En producción se compila en una
primera etapa de Docker y el resultado estático se sirve con nginx en una
imagen ligera independiente.

## Requisitos

Para trabajar en local necesitas:

- Node.js 22 o superior.
- npm.
- Docker Engine y Docker Compose, para desplegar con contenedores.

## Ejecutar en desarrollo

Instala las dependencias y arranca el servidor de Vite:

```bash
npm ci
npm run dev
```

Abre la URL que muestra Vite, normalmente `http://localhost:5173`.

## Comprobar el proyecto

El build ejecuta primero el chequeo de TypeScript y después genera los archivos
de producción en `dist`:

```bash
npm run build
```

También puedes ejecutar el linter:

```bash
npm run lint
```

## Desplegar con Docker Compose

El despliegue utiliza dos etapas definidas en el `Dockerfile`:

1. `builder`: instala las dependencias y ejecuta `npm run build`.
2. `production`: copia `dist` a nginx y publica la aplicación por el puerto 80
   dentro del contenedor.

Construye la imagen y arranca el servicio con:

```bash
docker compose up --build -d
```

La aplicación estará disponible en:

```text
http://localhost:8080
```

El archivo `nginx.conf` incluye un fallback a `index.html`, necesario para que
las rutas de una aplicación SPA de React funcionen al recargar la página.

Para ver el estado y los logs:

```bash
docker compose ps
docker compose logs -f web
```

Para detener y eliminar el contenedor:

```bash
docker compose down
```

## Cambiar el puerto publicado

El puerto del equipo anfitrión se configura en `docker-compose.yml`. Por
ejemplo, para usar el puerto `3000`, cambia:

```yaml
ports:
  - "3000:80"
```

El puerto `80` a la derecha debe mantenerse porque nginx escucha en ese puerto
dentro del contenedor.