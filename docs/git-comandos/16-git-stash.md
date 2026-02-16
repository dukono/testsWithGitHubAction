# 16. git stash - Guardado Temporal

[🏠 Volver al Índice](../../GIT_COMANDOS_GUIA_PRACTICA.md)

---

## 16. git stash - Guardado Temporal
[⬆️ Top](#tabla-de-contenidos)

**¿Qué hace?**
Guarda trabajo en progreso temporalmente sin commitear.

**Funcionamiento interno:**
```
1. Crea commits especiales en refs/stash
2. Guarda working + staging
3. Limpia working directory
4. Como una pila (stack): LIFO
```

**Uso práctico:**

```bash
# Stash básico
git stash
git stash push -m "WIP: feature half done"

# Stash incluyendo untracked
git stash -u

# Stash interactivo
git stash -p

# Ver lista
git stash list

# Ver contenido
git stash show
git stash show -p

# Aplicar stash (mantiene en lista)
git stash apply
git stash apply stash@{2}

# Pop stash (aplica y elimina)
git stash pop

# Crear rama desde stash
git stash branch nueva-rama

# Eliminar stash
git stash drop
git stash drop stash@{1}

# Limpiar todos
git stash clear
```

**Casos de uso:**

```bash
# Cambio urgente en otra rama
git stash
git checkout main
git checkout -b hotfix
# ... arreglas ...
git checkout feature-x
git stash pop

# Pull con cambios locales
git stash
git pull
git stash pop
# O:
git pull --autostash
```

**Mejores prácticas:**

```bash
✓ Usa mensajes descriptivos con -m
✓ Limpia stashes viejos regularmente
✓ Usa stash -u si añadiste archivos nuevos
✓ Prefiere stash pop sobre apply

✗ No uses stash como sistema de backup
✗ No acumules decenas de stashes
✗ No stashees y olvides
```

---


---

## Navegación

- [⬅️ Anterior: git reset](15-git-reset.md)
- [🏠 Volver al Índice](../../GIT_COMANDOS_GUIA_PRACTICA.md)
- [➡️ Siguiente: git tag](17-git-tag.md)

