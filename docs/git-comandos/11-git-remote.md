# 11. git remote - Gestionando Repositorios Remotos

[🏠 Volver al Índice](../../GIT_COMANDOS_GUIA_PRACTICA.md)

---

## 11. git remote - Gestionando Repositorios Remotos
[⬆️ Top](#tabla-de-contenidos)

**¿Qué hace?**
Gestiona referencias a repositorios remotos.

**Funcionamiento interno:**
```
Remotos se guardan en .git/config:
[remote "origin"]
    url = https://github.com/user/repo.git
    fetch = +refs/heads/*:refs/remotes/origin/*
```

**Uso práctico:**

```bash
# Listar remotos
git remote
git remote -v

# Añadir remoto
git remote add upstream https://github.com/original/repo.git

# Ver detalles
git remote show origin

# Cambiar URL
git remote set-url origin https://nuevo-url.git
git remote set-url origin git@github.com:user/repo.git

# Renombrar
git remote rename origin nuevo-nombre

# Eliminar
git remote remove upstream

# Limpiar refs obsoletas
git remote prune origin
git remote prune origin --dry-run
```

**Fork workflow:**

```bash
git clone https://github.com/tu-fork/proyecto.git
cd proyecto
git remote add upstream https://github.com/original/proyecto.git
git remote -v
# origin    tu-fork (fetch/push)
# upstream  original (fetch/push)

# Workflow:
git fetch upstream
git merge upstream/main
git push origin main
```

**Mejores prácticas:**

```bash
✓ Usa nombres descriptivos (origin, upstream, backup)
✓ Usa SSH para repos privados
✓ Configura upstream para forks
✓ Limpia con prune regularmente

✗ No pongas credenciales en URL
✗ No uses nombres confusos
✗ No borres origin sin reemplazarlo
```

---


---

## Navegación

- [⬅️ Anterior: git clone](10-git-clone.md)
- [🏠 Volver al Índice](../../GIT_COMANDOS_GUIA_PRACTICA.md)
- [➡️ Siguiente: git fetch](12-git-fetch.md)

