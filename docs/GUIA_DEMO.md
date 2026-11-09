# Guía de demostración del pipeline

Duración sugerida: 5–7 minutos.

1. **Presentación (30 s):** explicar el objetivo y mostrar la estructura del repositorio.
2. **Aplicación (1 min):** ejecutar `npm ci`, `npm start` y abrir `http://localhost:3000`. Crear, completar y eliminar una tarea.
3. **Pruebas (1 min):** ejecutar `npm test`; mostrar que Jest ejecuta las pruebas y genera cobertura.
4. **Workflow (1 min):** abrir `.github/workflows/ci.yml` y explicar los disparadores, job de validación y job de Pages.
5. **Fallo controlado (1 min):** cambiar temporalmente una expectativa en `tests/app.test.js`, subir el cambio y mostrar el job rojo en GitHub Actions.
6. **Corrección (1 min):** restaurar la expectativa correcta, hacer commit/push y mostrar el job verde.
7. **Despliegue (30 s):** abrir la URL de GitHub Pages y demostrar la interfaz.

## Evidencias que debes capturar

- Resumen de `npm test` con cobertura.
- GitHub Actions con ejecución fallida (roja).
- GitHub Actions con ejecución exitosa (verde).
- URL pública de GitHub Pages abierta en el navegador.

**Importante:** el fallo se provoca solo durante la demostración y debe corregirse antes de entregar. No dejes el repositorio con una prueba intencionalmente incorrecta.
