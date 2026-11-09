# Documentación técnica del pipeline

## Disparadores

El workflow `CI/CD` se activa con `push` a `main` y con `pull_request` cuyo destino sea `main`. Esto permite validar tanto los cambios integrados como los propuestos antes de incorporarlos.

## Flujo de ejecución

| Orden | Paso | Acción | Resultado esperado |
|---|---|---|---|
| 1 | Checkout | `actions/checkout@v4` | Código disponible en el runner |
| 2 | Node.js | `actions/setup-node@v4`, Node 20 | Entorno preparado |
| 3 | Dependencias | `npm install` | Dependencias instaladas declaradas en `package.json` |
| 4 | Revisión | `npm run lint` | Sintaxis JavaScript válida |
| 5 | Pruebas | `npm test` | Pruebas Jest/Supertest y cobertura |
| 6 | Build | `docker build` | Imagen construida correctamente |
| 7 | Pages | configure/upload/deploy Pages | Sitio estático publicado |

`deploy-pages` depende de `build-test`, por lo que no empieza si falla una validación. Además, se ejecuta exclusivamente para `push` a `main`, nunca para pull requests.

## Permisos y seguridad

El permiso general es `contents: read`. El job de publicación recibe únicamente `pages: write` e `id-token: write`, necesarios para publicar mediante GitHub Pages. El repositorio debe configurar Pages con **Source: GitHub Actions**.

## Cobertura

`npm test` ejecuta Jest con `--coverage`. El reporte de consola permite revisar porcentajes; el HTML se encuentra en `coverage/lcov-report/index.html`. La cobertura se genera en el runner, pero no se publica como sitio ni se carga como artefacto en esta versión.

## Qué se despliega

Se carga únicamente la carpeta `public/`. GitHub Pages sirve HTML, CSS y JavaScript estáticos. No ejecuta `src/server.js`, por lo que la API Express no estará disponible en la URL pública. En Pages la interfaz utiliza `localStorage`; en local, con Express, usa la API REST.

## Diagnóstico rápido

- **`npm install` falla:** confirma que `package-lock.json` esté versionado y que Node sea 20.
- **Pruebas fallan:** ejecuta `npm test` localmente y revisa la primera expectativa fallida.
- **Docker build falla:** ejecuta `docker build -t tareas-devops-itla .` localmente.
- **Pages no publica:** confirma `Settings → Pages → Source: GitHub Actions` y revisa permisos del job.
- **El workflow no se activa:** confirma que el archivo esté en `.github/workflows/ci.yml` y que la rama se llame `main`.
