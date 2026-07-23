# Flujo de trabajo Git para el agente

Dos modalidades. Yo indico cuál usar al arrancar cada tarea diciendo
**"modalidad 1"** o **"modalidad 2"**. Si no lo aclaro, preguntá antes de tocar git.

**No usás `gh`** (no está instalado). La PR se abre por URL, no por línea de comandos.

## Convenciones fijas
- Rama base: `main`
- Changelog: `CHANGELOG.md` (formato *Keep a Changelog*, sección `Unreleased`)
- Nombre de rama: `<tipo>/<slug-corto>` → `feature/`, `fix/`, `chore/`, `docs/`
- Mensaje de commit: *Conventional Commits* (`feat:`, `fix:`, `chore:`…)

---

## Modalidad 1 — Feature branch + PR (merge manual mío)

### Fase A — vos preparás la PR (turno automático)

1. Sincronizar la base:
   ```bash
   git checkout main
   git pull --ff-only origin main
   ```
2. Crear la rama:
   ```bash
   git checkout -b <tipo>/<slug>
   ```
3. Actualizar `CHANGELOG.md` (sección `Unreleased`, tipo correspondiente:
   Added / Changed / Fixed / Removed).
4. Stage + commit:
   ```bash
   git add -A
   git commit -m "<tipo>: <descripción>"
   ```
5. Push con upstream:
   ```bash
   git push -u origin <tipo>/<slug>
   ```
6. Preparar la PR **sin `gh`**:
   - Derivá `<owner>/<repo>` de `git remote get-url origin`.
   - Mostrame la URL de compare ya expandida:
     ```
     https://github.com/<owner>/<repo>/compare/main...<tipo>/<slug>?expand=1
     ```
   - Redactame título y cuerpo de la PR a partir del changelog/commit,
     para que los pegue.
   - (Opcional, estoy en Linux) podés abrirla con `xdg-open "<url>"`.
   - Ahí termina tu turno. **No mergees vos.**

### Fase B — yo actúo en GitHub (manual)
Reviso, hago **Merge** de la PR y **borro la rama remota** desde la UI de GitHub.

### Fase C — limpieza (cuando yo te diga "ya mergeé")

7. Volver y sincronizar:
   ```bash
   git checkout main
   git pull --ff-only origin main
   ```
8. Borrar la rama local con verificación de merge:
   ```bash
   git branch -d <tipo>/<slug>
   ```
   Si falla con *"not fully merged"* (pasa con **Squash** o **Rebase merge**:
   el contenido está en main pero los SHAs difieren), confirmá que no haya
   diferencias reales y recién ahí forzá:
   ```bash
   git diff --quiet main..<tipo>/<slug> && git branch -D <tipo>/<slug>
   ```
   Si `git diff --quiet` devuelve error (hay diffs), **no borres**: avisame.
9. Limpiar las refs remotas ya borradas:
   ```bash
   git fetch --prune
   ```
10. Quedar en `main` limpio y sincronizado, listo para el próximo cambio.
    **No** crees la rama siguiente hasta que yo defina la próxima tarea.

---

## Modalidad 2 — Directo a `main` (sin rama, sin PR)

1. Sincronizar:
   ```bash
   git checkout main
   git pull --ff-only origin main
   ```
2. Actualizar `CHANGELOG.md`.
3. Stage + commit:
   ```bash
   git add -A
   git commit -m "<tipo>: <descripción>"
   ```
4. Push directo:
   ```bash
   git push origin main
   ```

Sin rama nueva, sin PR, sin merge. Si el push se rechaza por *branch
protection*, avisame — main está protegida y esta modalidad no aplica.

---

## Reglas transversales
- Nunca uses `git push --force` sobre `main`.
- Nunca borres una rama sin verificar antes que su contenido esté en `main`.
- Un cambio = un commit lógico (no acumules cambios no relacionados en la misma rama).
- Si `git pull --ff-only` falla, hay divergencia: pará y avisame, no hagas merge automático.
