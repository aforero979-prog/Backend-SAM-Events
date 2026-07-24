# Guía de Limpieza e Integración de Ramas en Git

Esta guía documenta los métodos para limpiar la rama principal (`main`) de un repositorio cuando el código existente es obsoleto o irreparable, y se requiere preparar la rama para integrar características desde otras ramas del proyecto.

---

## Opción 1: Reiniciar `main` desde cero (Rama Huérfana - *Recomendado*)

Esta estrategia crea una rama sin historial previo (orphan) para dejar un lienzo completamente limpio antes de realizar los merges.

### Paso a paso:

1. **Crear una rama huérfana temporal:**
   ```bash
   git checkout --orphan main-limpia
   ```

2. **Limpiar todos los archivos rastreados del área de trabajo:**
   ```bash
   git rm -rf .
   ```

3. **Crear el commit inicial (vacío o con archivos base):**
   ```bash
   git commit --allow-empty -m "Initial commit: Base limpia para integrar las ramas"
   ```

4. **Renombrar la rama a `main`:**
   ```bash
   git branch -M main
   ```

5. **Subir los cambios al remoto sobrescribiendo la historia anterior:**
   ```bash
   git push -f origin main
   ```
   > ⚠️ **Atención:** El uso del parámetro `-f` (`--force`) sobrescribirá permanentemente la historia de la rama `main` en el servidor remoto.

---

## Opción 2: Resetear `main` al punto de partida de una rama existente

Utiliza esta opción si una de tus ramas (por ejemplo `feature/authentication`) representa la base adecuada sobre la cual integrar las demás ramas.

### Paso a paso:

1. **Cambiar a la rama `main` y sincronizar el repositorio:**
   ```bash
   git checkout main
   git fetch origin
   ```

2. **Resetear `main` al estado exacto de la rama base:**
   ```bash
   git reset --hard origin/feature/authentication
   ```

3. **Publicar la actualización en el servidor remoto:**
   ```bash
   git push -f origin main
   ```

---

## Proceso de Integración (Merge de Ramas)

Una vez que la rama `main` esté limpia y configurada, sigue este flujo para consolidar las diferentes funcionalidades del proyecto:

1. **Descargar los últimos cambios del remoto:**
   ```bash
   git fetch origin
   ```

2. **Realizar el merge de cada rama funcional:**
   ```bash
   git merge origin/feature/authentication
   git merge origin/feature/user
   git merge origin/feature/categories
   git merge origin/feature/cities
   git merge origin/feature/events
   git merge origin/Locations
   ```

3. **Verificar que la aplicación compile y funcione correctamente tras cada integración.**

4. **Publicar los cambios finales integrados:**
   ```bash
   git push origin main
   ```
