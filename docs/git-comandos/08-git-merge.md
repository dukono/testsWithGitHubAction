# 8. git merge - Integrando Cambios

[🏠 Volver al Índice](../../GIT_COMANDOS_GUIA_PRACTICA.md)

---

## 8. git merge - Integrando Cambios
[⬆️ Top](#tabla-de-contenidos)

**¿Qué hace?**
Integra cambios de una rama en otra, combinando el trabajo de diferentes líneas de desarrollo. Es uno de los comandos más críticos en Git para la colaboración en equipo.

**Funcionamiento interno:** [🔙](#8-git-merge---integrando-cambios)

```
Git merge puede operar de 3 formas diferentes:

1. FAST-FORWARD (merge "rápido"):
   main:    A---B
   feature:      C---D

   Resultado: main simplemente avanza al commit D
   main:    A---B---C---D

   → No crea merge commit
   → Solo mueve el puntero de la rama
   → Historia lineal limpia
   → Condición: main no ha avanzado desde que se creó feature

2. THREE-WAY MERGE (merge de 3 vías):
   main:    A---B---C
                \
   feature:      D---E

   Git usa 3 commits:
   - Ancestro común (B)
   - Último commit de main (C)
   - Último commit de feature (E)

   Resultado: Se crea nuevo merge commit (M)
   main:    A---B---C---M
                \     /
   feature:      D---E

   → Crea merge commit con 2 líneas de commits
   → Preserva historia completa
   → Historia no lineal (ramificada)

3. CONFLICTO:
   Cuando ambas ramas modifican las mismas líneas:
   → Git no puede decidir automáticamente
   → Marca conflictos en archivos
   → Requiere resolución manual
   → Crea merge commit tras resolver

Internamente:
1. git merge-base main feature  → Encuentra ancestro común
2. git diff-tree ancestro main  → Cambios en main
3. git diff-tree ancestro feature → Cambios en feature
4. Aplica ambos sets de cambios
5. Si no hay conflictos → merge automático
6. Si hay conflictos → pausa y marca conflictos
```

**Todas las opciones importantes:** [🔙](#8-git-merge---integrando-cambios)

```bash
# ============================================
# OPCIONES DE ESTRATEGIA DE MERGE
# ============================================

# 1. Merge básico (comportamiento por defecto)
git merge feature-x
# → Fast-forward si es posible
# → Three-way merge si no

# 2. Forzar merge commit (sin fast-forward)
git merge --no-ff feature-x
# → SIEMPRE crea merge commit
# → Preserva historia de rama
# → Útil para features importantes
# → Mantiene visible qué commits pertenecían a la feature

# 3. Solo fast-forward (falla si no es posible)
git merge --ff-only feature-x
# → Solo avanza puntero
# → Falla si requiere merge commit
# → Útil para mantener historia lineal estricta
# → Común en workflows con rebase

# 4. Squash merge (aplasta todos los commits en uno)
git merge --squash feature-x
# → Aplica TODOS los cambios de feature-x
# → NO crea merge commit automáticamente
# → Debes hacer commit manual después
# → Resultado: 1 solo commit en main
# → Pierde historia individual de commits de feature
git commit -m "Add complete feature X"

# 5. Merge con edición de mensaje
git merge --edit feature-x
# → Abre editor para personalizar mensaje de merge commit
# → Por defecto usa mensaje automático

git merge --no-edit feature-x
# → Usa mensaje automático sin preguntar

# ============================================
# ESTRATEGIAS DE MERGE
# ============================================

# 6. Estrategia "ours" (en conflictos, prefiere nuestra versión)
git merge -X ours feature-x
# → En conflictos automáticos, usa versión de rama actual
# → CUIDADO: Puede silenciar cambios importantes
# → Útil cuando estás seguro de que tu versión es correcta

# 7. Estrategia "theirs" (en conflictos, prefiere su versión)
git merge -X theirs feature-x
# → En conflictos automáticos, usa versión de rama entrante
# → CUIDADO: Puede sobrescribir tu trabajo
# → Útil cuando aceptas completamente los cambios externos

# 8. Estrategia "recursive" (por defecto, más opciones)
git merge -s recursive -X patience feature-x
# patience: Algoritmo más cuidadoso (más lento, menos conflictos)

git merge -s recursive -X diff-algorithm=histogram feature-x
# Algoritmos: myers (default), minimal, patience, histogram

git merge -s recursive -X ignore-space-change feature-x
# Ignora cambios solo de espacios en blanco

git merge -s recursive -X ignore-all-space feature-x
# Ignora todos los espacios al comparar

git merge -s recursive -X ignore-space-at-eol feature-x
# Ignora espacios al final de línea

git merge -s recursive -X renormalize feature-x
# Re-normaliza archivos (útil con cambios de line-endings)

# 9. Estrategia "octopus" (merge múltiples ramas)
git merge branch1 branch2 branch3
# → Merge de 3+ ramas simultáneamente
# → Falla si hay conflictos (no soporta resolución manual)
# → Útil para integrar múltiples features simples

# 10. Estrategia "ours" (NO confundir con -X ours)
git merge -s ours old-feature
# → IGNORA completamente cambios de old-feature
# → Solo registra merge en historia
# → Útil para marcar rama como mergeada sin aplicar cambios
# → Diferente de -X ours (que sí intenta merge)

# 11. Estrategia "subtree"
git merge -s subtree -X subtree=libs/ external-lib
# → Merge de repositorio externo como subdirectorio
# → Útil para dependencias embebidas

# ============================================
# OPCIONES DE CONTROL Y VERIFICACIÓN
# ============================================

# 12. Verificar merge sin hacerlo
git merge --no-commit --no-ff feature-x
# → Prepara merge pero NO commitea
# → Permite revisar antes de finalizar
# → Útil para verificar resultado
# Luego:
git commit  # Para finalizar
# o
git merge --abort  # Para cancelar

# 13. Ver qué se va a mergear
git log HEAD..feature-x
# → Commits que entrarán en merge
git diff HEAD...feature-x
# → Cambios desde punto de divergencia

# 14. Merge con log de commits incluidos
git merge --log feature-x
# → Incluye resumen de commits en mensaje
git merge --no-log feature-x
# → No incluye resumen

# 15. Merge con firma GPG
git merge -S feature-x
# → Firma merge commit con GPG
# → Para verificación de autoría

# 16. Merge verboso
git merge -v feature-x
# → Muestra información detallada del proceso

git merge -q feature-x
# → Modo silencioso (solo errores)

# ============================================
# MANEJO DE MERGE EN PROGRESO
# ============================================

# 17. Abortar merge
git merge --abort
# → Cancela merge en curso
# → Restaura estado pre-merge
# → Solo funciona si merge no está completo

# 18. Continuar merge tras resolver conflictos
git add archivo-resuelto.txt
git commit
# → Git detecta merge en progreso
# → Usa mensaje de merge automático

# 19. Estado de merge en progreso
git status
# → Muestra archivos en conflicto
# → Indica que merge está pendiente

ls -la .git/
# → .git/MERGE_HEAD existe durante merge
# → Contiene SHA del commit siendo mergeado
```

**Resolución de conflictos - Guía completa:** [🔙](#8-git-merge---integrando-cambios)

```bash
# ============================================
# IDENTIFICAR CONFLICTOS
# ============================================

# Ver archivos en conflicto
git status
# Muestra:
# - Unmerged paths (archivos con conflictos)
# - Changes to be committed (archivos auto-mergeados)

# Listar solo archivos con conflictos
git diff --name-only --diff-filter=U

# Ver conflictos con contexto
git diff

# Ver estadísticas de conflictos
git diff --stat

# ============================================
# ANATOMÍA DE UN CONFLICTO
# ============================================

# Git marca conflictos en el archivo:
<<<<<<< HEAD (rama actual)
código de la rama actual (main)
este código estaba aquí antes
=======
código de la rama entrante (feature-x)
este código viene del merge
>>>>>>> feature-x (rama que se está mergeando)

# Significado de marcadores:
# <<<<<<< HEAD        → Inicio de tu versión
# =======             → Separador
# >>>>>>> feature-x   → Fin de su versión

# ============================================
# ESTRATEGIAS DE RESOLUCIÓN
# ============================================

# 1. Resolución manual (más común)
# - Abre archivo en editor
# - Elimina marcadores <<<, ===, >>>
# - Edita código para combinar o elegir
# - Guarda archivo
git add archivo.txt
git commit

# 2. Aceptar versión completa (sin editar)
git checkout --ours archivo.txt    # Usar nuestra versión
git add archivo.txt

git checkout --theirs archivo.txt  # Usar su versión
git add archivo.txt

# 3. Ver diferencias durante conflicto
git diff --ours      # Diferencias con nuestra versión
git diff --theirs    # Diferencias con su versión
git diff --base      # Diferencias con ancestro común

# 4. Herramienta visual de merge
git mergetool
# → Abre herramienta configurada (vimdiff, meld, kdiff3, etc.)
# → Muestra 3 paneles: base, ours, theirs
# → Facilita resolución visual

# Configurar herramienta:
git config --global merge.tool meld
git config --global mergetool.prompt false

# 5. Ver contenido de versiones específicas
git show :1:archivo.txt  # Versión ancestro común (base)
git show :2:archivo.txt  # Versión nuestra (ours/HEAD)
git show :3:archivo.txt  # Versión suya (theirs)

# Guardar para comparar:
git show :2:archivo.txt > archivo-ours.txt
git show :3:archivo.txt > archivo-theirs.txt
# Comparar con herramienta externa

# ============================================
# CASOS ESPECIALES
# ============================================

# Conflictos en archivos binarios
git checkout --ours archivo.bin
# o
git checkout --theirs archivo.bin
# (No se pueden resolver manualmente línea a línea)

# Conflictos por archivo eliminado en una rama
# Git pregunta si mantener o eliminar:
git rm archivo.txt      # Confirma eliminación
# o
git add archivo.txt     # Mantiene archivo

# Conflictos por archivo renombrado
# Git puede detectar rename automáticamente
# Si no, resolver manualmente y hacer add

# ============================================
# ABORTAR Y REINTENTAR
# ============================================

# Abortar merge completo
git merge --abort
# → Vuelve a estado pre-merge
# → Útil si te equivocaste en resolución

# Reiniciar resolución de un archivo
git checkout -m archivo.txt
# → Restaura marcadores de conflicto
# → Permite resolver de nuevo

# Ver merge que causó conflicto
cat .git/MERGE_HEAD
# → SHA del commit siendo mergeado

git log -1 MERGE_HEAD
# → Detalles del commit en conflicto

# ============================================
# POST-RESOLUCIÓN
# ============================================

# Verificar que no quedan conflictos
git diff --check
# → Detecta marcadores de conflicto olvidados

# Verificar que todo compila/funciona
npm test  # o tu sistema de tests
git commit

# Limpiar archivos .orig (backup de mergetool)
git clean -f
# o configurar para no crearlos:
git config --global mergetool.keepBackup false
```

**Casos de uso del mundo real:**

```bash
# ============================================
# CASO 1: Feature simple lista para producción
# ============================================
git checkout main
git pull origin main
git merge --no-ff feature-login
git push origin main
# → Usa --no-ff para mantener visible la feature en historia

# ============================================
# CASO 2: Sincronizar feature con main
# ============================================
# Estás en feature-x, main avanzó, quieres últimos cambios
git checkout feature-x
git merge main
# → Trae cambios de main a tu feature
# → Resuelve conflictos ahora (no luego en main)
# → Testea todo funciona junto

# ============================================
# CASO 3: Multiple commits WIP, quieres 1 solo
# ============================================
git checkout main
git merge --squash feature-x
# Archivo .git/SQUASH_MSG tiene todos los mensajes
git commit -m "Add user authentication system

- Login form
- Password validation
- Session management
- Remember me functionality"
# → Main tiene 1 commit limpio
# → Historia de desarrollo (commits WIP) se pierde

# ============================================
# CASO 4: Hotfix urgente en producción
# ============================================
git checkout main
git checkout -b hotfix-security
# ... fixes ...
git commit -m "Fix: Security vulnerability CVE-2024-1234"
git checkout main
git merge --ff-only hotfix-security
# → --ff-only asegura merge limpio
# → Si falla, main se movió y hay que investigar
git push origin main
git branch -d hotfix-security

# ============================================
# CASO 5: Merge de múltiples features independientes
# ============================================
git checkout develop
git merge feature-a feature-b feature-c
# → Octopus merge
# → Solo si no hay conflictos
# → Historia muestra merge simultáneo

# ============================================
# CASO 6: Merge con revisión antes de commitear
# ============================================
git merge --no-commit --no-ff feature-x
# → Prepara merge sin commitear
git diff --staged
# → Revisa todos los cambios
npm test
# → Verifica que funciona
git commit
# o si algo falla:
git merge --abort

# ============================================
# CASO 7: Rama obsoleta, solo quieres marcarla como mergeada
# ============================================
git merge -s ours old-experiment
# → No aplica ningún cambio de old-experiment
# → Pero Git la marca como mergeada
# → Útil para limpiar ramas sin afectar código

# ============================================
# CASO 8: Merge de release branch
# ============================================
# Merge a main (producción)
git checkout main
git merge --no-ff --log release-1.5.0
# --log incluye lista de commits en mensaje

# Merge de vuelta a develop
git checkout develop
git merge --no-ff release-1.5.0

# ============================================
# CASO 9: Resolver conflicto prefiriendo una versión
# ============================================
git merge feature-x
# ... conflicto ...
git checkout --ours .      # Todas las versiones nuestras
# o
git checkout --theirs .    # Todas las versiones de ellos
git add .
git commit

# Más selectivo (solo ciertos archivos):
git checkout --ours src/
git checkout --theirs config/
git add .
git commit

# ============================================
# CASO 10: Merge con conflictos, quieres ver qué cambió
# ============================================
git merge feature-x
# ... conflictos ...

# Ver historial de cambios en archivo conflictivo
git log --oneline --all -- archivo-conflicto.txt

# Ver qué cambió en cada rama
git log main..feature-x -- archivo-conflicto.txt
git show feature-x:archivo-conflicto.txt
git show main:archivo-conflicto.txt

# Resolver informadamente
# ... edita ...
git add archivo-conflicto.txt
git commit
```

**Troubleshooting y problemas comunes:**

```bash
# ============================================
# PROBLEMA 1: "Already up to date"
# ============================================
git merge feature-x
# Already up to date.

Causa: feature-x no tiene commits nuevos vs main
Solución:
- Verificar que estás en rama correcta
- Verificar que feature-x tiene commits:
  git log main..feature-x

# ============================================
# PROBLEMA 2: "fatal: refusing to merge unrelated histories"
# ============================================
Causa: Ramas sin ancestro común (repos separados)
Solución:
git merge --allow-unrelated-histories other-branch
# ⚠️ CUIDADO: Puede crear merge complejo

# ============================================
# PROBLEMA 3: Merge incompleto, .git/MERGE_HEAD existe
# ============================================
git status
# On branch main
# You have unmerged paths.

Causa: Merge con conflictos sin resolver
Solución:
1. Resolver conflictos:
   git status  # Ver qué falta
   # ... resolver ...
   git add .
   git commit
2. O abortar:
   git merge --abort

# ============================================
# PROBLEMA 4: Merge commit no deseado
# ============================================
# Ya hiciste merge y no querías merge commit
git reset --hard HEAD~1  # Deshace último commit
git merge --ff-only feature-x  # Intenta fast-forward

# ============================================
# PROBLEMA 5: Conflictos masivos, difícil resolver
# ============================================
Solución 1: Abortar y usar rebase
git merge --abort
git rebase main  # Resuelve conflicto por conflicto

Solución 2: Estrategia más agresiva
git merge -X theirs feature-x
# LUEGO revisa cambios críticos manualmente

Solución 3: Resolver en herramienta visual
git mergetool

# ============================================
# PROBLEMA 6: Merge eliminó archivo que debería existir
# ============================================
# Git puede auto-mergear eliminación incorrectamente
git show HEAD:archivo-perdido.txt > archivo-perdido.txt
git add archivo-perdido.txt
git commit --amend  # Corrige merge commit

# ============================================
# PROBLEMA 7: Merge rompió funcionalidad
# ============================================
# Opción 1: Revert del merge
git revert -m 1 HEAD
# -m 1 indica mantener lado 1 (main) del merge

# Opción 2: Reset (si no pusheaste)
git reset --hard HEAD~1

# ============================================
# PROBLEMA 8: No puedes hacer merge (archivos sucios)
# ============================================
error: Your local changes would be overwritten by merge.

Solución 1: Commitear cambios
git add .
git commit -m "WIP"
git merge feature-x

Solución 2: Stash
git stash
git merge feature-x
git stash pop

Solución 3: Descartar cambios
git reset --hard  # ⚠️ PIERDE CAMBIOS
git merge feature-x
```

**Mejores prácticas y patrones:**

```bash
# ============================================
# ✅ BUENAS PRÁCTICAS
# ============================================

# 1. Siempre actualiza antes de merge
git checkout main
git pull origin main
git merge feature-x

# 2. Usa --no-ff para features importantes
git merge --no-ff feature-login
# → Historia clara, fácil revertir feature completa

# 3. Resuelve conflictos en feature branch, no en main
git checkout feature-x
git merge main
# ... resolver conflictos ...
git checkout main
git merge feature-x  # Ahora sin conflictos

# 4. Testea tras resolver conflictos
git merge feature-x
# ... resolver ...
npm test
git commit

# 5. Usa mensajes de merge descriptivos
git merge --no-ff --edit feature-auth
# Edita para incluir:
# - Qué hace la feature
# - Issues relacionados (#123)
# - Reviewers

# 6. Squash para limpiar historia
git merge --squash feature-experiment
# → 47 commits de prueba → 1 commit limpio

# 7. Verifica antes de push
git log --oneline --graph -10
git diff origin/main
git push origin main

# 8. Usa merge commits para puntos importantes
git merge --no-ff release-2.0
# → Marca claramente releases en historia

# ============================================
# ✗ MALAS PRÁCTICAS
# ============================================

# 1. Mergear sin testear
git merge feature-x && git push  # ❌
# Puede romper main

# 2. Usar -X ours/theirs sin revisar
git merge -X theirs external-branch  # ❌
# Puede sobrescribir trabajo importante

# 3. Mergear directo a main sin revisión
# En proyectos serios, usa Pull Requests

# 4. Ignorar conflictos "pequeños"
# Todo conflicto requiere atención

# 5. No limpiar branches tras merge
git merge feature-x
git push
# Luego:
git branch -d feature-x  # ✅ Limpia local
git push origin --delete feature-x  # ✅ Limpia remoto

# 6. Merge de ramas públicas con rebase
# Causa problemas a colaboradores

# ============================================
# WORKFLOWS COMUNES
# ============================================

# GitHub Flow (simple)
1. Crea feature branch desde main
2. Desarrolla y commitea
3. Push y crea Pull Request
4. Revisión de código
5. Merge (con --no-ff) a main
6. Delete branch

# Git Flow (complejo)
- main: Producción
- develop: Integración
- feature/*: Nuevas features
- release/*: Preparar release
- hotfix/*: Fixes urgentes

# Feature → develop: --no-ff
# develop → main: --no-ff (con tag)
# hotfix → main y develop: --no-ff
```

**Comparación: merge vs rebase:**

```bash
# ============================================
# CUÁNDO USAR MERGE
# ============================================
✅ Integrar features completas a main
✅ Merges de release branches
✅ Colaboración en ramas públicas
✅ Preservar historia exacta de desarrollo
✅ Cuando múltiples devs trabajan en misma rama

Ventajas:
- No reescribe historia
- Seguro para ramas compartidas
- Preserva contexto (cuándo se mergeó)
- Fácil revertir (git revert -m 1)

Desventajas:
- Historia puede volverse compleja
- Grafo con muchas ramas
- "Merge commits" pueden saturar log

# ============================================
# CUÁNDO USAR REBASE
# ============================================
✅ Actualizar feature branch con main
✅ Limpiar commits locales antes de merge
✅ Mantener historia lineal
✅ Trabajo personal en rama local

Ventajas:
- Historia lineal y limpia
- Fácil de leer git log
- No crea merge commits extra

Desventajas:
- Reescribe historia (cambia SHAs)
- Peligroso en ramas públicas
- Puede causar problemas a colaboradores

# ============================================
# ESTRATEGIA HÍBRIDA (RECOMENDADA)
# ============================================

# 1. Durante desarrollo: rebase
git checkout feature-x
git rebase main  # Mantiene feature actualizada y limpia

# 2. Para integrar: merge
git checkout main
git merge --no-ff feature-x  # Integra feature completa

Resultado:
- Historia limpia en features (rebase)
- Historia clara en main (merge commits marcan features)
- Lo mejor de ambos mundos
```

**Configuración recomendada:**

```bash
# Configurar merge sin fast-forward por defecto
git config --global merge.ff false

# Siempre mostrar diffstat tras merge
git config --global merge.stat true

# Configurar herramienta de merge
git config --global merge.tool meld
git config --global mergetool.prompt false
git config --global mergetool.keepBackup false

# Estilo de conflictos (diff3 muestra ancestro común)
git config --global merge.conflictstyle diff3

# Ejemplo de conflicto con diff3:
<<<<<<< HEAD
código actual
||||||| merged common ancestors
código ancestro común
=======
código entrante
>>>>>>> feature-x

# Configurar para squash automático en certain branches
# (en .git/config o ~/.gitconfig)
[branch "develop"]
    mergeoptions = --no-ff

# Verificar configuración
git config --list | grep merge
```

---


---

## Navegación

- [⬅️ Anterior: git checkout / git switch](07-git-checkout-switch.md)
- [🏠 Volver al Índice](../../GIT_COMANDOS_GUIA_PRACTICA.md)
- [➡️ Siguiente: git rebase](09-git-rebase.md)

