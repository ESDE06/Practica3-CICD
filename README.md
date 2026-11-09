# Práctica 3 — Pipeline CI/CD con GitHub Actions

**Asignatura:** Electiva 1 — Introducción a DevOps (ITLA)  
**Proyecto base:** plantilla oficial `plantilla-devops-itla.zip`

Aplicación web de lista de tareas con Node.js/Express, pruebas automatizadas Jest + Supertest, cobertura, Docker y publicación automática en GitHub Pages.

## 1. Requisitos

- Git y cuenta de GitHub.
- Node.js 20 o superior y npm.
- Repositorio público en GitHub.
- Docker Desktop (opcional para ejecutar el contenedor localmente).

## 2. Estructura

```text
plantilla-devops-itla/
├── .github/workflows/ci.yml   # Pipeline CI/CD
├── public/index.html          # Sitio estático publicado en Pages
├── src/app.js                 # API Express y reglas de tareas
├── src/server.js              # Inicio del servidor
├── tests/app.test.js          # Pruebas unitarias/de integración HTTP
├── docs/                      # Documentación de la práctica
├── Dockerfile                 # Imagen de producción
├── package.json               # Scripts y dependencias
└── README.md
```

## 3. Instalación y ejecución local

```bash
npm install
npm run lint
npm test
npm start
```

Abrir `http://localhost:3000`. Endpoints: `GET /health`, `GET /metrics`, `GET/POST /api/tareas`, `PATCH /api/tareas/:id` y `DELETE /api/tareas/:id`.

La API mantiene los datos en memoria; se reinician al detener el servidor. La página desplegada en GitHub Pages no tiene servidor Express, por eso cambia automáticamente a `localStorage` y guarda los datos en el navegador de quien la visita. GitHub Pages publica el frontend estático, no la API.

### Docker (opcional)

```bash
docker build -t tareas-devops-itla .
docker run --rm -p 3000:3000 tareas-devops-itla
```

Visita `http://localhost:3000` y `http://localhost:3000/health`.

## 4. Mejoras implementadas para la Práctica 3

- Se agregó `DELETE /api/tareas/:id` para eliminar una tarea.
- Se valida que el título no esté vacío y que no supere 100 caracteres.
- Se agregaron pruebas para eliminación exitosa, tarea inexistente y título demasiado largo.
- La interfaz ahora permite marcar tareas como completadas y eliminarlas; usa la API local o `localStorage` en Pages.
- Se conserva el Dockerfile y se fortalece el workflow con `npm install`, permisos mínimos y verificación del sitio.

## 5. Pipeline CI/CD

El workflow `.github/workflows/ci.yml` se ejecuta cuando se hace `push` o se abre/actualiza un `pull_request` dirigido a `main`.

1. **Checkout:** descarga el código del repositorio.
2. **Setup Node:** prepara Node.js 20.
3. **Instalación:** `npm install` instala las dependencias declaradas en `package.json`.
4. **Lint:** `npm run lint` verifica la sintaxis de los archivos JavaScript.
5. **Test:** `npm test` ejecuta Jest/Supertest y genera `coverage/`.
6. **Build Docker:** comprueba que la imagen de la aplicación se pueda construir.
7. **Deploy:** si todo pasa y el evento es un `push` a `main`, publica `public/` en GitHub Pages.

En los `pull_request` se valida el proyecto, pero no se despliega. Si una etapa falla, las etapas dependientes no continúan. Más detalle: [`docs/PIPELINE.md`](docs/PIPELINE.md).

## 6. Publicar en GitHub

1. Crea en GitHub un repositorio **público y vacío** (sin README, licencia ni `.gitignore` inicial).
2. Abre una terminal dentro de esta carpeta y ejecuta:

   ```bash
   git init
   git add .
   git commit -m "Completar práctica 3 CI/CD"
   git branch -M main
   git remote add origin https://github.com/TU_USUARIO/TU_REPOSITORIO.git
   git push -u origin main
   ```

3. En GitHub entra a **Settings → Pages** y selecciona **Source: GitHub Actions**.
4. Abre **Actions → CI/CD** y espera que los trabajos terminen en verde.
5. En el job de despliegue encontrarás la URL de GitHub Pages. Comparte esa URL junto con la del repositorio.

Si `origin` ya existe, no repitas `git remote add`; revisa con `git remote -v` y actualízalo si hace falta: `git remote set-url origin URL`.

## 7. Pruebas y cobertura

```bash
npm test
```

Jest muestra el resumen de cobertura en la terminal y crea el reporte HTML en `coverage/lcov-report/index.html`. La suite cubre salud, creación, validación, cambio de estado, eliminación, errores 404 y métricas.

## 8. Demostración requerida

Para evidenciar el funcionamiento del pipeline:

1. Haz un `push` inicial y muestra el workflow exitoso.
2. En una rama de demostración, modifica temporalmente una expectativa de una prueba para que sea incorrecta.
3. Ejecuta la prueba localmente para comprobar que falla y sube la modificación a GitHub.
4. En **Actions**, muestra que el job queda rojo y que el despliegue no se ejecuta.
5. Revierte la expectativa incorrecta, sube el arreglo y muestra el workflow verde.
6. No dejes la prueba alterada en la versión final.

No se incluye una ejecución ficticia ni capturas inventadas: los estados rojo/verde deben capturarse desde tu propio repositorio.

## 9. Documentos de apoyo

- [`docs/PIPELINE.md`](docs/PIPELINE.md): explicación técnica de cada etapa.
- [`docs/INFORME_IMPLEMENTACION.md`](docs/INFORME_IMPLEMENTACION.md): informe de implementación y resultados esperados.
- [`docs/GUIA_DEMO.md`](docs/GUIA_DEMO.md): guion de demostración y evidencias.
- [`CONTRIBUTING.md`](CONTRIBUTING.md): guía de contribución.

## 10. Limitaciones conocidas

- La API usa memoria, no una base de datos persistente.
- GitHub Pages aloja el frontend estático; no aloja el proceso Node.js/Express.
- La construcción Docker se valida en CI, pero la imagen no se publica en un registro.
- `npm run lint` es una comprobación de sintaxis con Node.js, no una configuración completa de ESLint.

Estas limitaciones mantienen el alcance de la práctica básica y pueden ampliarse en el proyecto final.
