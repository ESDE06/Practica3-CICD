# Guía de contribución

1. Crea una rama para cada cambio: `git switch -c feature/descripcion`.
2. Instala dependencias con `npm install`.
3. Antes de confirmar cambios, ejecuta `npm run lint` y `npm test`.
4. Mantén o agrega pruebas cuando cambies el comportamiento de la API.
5. Usa mensajes de commit descriptivos, por ejemplo: `feat: agregar eliminación de tareas`.
6. Sube la rama y abre un Pull Request hacia `main`.
7. Comprueba que el workflow CI/CD termine correctamente antes de integrar.

No subas `node_modules/`, `coverage/`, archivos `.env` ni credenciales al repositorio.
