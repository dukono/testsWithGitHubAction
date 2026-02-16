# 20. git clean - Limpiando Archivos No Rastreados

[🏠 Volver al Índice](../../GIT_COMANDOS_GUIA_PRACTICA.md)

---

## 20. git clean - Limpiando Archivos No Rastreados
[⬆️ Top](#tabla-de-contenidos)

**¿Qué hace?**
Elimina archivos untracked del working directory.

**⚠️ PELIGRO: Eliminación NO es reversible**

**Funcionamiento interno:**
```
1. Escanea working directory
2. Identifica archivos untracked
3. Los elimina del filesystem
```

**Uso práctico:**

```bash
# ⚠️ SIEMPRE DRY-RUN PRIMERO
git clean -n
git clean --dry-run

# Eliminar archivos
git clean -f

# Eliminar archivos + directorios
git clean -fd

# Eliminar TODO (incluye .gitignore)
git clean -fxd

# Interactivo (RECOMENDADO)
git clean -i

# Con exclusiones
git clean -fxd -e "*.log"
git clean -fd -e node_modules
```

**Clean vs Reset:**

```bash
CLEAN: Elimina archivos untracked
→ NO en Git
→ NO recuperables

RESET: Descarta cambios tracked
→ En Git
→ Recuperables con reflog

COMBINACIÓN (reset completo):
git reset --hard HEAD  # Tracked
git clean -fxd         # Untracked
```

**Mejores prácticas:**

```bash
✓ SIEMPRE usa -n (dry-run) primero
✓ Usa -i (interactive) para selectivo
✓ Usa .gitignore para archivos ignorables
✓ Verifica con git status antes

✗ NUNCA uses git clean sin revisar
✗ No uses -x sin entender consecuencias
✗ No asumas que puedes recuperar
```

---


---

## Navegación

- [⬅️ Anterior: git cherry-pick](19-git-cherry-pick.md)
- [🏠 Volver al Índice](../../GIT_COMANDOS_GUIA_PRACTICA.md)
- [➡️ Siguiente: git rm y git mv](21-git-rm-mv.md)

