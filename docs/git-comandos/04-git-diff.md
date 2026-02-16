# 4. git diff - Comparando Cambios

[🏠 Volver al Índice](../../GIT_COMANDOS_GUIA_PRACTICA.md)

---

## ¿Qué hace?
Muestra diferencias entre working directory, staging, commits y ramas.

**Funcionamiento interno:** [🔙](#4-git-diff---comparando-cambios)

```
1. Lee contenido de dos fuentes
2. Ejecuta algoritmo de diff (Myers, patience, histogram)
3. Genera "hunks" (bloques de diferencias)
4. Formatea output
```

**Uso práctico:** [🔙](#4-git-diff---comparando-cambios)

```bash
# Diff de working (NO stageado)
git diff

# Diff de staging (lo que vas a commitear)
git diff --staged
# o: git diff --cached

# Diff completo (working vs último commit)
git diff HEAD

# Diff entre commits
git diff abc123 def456
git diff HEAD~5 HEAD

# Diff entre ramas
git diff main feature-x
git diff main...feature-x  # Desde punto de divergencia

# Diff de archivo específico
git diff archivo.txt
git diff HEAD~3 -- archivo.txt

# Diff con stats (resumen)
git diff --stat

# Diff solo nombres de archivos
git diff --name-only
git diff --name-status

# Diff por palabras (útil para textos)
git diff --word-diff

# Ignorar espacios
git diff -w

# Detectar líneas movidas
git diff --color-moved
```

**Mejores prácticas:** [🔙](#4-git-diff---comparando-cambios)

```bash
✓ Usa git diff antes de add
✓ Usa git diff --staged antes de commit
✓ Usa --word-diff para documentación
✓ Usa ... (tres puntos) para comparar ramas

✗ No ignores el diff antes de commitear
✗ No confundas git diff con git diff --staged
```

---


---

## Navegación

- [⬅️ Anterior: git status](03-git-status.md)
- [🏠 Volver al Índice](../../GIT_COMANDOS_GUIA_PRACTICA.md)
- [➡️ Siguiente: Referencias de Commits](04.1-referencias-commits.md)

