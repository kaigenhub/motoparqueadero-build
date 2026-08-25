# MotoParqueadero Build

Repositorio público dedicado únicamente a compilar y publicar MotoParqueadero.

El código fuente permanece en el repositorio privado `kaigenhub/motoparqueadero`. Este repositorio no contiene el código ni las claves privadas: el workflow lo descarga usando secretos cifrados de GitHub y publica los instaladores en `kaigenhub/motoparqueadero-releases`.

## Secretos requeridos

En **Settings → Secrets and variables → Actions** deben existir:

- `SOURCE_REPO_TOKEN`: token fine-grained con acceso `Contents: Read-only` únicamente al repositorio privado `motoparqueadero`.
- `RELEASES_TOKEN`: token fine-grained con acceso `Contents: Read and write` únicamente a `motoparqueadero-releases`.
- `TAURI_SIGNING_PRIVATE_KEY`: contenido de la clave privada de firma de actualizaciones.
- `TAURI_SIGNING_PRIVATE_KEY_PASSWORD`: contraseña de la clave, si tiene una.

Las claves nunca deben guardarse en este repositorio.

## Publicar una versión

1. Crear y subir el tag correspondiente en el repositorio privado, por ejemplo `v0.2.4`.
2. Ir a **Actions → Compilar y publicar MotoParqueadero → Run workflow**.
3. Escribir la versión sin `v`, por ejemplo `0.2.4`.
4. El workflow compila macOS universal y Windows x64 NSIS y publica los archivos en `motoparqueadero-releases`.

También se puede iniciar desde una terminal con:

```bash
gh workflow run release.yml \
  --repo kaigenhub/motoparqueadero-build \
  -f version=0.2.4
```

Los runners estándar de este repositorio público no consumen minutos facturables de GitHub Actions.

## Arquitectura

- `motoparqueadero`: código fuente privado.
- `motoparqueadero-build`: workflow público de compilación.
- `motoparqueadero-releases`: instaladores y archivos de actualización públicos.

El repositorio público de compilación no contiene el código fuente privado; lo obtiene durante la ejecución con `SOURCE_REPO_TOKEN`.
