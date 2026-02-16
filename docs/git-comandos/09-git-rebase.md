# 9. git rebase - Reescribiendo Historia

[🏠 Volver al Índice](../../GIT_COMANDOS_GUIA_PRACTICA.md)

---

## 9. git rebase - Reescribiendo Historia
[⬆️ Top](#tabla-de-contenidos)

**¿Qué hace?**
Reaplica commits de una rama encima de otra, reescribiendo historia.

**Funcionamiento interno:**
```
1. Identifica commits únicos
2. Guarda como patches temporales
3. Resetea rama a base
4. Aplica patches uno por uno (nuevos commits)
```

**Uso práctico:**

```bash
# Rebase básico
git checkout feature-x
git rebase main

# Rebase interactivo (SUPER PODEROSO)
git rebase -i main
# Opciones:
# pick   - Usar commit
# reword - Cambiar mensaje
# edit   - Pausar para modificar
# squash - Combinar con anterior (mantiene mensaje)
# fixup  - Combinar con anterior (descarta mensaje)
# drop   - Eliminar commit

# Squash últimos N commits
git rebase -i HEAD~3

# Continuar tras conflicto
git add archivo-resuelto
git rebase --continue

# Saltar commit
git rebase --skip

# Abortar
git rebase --abort
```

**Rebase vs Merge:**

```bash
MERGE:
✓ Historia completa
✓ No reescribe commits
✓ Seguro para ramas públicas
✗ Grafo complejo

REBASE:
✓ Historia lineal
✓ Fácil de entender
✗ Reescribe commits
✗ Peligroso para ramas públicas

# ¿Cuándo usar cada uno?
REBASE: Rama local/feature antes de merge
MERGE: Integrar features a main
```

**⚠️ Regla de oro:**

```bash
NUNCA rebasees commits ya pusheados a repositorio público

Correcto:
git rebase main          # OK, commits solo locales
git push origin feature-x

Incorrecto:
git push origin feature-x
git rebase main          # ¡ROMPE REPO DE OTROS!
git push --force
```

**Mejores prácticas:**

```bash
✓ Usa rebase para limpiar historia local
✓ Rebase feature sobre main antes de merge
✓ Usa --force-with-lease en vez de --force
✓ Nunca rebasees ramas públicas compartidas

✗ No rebasees main o develop
✗ No rebasees commits públicos
✗ No uses --force sin --force-with-lease
```

---


---

## Navegación

- [⬅️ Anterior: git merge](08-git-merge.md)
- [🏠 Volver al Índice](../../GIT_COMANDOS_GUIA_PRACTICA.md)
- [➡️ Siguiente: git clone](10-git-clone.md)

