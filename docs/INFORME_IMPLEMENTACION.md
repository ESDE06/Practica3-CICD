# Informe de implementación — Práctica 3 CI/CD

## 1. Introducción

Esta práctica implementa un flujo básico de integración y entrega continua (CI/CD) para una aplicación web sencilla de gestión de tareas. Se partió de la plantilla oficial del curso y se amplió para demostrar validación automática, pruebas de software, construcción de contenedor y publicación de un sitio estático mediante GitHub Actions.

## 2. Herramientas utilizadas

- **Git y GitHub:** control de versiones y repositorio remoto.
- **Node.js 20 y npm:** ejecución de la aplicación y administración de dependencias.
- **Express:** API REST para las tareas y endpoints de salud y métricas.
- **Jest y Supertest:** pruebas automatizadas de la API HTTP.
- **Docker:** construcción de una imagen reproducible de la aplicación.
- **GitHub Actions:** automatización del pipeline.
- **GitHub Pages:** alojamiento del frontend estático.

## 3. Implementación de la aplicación

La aplicación permite consultar, crear, completar y eliminar tareas. El servidor Express expone `GET /health`, `GET /metrics`, `GET /api/tareas`, `POST /api/tareas`, `PATCH /api/tareas/:id` y `DELETE /api/tareas/:id`. Se valida que el título sea obligatorio y no exceda 100 caracteres. La interfaz puede trabajar con la API local y, cuando se publica en Pages, usa almacenamiento local del navegador.

## 4. Estrategia de pruebas

Se utiliza Jest para organizar los casos y Supertest para realizar solicitudes HTTP contra la aplicación sin levantar un puerto real. Se prueban el endpoint de salud, creación correcta, validación de título vacío y largo, cambio de estado, respuestas 404 al modificar o eliminar recursos inexistentes, eliminación correcta y métricas. La ejecución con `npm test` genera el reporte de cobertura.

## 5. Automatización CI/CD

El workflow se activa ante `push` y `pull_request` hacia `main`. El job de validación descarga el repositorio, configura Node 20, instala dependencias mediante `npm install`, revisa sintaxis, ejecuta pruebas y construye la imagen Docker. Cuando el evento es un `push` a `main` y las validaciones terminan correctamente, un segundo job publica la carpeta `public/` en GitHub Pages.

## 6. Verificación y evidencias

La verificación debe realizarse desde el repositorio real del estudiante. Se recomienda incluir en la entrega capturas de: (1) pruebas locales con su resumen de cobertura; (2) ejecución verde del workflow; (3) ejecución roja provocada intencionalmente al alterar una expectativa de prueba; (4) ejecución verde después de corregirla; y (5) página pública desplegada. Las capturas se deben obtener después de subir el proyecto a GitHub; no se sustituyen por imágenes simuladas.

## 7. Resultados y limitaciones

El pipeline automatiza controles repetibles y evita publicar una versión cuando las validaciones fallan. El frontend se despliega en GitHub Pages. La API y sus datos en memoria funcionan al ejecutar Express localmente; Pages no ejecuta servidores Node.js, por lo que la versión pública almacena las tareas en `localStorage`. La imagen Docker se construye para comprobar que el contenedor es válido, pero no se publica en un registro.

## 8. Conclusión

La implementación demuestra el ciclo básico de CI/CD: cada cambio se valida automáticamente y la rama principal se publica solo después de superar las etapas de calidad y construcción. La separación entre validación y despliegue facilita detectar errores antes de entregar una nueva versión. Como ampliación futura se podría incorporar persistencia con base de datos, análisis ESLint, umbrales mínimos de cobertura y publicación de la imagen en GitHub Container Registry.
