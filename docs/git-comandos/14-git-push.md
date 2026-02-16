# 14. git push - Subiendo Cambios

[🏠 Volver al Índice](../../GIT_COMANDOS_GUIA_PRACTICA.md)

---

## 14. git push - Subiendo Cambios
[⬆️ Top](#tabla-de-contenidos)

**¿Qué hace?**
Envía commits locales al repositorio remoto.

**Funcionamiento interno:**
```
1. Conecta con remoto
2. Compara refs
3. Verifica que sea fast-forward
4. Empaqueta objetos faltantes
5. Envía objetos
6. Actualiza refs remotas
```

**Uso práctico:**

```bash
# Push básico
git push
git push origin main

# Push con tracking
git push -u origin feature-x

# Push forzado (¡CUIDADO!)
git push --force  # PELIGROSO
git push --force-with-lease  # PREFERIBLE

# Push de tags
git push origin v1.0.0
git push --tags

# Eliminar rama remota
git push origin --delete feature-x

# Push dry-run
git push --dry-run
```

**⚠️ Force push:**

```bash
NUNCA fuerces push en ramas compartidas (main, develop)

Cuándo SÍ:
✓ Feature branch personal
✓ Después de rebase local
✓ Corregir commits antes de merge

Cuándo NO:
✗ main/develop/master
✗ Ramas de otros
✗ Ramas públicas

SIEMPRE usa --force-with-lease (no --force):
git push --force-with-lease
→ Solo fuerza si nadie más actualizó
```

**Mejores prácticas:**

```bash
✓ Commitea cambios atómicos, push frecuentemente
✓ Usa --force-with-lease en vez de --force
✓ Verifica con --dry-run antes de push importante
✓ Pull antes de push (evita rechazos)

✗ NO uses --force en ramas compartidas
✗ NO pushees credenciales, secrets, keys
✗ NO pushees archivos gigantes
✗ NO ignores errores de push
```

---


---

## Navegación

- [⬅️ Anterior: git pull](13-git-pull.md)
- [🏠 Volver al Índice](../../GIT_COMANDOS_GUIA_PRACTICA.md)
- [➡️ Siguiente: git reset](15-git-reset.md)

