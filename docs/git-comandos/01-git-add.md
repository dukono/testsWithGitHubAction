# 1. git add - Preparando Cambios

[🏠 Volver al Índice](../../GIT_COMANDOS_GUIA_PRACTICA.md)

---

## ¿Qué hace?
Prepara cambios del working directory para el próximo commit, moviéndolos al staging area (index).

**Funcionamiento interno:** [🔙](#1-git-add---preparando-cambios)

```
Internamente hace:
1. git hash-object -w file.txt
   → Calcula SHA-1 del contenido
   → Comprime con zlib
   → Guarda blob en .git/objects/

2. git update-index --add file.txt
   → Actualiza .git/index con:
     - Ruta del archivo
     - Hash del blob
     - Permisos (100644, 100755, etc.)
     - Timestamp

Resultado:
- Blob creado en objects/
- Index (.git/index) actualizado
- Working directory NO cambia
- Repository NO cambia (aún no hay commit)
```

**Uso práctico y opciones:** [🔙](#1-git-add---preparando-cambios)

```bash
# 1. Añadir archivo específico
git add archivo.txt
# → Stagea solo archivo.txt

# 2. Añadir todos los archivos modificados y nuevos
git add .
# → Stagea todo desde directorio actual
# → Incluye subdirectorios
# → Respeta .gitignore

# 3. Añadir todos los archivos del repositorio
git add -A
# o: git add --all
# → Stagea TODO: nuevos, modificados, eliminados
# → Desde cualquier directorio

# 4. Añadir solo archivos rastreados (ignora nuevos)
git add -u
# o: git add --update
# → Solo archivos ya en Git
# → NO añade archivos nuevos
# → Útil para "actualizar solo lo existente"

# 5. Añadir interactivamente (PODER REAL)
git add -i
# → Modo interactivo con menú
# → Puedes elegir qué hacer con cada archivo

# 6. Añadir por parches (SUPER ÚTIL)
git add -p archivo.txt
# o: git add --patch
# → Te muestra cada "hunk" de cambios
# → Preguntas: Stage this hunk? [y,n,q,a,d,s,e,?]
# → Puedes stagear solo PARTE de un archivo
```

**Caso de uso real: Commits atómicos con -p:** [🔙](#1-git-add---preparando-cambios)

```bash
Escenario: Modificaste un archivo con 2 features diferentes

# archivo.py tiene:
# - Cambio A: Nueva función calculate()
# - Cambio B: Fix bug en validate()

# Quieres 2 commits separados:

# Paso 1: Stagea solo cambios de calculate()
git add -p archivo.py
# → Ves el hunk con calculate()
# → Presionas 'y' (yes)
# → Ves el hunk con validate()
# → Presionas 'n' (no)

git commit -m "feat: Add calculate function"

# Paso 2: Stagea el resto
git add archivo.py
git commit -m "fix: Fix validation bug"

Resultado: 2 commits atómicos, historia más clara
```

**Opciones avanzadas de add -p:** [🔙](#1-git-add---preparando-cambios)

```
Durante git add -p, opciones disponibles:

y - Stage this hunk (sí, añadir este cambio)
n - Do not stage (no, saltar)
q - Quit (salir, no procesar más)
a - Stage this and all remaining hunks (todos los siguientes)
d - Do not stage this or any remaining (ninguno de los siguientes)
s - Split into smaller hunks (dividir en partes más pequeñas)
e - Manually edit hunk (editar manualmente)
? - Help (ayuda)

Opción 's' (split) es PODEROSA:
→ Si un hunk tiene múltiples cambios cercanos
→ Puedes intentar dividirlo en hunks más pequeños
→ Para control más granular

Opción 'e' (edit) es para EXPERTOS:
→ Abre editor con el diff
→ Puedes editar líneas manualmente
→ Útil cuando 's' no divide suficiente
```

**Patrones de uso comunes:** [🔙](#1-git-add---preparando-cambios)

```bash
# Patrón 1: Añadir por tipo de archivo
git add *.py          # Solo archivos Python
git add src/          # Todo en directorio src/
git add "*.txt"       # Todos los .txt (comillas para expansión)

# Patrón 2: Añadir excepto algunos
git add .
git reset HEAD archivo-no-deseado.txt
# → Añade todo, luego quita uno

# Patrón 3: Añadir forzando (ignorar .gitignore)
git add -f archivo-ignorado.log
# → Fuerza añadir aunque esté en .gitignore
# → Úsalo con CUIDADO

# Patrón 4: Dry run (ver qué se añadiría)
git add -n .
# o: git add --dry-run .
# → Muestra qué se añadiría sin hacerlo

# Patrón 5: Añadir con verbose
git add -v archivo.txt
# → Muestra qué archivos se añaden
```

**Ver qué está stageado:** [🔙](#1-git-add---preparando-cambios)

```bash
# Ver estado
git status
# → Muestra archivos stageados y no stageados

# Ver diferencias stageadas
git diff --staged
# o: git diff --cached
# → Muestra QUÉ cambios están en staging

# Ver diferencias NO stageadas
git diff
# → Muestra cambios en working directory
# → Que NO están en staging
```

**Mejores prácticas:** [🔙](#1-git-add---preparando-cambios)

```bash
✓ Usa git add -p para commits granulares
✓ Revisa con git diff --staged antes de commit
✓ No uses git add . ciegamente, revisa qué añades
✓ Usa .gitignore para archivos que nunca deben añadirse
✓ Considera git add -u cuando solo actualizas existentes

✗ Evita git add * (puede añadir archivos no deseados)
✗ No uses git add -f a menos que sea absolutamente necesario
✗ No stagees archivos generados (builds, logs, node_modules)
```
# → Ves hunk con feature B: presionas 'n'
git commit -m "feat: Add feature A"

git add archivo.py
git commit -m "feat: Add feature B"
```

**Mejores prácticas:**

✓ Usa git add -p para commits granulares
✓ Revisa con git diff --staged antes de commit
✓ Usa .gitignore para archivos que nunca deben añadirse

✗ Evita git add * (puede añadir no deseados)
✗ No stagees archivos generados (builds, node_modules)
```

---


---

## Navegación

- [🏠 Volver al Índice](../../GIT_COMANDOS_GUIA_PRACTICA.md)
- [➡️ Siguiente: git commit](02-git-commit.md)

