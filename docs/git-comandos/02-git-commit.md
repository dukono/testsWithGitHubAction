# 2. git commit - Guardando la Historia

[🏠 Volver al Índice](../../GIT_COMANDOS_GUIA_PRACTICA.md)

---

## ¿Qué hace?
Crea un snapshot inmutable del proyecto con los cambios del staging area.

**Funcionamiento interno:** [🔙](#2-git-commit---guardando-la-historia)

```
1. Crea tree object del staging
2. Crea commit object con tree + parent + metadata
3. Actualiza referencia de rama
4. Actualiza reflog
```

**Uso práctico:** [🔙](#2-git-commit---guardando-la-historia)

```bash
# 1. Commit básico
git commit -m "Mensaje descriptivo"

# 2. Mensaje multilínea (título + descripción)
git commit -m "Título corto" -m "Descripción detallada más larga"

# 3. Abrir editor para mensaje largo
git commit
# → Se abre tu editor configurado
# → Primera línea = título
# → Línea vacía
# → Resto = descripción

# 4. Add + commit automático (SOLO archivos tracked)
git commit -am "Mensaje"
# o: git commit --all -m "Mensaje"
# → Añade y commitea archivos modificados
# → NO añade archivos nuevos (untracked)
# → Útil para cambios rápidos

# 5. Modificar último commit (IMPORTANTE)
git commit --amend -m "Nuevo mensaje"
# → Reemplaza el último commit
# → Útil para corregir errores

# 6. Amend sin cambiar mensaje
git commit --amend --no-edit
# → Añade cambios al último commit
# → Mantiene el mensaje original

# 7. Amend solo el mensaje
git commit --amend
# → Abre editor para cambiar mensaje
# → No añade cambios nuevos

# 8. Commit vacío (útil para CI/CD)
git commit --allow-empty -m "Trigger CI"
# → Crea commit sin cambios
# → Útil para forzar rebuild

# 9. Commit con fecha específica
git commit -m "Mensaje" --date="2024-01-15 10:30:00"
# → Sobrescribe fecha del commit

# 10. Commit como otro autor
git commit -m "Mensaje" --author="Nombre <email@ejemplo.com>"
# → Útil para pair programming
# → O commits de otros

# 11. Commit sin hooks
git commit -m "Mensaje" --no-verify
# o: git commit -m "Mensaje" -n
# → Omite pre-commit y commit-msg hooks
# → Úsalo con CUIDADO

# 12. Commit con template
git commit -t plantilla.txt
# → Usa archivo como plantilla de mensaje

# 13. Commit verboso (muestra diff)
git commit -v
# → Muestra diff en el editor
# → Ayuda a escribir mejor mensaje

# 14. Commit solo de archivos específicos
git commit archivo1.txt archivo2.txt -m "Mensaje"
# → Commitea solo esos archivos (deben estar staged)

# 15. Commit con firma GPG
git commit -S -m "Signed commit"
# → Firma el commit con tu clave GPG
# → Verifica identidad del autor

# 16. Reutilizar mensaje de otro commit
git commit -C <commit-hash>
# → Copia mensaje de otro commit
# O editar el mensaje:
git commit -c <commit-hash>
```

**Casos de uso del --amend:** [🔙](#2-git-commit---guardando-la-historia)

```bash
# Caso 1: Olvidaste un archivo
git add archivo-olvidado.txt
git commit --amend --no-edit
# → Añade el archivo al último commit

# Caso 2: Error de escritura en mensaje
git commit --amend -m "Mensaje corregido"
# → Corrige el mensaje del último commit

# Caso 3: Añadir más cambios al último commit
git add mas-cambios.txt
git commit --amend
# → Añade cambios y edita mensaje si quieres

# ⚠️ IMPORTANTE: Solo usa --amend en commits NO pusheados
# Si ya hiciste push, necesitarás force push (peligroso en ramas compartidas)
```

**Opciones de formato de mensaje:** [🔙](#2-git-commit---guardando-la-historia)

```bash
# Mensaje desde archivo
git commit -F mensaje.txt

# Mensaje desde stdin
echo "Mi mensaje" | git commit -F -

# Limpiar espacios del mensaje
git commit --cleanup=strip -m "  Mensaje con espacios  "
# → Elimina espacios extra

# Mantener mensaje tal cual
git commit --cleanup=verbatim -m "Mensaje exacto"
```

**Commits interactivos:** [🔙](#2-git-commit---guardando-la-historia)

```bash
# Commit interactivo (elige qué añadir)
git commit -p
# → Similar a git add -p + commit
# → Selecciona hunks a commitear
```

**Mensajes de commit efectivos (Conventional Commits):** [🔙](#2-git-commit---guardando-la-historia)

```bash
feat: Add user authentication
fix: Fix login validation bug
docs: Update README
style: Format code
refactor: Simplify auth logic
test: Add integration tests
chore: Update dependencies

# Con scope:
feat(auth): Add login endpoint
fix(api): Handle timeout errors

# Formato completo:
feat(api): Add user registration

- Implement POST /api/register
- Add email validation
- Add password hashing

Closes #123
```

**Troubleshooting común:** [🔙](#2-git-commit---guardando-la-historia)

```bash
# Problema 1: "Nothing to commit"
# Solución: Añade archivos al staging primero
git add .
git commit -m "Mensaje"

# Problema 2: Olvidaste añadir un archivo al commit
# Solución: Usar --amend
git add archivo-olvidado.txt
git commit --amend --no-edit

# Problema 3: Mensaje de commit equivocado
# Solución: Usar --amend
git commit --amend -m "Mensaje correcto"

# Problema 4: Necesitas modificar el último commit
# Solución: Ver ejemplos de --amend arriba
git commit --amend

# Problema 5: Commit en rama equivocada
# Solución: Usar cherry-pick (ver sección de cherry-pick)
# O usar reset para deshacer (ver sección de reset)

# Problema 6: "Please tell me who you are"
# Solución: Configurar identidad
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"

# Problema 7: Editor no se abre o no sabes usar vi
# Solución: Cambiar editor
git config --global core.editor "nano"
# O usar -m directamente:
git commit -m "Mensaje"

# Problema 8: Quieres deshacer un commit
# Solución: Ver sección "git reset" o "git revert" según el caso
```

**Mejores prácticas:** [🔙](#2-git-commit---guardando-la-historia)

```bash
✓ Commits pequeños y atómicos
✓ Mensajes descriptivos (explica POR QUÉ)
✓ Usa convenciones (Conventional Commits)
✓ Usa --amend solo en commits NO pusheados

✗ Evita commits gigantes
✗ Evita mensajes genéricos ("fix", "update")
✗ No uses --amend en commits públicos
```

---

## Navegación

- [⬅️ Anterior: git add](01-git-add.md)
- [🏠 Volver al Índice](../../GIT_COMANDOS_GUIA_PRACTICA.md)
- [➡️ Siguiente: git status](03-git-status.md)

