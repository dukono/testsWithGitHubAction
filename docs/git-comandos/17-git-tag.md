# 17. git tag - Marcando Versiones

[🏠 Volver al Índice](../../GIT_COMANDOS_GUIA_PRACTICA.md)

---

## 17. git tag - Marcando Versiones
[⬆️ Top](#tabla-de-contenidos)

**¿Qué hace?**
Crea referencias inmutables a commits (usualmente para versiones).

**Funcionamiento interno:**
```
Lightweight tag: Solo referencia
Annotated tag: Objeto completo con mensaje, autor, fecha
```

**Uso práctico:**

```bash
# Crear lightweight tag
git tag v1.0.0

# Crear annotated tag (RECOMENDADO)
git tag -a v1.0.0 -m "Release 1.0.0"

# ============================================
# LISTAR TAGS
# ============================================

# Listar todos los tags
git tag
# → Orden alfabético por defecto

# Listar con patrón
git tag -l "v1.*"
git tag -l "v*-beta*"
git tag --list "release-*"

# Listar tags que contienen un commit
git tag --contains abc123
git tag --contains HEAD

# Listar tags que NO contienen un commit
git tag --no-contains abc123

# Listar tags merged/no-merged
git tag --merged main
git tag --no-merged main

# Listar tags con anotaciones
git tag -n
git tag -n5  # Muestra hasta 5 líneas del mensaje

# Ordenar tags
git tag --sort=-creatordate      # Por fecha (más recientes primero)
git tag --sort=version:refname   # Por versión semántica
git tag --sort=refname           # Alfabético
git tag --sort=-taggerdate       # Por fecha del tagger


# ============================================
# VER DETALLES DE TAGS
# ============================================

# Ver información completa
git show v1.0.0
# → Muestra tag object + commit + diff

# Ver solo información del tag
git show v1.0.0 --no-patch

# Ver múltiples tags
git show v1.0.0 v2.0.0

# Ver commit al que apunta
git rev-list -n 1 v1.0.0

# Ver diferencia entre tags
git diff v1.0.0..v2.0.0
git log v1.0.0..v2.0.0 --oneline


# ============================================
# FORMATO PERSONALIZADO (--format)
# ============================================

> 📖 **NOTA:** Para una referencia completa de todos los placeholders disponibles,
> formatos avanzados, condicionales y ejemplos con otros comandos (log, branch,
> for-each-ref, show-ref, etc.), consulta la **[Sección 22: Referencias y Placeholders de Formato](#22-referencias-y-placeholders-de-formato)**.

# git tag también acepta placeholders como git branch
# Ver sección 22 para lista completa

# Lista simple con hash
git tag --format="%(refname:short) %(objectname:short)"
# Salida:
# v1.0.0 a1b2c3d
# v1.1.0 e4f5g6h
# v2.0.0 i7j8k9l

# Con fecha y autor
git tag --format="%(refname:short) | %(creatordate:short) | %(taggername)"
# Salida:
# v1.0.0 | 2024-01-15 | Juan Pérez
# v1.1.0 | 2024-02-20 | María García

# Con mensaje del tag
git tag --format="%(refname:short) - %(contents:subject)"
# Salida:
# v1.0.0 - Initial release
# v1.1.0 - Bug fixes and improvements

# Con información completa
git tag --format="Tag: %(refname:short)
Commit: %(objectname:short)
Fecha: %(creatordate:short)
Autor: %(taggername) <%(taggeremail)>
Mensaje: %(contents:subject)
---"

# Con colores
git tag --format="%(color:green)%(refname:short)%(color:reset) (%(creatordate:relative))"

# Ordenado por fecha con formato
git tag --sort=-creatordate --format="%(creatordate:short) %(refname:short) - %(contents:subject)"

# Export a CSV
git tag --format="%(refname:short),%(objectname:short),%(taggername),%(creatordate:short),%(contents:subject)" > tags.csv


# PLACEHOLDERS ESPECÍFICOS PARA TAGS:
%(refname)              # refs/tags/v1.0.0
%(refname:short)        # v1.0.0
%(objectname)           # Hash del tag object
%(objectname:short)     # Hash abreviado
%(objecttype)           # "tag" o "commit"
%(taggername)           # Nombre del tagger (solo annotated)
%(taggeremail)          # Email del tagger
%(taggerdate)           # Fecha del tag
%(taggerdate:short)     # 2024-02-13
%(taggerdate:relative)  # "2 days ago"
%(creatordate)          # Fecha de creación (funciona con lightweight)
%(contents)             # Mensaje completo del tag
%(contents:subject)     # Primera línea del mensaje
%(contents:body)        # Cuerpo del mensaje (sin subject)


# ============================================
# CREAR Y GESTIONAR TAGS
# ============================================

# Crear lightweight tag (simple puntero)
git tag v1.0.0
# → Solo referencia al commit, sin metadata

# Crear annotated tag (RECOMENDADO para releases)
git tag -a v1.0.0 -m "Release 1.0.0"
# → Objeto completo: mensaje, autor, fecha, firma opcional

# Tag con mensaje multilínea
git tag -a v1.0.0 -m "Release 1.0.0

Features:
- User authentication
- Payment integration
- Dashboard redesign"

# Tag en commit específico
git tag -a v1.0.0 abc123 -m "Release 1.0.0"

# Tag con editor
git tag -a v1.0.0
# → Abre editor para escribir mensaje extenso

# Tag con firma GPG
git tag -s v1.0.0 -m "Signed release 1.0.0"
# → Crea tag firmado, verificable

# Verificar firma de tag
git tag -v v1.0.0
git show --show-signature v1.0.0

# Tag forzado (reemplazar existente)
git tag -f v1.0.0
git tag -af v1.0.0 -m "Release 1.0.0 (updated)"


# ============================================
# ELIMINAR TAGS
# ============================================

# Eliminar tag local
git tag -d v1.0.0

# Eliminar múltiples tags locales
git tag -d v1.0.0 v1.1.0 v2.0.0

# Eliminar tag remoto
git push origin --delete v1.0.0
# o (sintaxis vieja):
git push origin :refs/tags/v1.0.0

# Eliminar todos los tags locales (cuidado)
git tag -l | xargs git tag -d


# ============================================
# PUSH DE TAGS
# ============================================

# Push de un tag específico
git push origin v1.0.0

# Push de todos los tags
git push --tags
# o:
git push origin --tags

# Push de tag y commit juntos
git push origin main --follow-tags
# → Pushea commit + tags anotados alcanzables

# Configurar push automático de tags
git config --global push.followTags true
# → Pushea tags automáticamente con commits


# ============================================
# CHECKOUT Y RAMAS DESDE TAGS
# ============================================

# Checkout de tag (detached HEAD)
git checkout v1.0.0
# → Estás en estado "detached HEAD"
# → Útil para revisar código de release

# Crear rama desde tag
git checkout -b hotfix-1.0.1 v1.0.0
# → Crea rama apuntando al commit del tag
# → Útil para hotfixes en versiones antiguas

# Ver en qué ramas está un tag
git branch --contains v1.0.0
git branch -a --contains v1.0.0  # Incluye remotas
```

**Semantic Versioning:**

```bash
v<MAJOR>.<MINOR>.<PATCH>

Ejemplos:
v1.0.0           # Release estable
v1.0.0-alpha.1   # Pre-release
v1.0.0-beta.2    # Beta
v1.0.0-rc.1      # Release candidate

Incremento:
v1.2.3 → v2.0.0  # Breaking change (MAJOR)
v1.2.3 → v1.3.0  # New feature (MINOR)
v1.2.3 → v1.2.4  # Bug fix (PATCH)
```

**Mejores prácticas:**

```bash
✓ Usa annotated tags para releases (-a)
✓ Sigue semantic versioning
✓ Firma tags importantes con GPG (-s)
✓ Push tags explícitamente
✓ Tag desde main después de merge

✗ No muevas tags ya pusheados
✗ No uses lightweight tags para releases
✗ No olvides pushear tags
```

---


---

## Navegación

- [⬅️ Anterior: git stash](16-git-stash.md)
- [🏠 Volver al Índice](../../GIT_COMANDOS_GUIA_PRACTICA.md)
- [➡️ Siguiente: git revert](18-git-revert.md)

