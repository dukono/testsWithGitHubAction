# 10. git clone - Copiando Repositorios

[🏠 Volver al Índice](../../GIT_COMANDOS_GUIA_PRACTICA.md)

---

## 10. git clone - Copiando Repositorios

**¿Qué hace?**
Crea una copia local completa de un repositorio remoto.

**Funcionamiento interno:**
```
1. Crea directorio
2. git init
3. git remote add origin <url>
4. git fetch origin
5. git checkout <default-branch>
```

**Uso práctico:**

```bash
# Clone básico
git clone https://github.com/user/repo.git

# Clone con nombre personalizado
git clone https://github.com/user/repo.git mi-proyecto

# Clone shallow (solo último commit, rápido)
git clone --depth 1 https://github.com/user/repo.git

# Clone de rama específica
git clone -b develop https://github.com/user/repo.git

# Clone con submódulos
git clone --recursive https://github.com/user/repo.git

# Clone parcial (sin blobs)
git clone --filter=blob:none https://github.com/user/repo.git
```

**Protocolos:**

```bash
# HTTPS (recomendado, universal)
git clone https://github.com/user/repo.git

# SSH (más rápido, requiere key)
git clone git@github.com:user/repo.git

# Local
git clone /ruta/al/repo.git
```

**Mejores prácticas:**

```bash
✓ Usa HTTPS para proyectos públicos
✓ Usa SSH para proyectos privados
✓ Usa --depth 1 en CI/CD
✓ Usa --recursive para repos con submódulos

✗ No clones con --depth si necesitas historia
✗ No desactives SSL verification sin razón
```

---


---

## Navegación

- [⬅️ Anterior: git rebase](09-git-rebase.md)
- [🏠 Volver al Índice](../../GIT_COMANDOS_GUIA_PRACTICA.md)
- [➡️ Siguiente: git remote](11-git-remote.md)

