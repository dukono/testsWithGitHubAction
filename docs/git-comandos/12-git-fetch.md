# 12. git fetch - Descargando Cambios

[🏠 Volver al Índice](../../GIT_COMANDOS_GUIA_PRACTICA.md)

---

## 12. git fetch - Descargando Cambios
[⬆️ Top](#tabla-de-contenidos)

**¿Qué hace?**
Descarga objetos y refs del remoto SIN modificar working directory.

**Funcionamiento interno:**
```
1. Conecta con remoto
2. Compara refs locales vs remotas
3. Descarga objetos faltantes
4. Actualiza refs/remotes/origin/*
5. NO modifica ramas locales
6. NO modifica working directory
```

**Uso práctico:**

```bash
# Fetch básico
git fetch
git fetch origin

# Fetch de rama específica
git fetch origin main

# Fetch de todos los remotos
git fetch --all

# Fetch con prune (limpia refs obsoletas)
git fetch --prune

# Fetch de PR (GitHub)
git fetch origin pull/123/head:pr-123

# Ver resultado
git log HEAD..origin/main --oneline
git diff origin/main
```

**Fetch vs Pull:**

```bash
# FETCH: Solo descarga
git fetch origin main
→ origin/main actualizado
→ main local SIN cambios
→ Puedes revisar antes de integrar

# PULL: Fetch + Merge
git pull origin main
→ Descarga Y mergea automáticamente
→ Más rápido pero menos control
```

**Mejores prácticas:**

```bash
✓ Usa fetch antes de pull (revisa cambios)
✓ Usa --prune regularmente
✓ Fetch frecuentemente
✓ Revisa con git log tras fetch

✗ No confundas fetch con pull
✗ No asumas que fetch cambia working
✗ No olvides mergear después de fetch
```

---


---

## Navegación

- [⬅️ Anterior: git remote](11-git-remote.md)
- [🏠 Volver al Índice](../../GIT_COMANDOS_GUIA_PRACTICA.md)
- [➡️ Siguiente: git pull](13-git-pull.md)

