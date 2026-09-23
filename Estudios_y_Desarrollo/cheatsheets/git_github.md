# 🧠 Git & GitHub Cheat Sheet

Guía rápida y visual para trabajar con Git y GitHub desde la terminal.

---

## 📦 Inicialización y Configuración

| Comando | Descripción | Ejemplo |
|--------|-------------|---------|
| `git init` | Inicializa un repositorio Git local | `git init` |
| `git clone <url>` | Clona un repositorio remoto | `git clone https://github.com/user/repo.git` |
| `git config` | Configura Git global o localmente | `git config --global user.name "Tu Nombre"` |
| `git config --list` | Muestra la configuración actual | `git config --list` |

---

## 🔍 Estado y Logs

| Comando | Descripción | Ejemplo |
|--------|-------------|---------|
| `git status` | Muestra el estado de los archivos | `git status` |
| `git log` | Muestra historial de commits | `git log --oneline --graph` |
| `git diff` | Muestra diferencias entre archivos modificados | `git diff` |

---

## ✍️ Añadir y Confirmar Cambios

| Comando | Descripción | Ejemplo |
|--------|-------------|---------|
| `git add <archivo>` | Añade archivos al staging area | `git add index.html` |
| `git add .` | Añade todos los archivos modificados | `git add .` |
| `git commit -m "mensaje"` | Crea un commit con mensaje | `git commit -m "Añadir nueva sección"` |
| `git commit --amend` | Modifica el último commit | `git commit --amend -m "Mensaje nuevo"` |

---

## 🧭 Ramas (Branches)

| Comando | Descripción | Ejemplo |
|--------|-------------|---------|
| `git branch` | Lista ramas existentes | `git branch` |
| `git branch <nombre>` | Crea una nueva rama | `git branch feature-x` |
| `git checkout <rama>` | Cambia de rama | `git checkout develop` |
| `git checkout -b <rama>` | Crea y cambia a una rama | `git checkout -b fix-login` |
| `git merge <rama>` | Une una rama a la actual | `git merge feature-x` |
| `git branch -d <rama>` | Elimina una rama | `git branch -d vieja-rama` |

---

## 🔁 Sincronización con GitHub

| Comando | Descripción | Ejemplo |
|--------|-------------|---------|
| `git remote add origin <url>` | Asocia el repo local con el remoto | `git remote add origin https://github.com/user/repo.git` |
| `git push -u origin main` | Sube la rama por primera vez y la vincula | `git push -u origin main` |
| `git push` | Sube cambios al remoto | `git push` |
| `git pull` | Trae cambios del remoto y los fusiona | `git pull` |
| `git fetch` | Trae cambios del remoto sin fusionar | `git fetch` |
| `git remote -v` | Lista repositorios remotos | `git remote -v` |

---

## 🛠️ Reescritura y Reparación

| Comando | Descripción | Ejemplo |
|--------|-------------|---------|
| `git reset <archivo>` | Quita archivos del staging | `git reset index.html` |
| `git reset --hard` | Borra todos los cambios locales no guardados | `git reset --hard` |
| `git checkout -- <archivo>` | Restaura archivo al último commit | `git checkout -- index.html` |
| `git revert <hash>` | Revierte un commit sin reescribir el historial | `git revert a1b2c3` |
| `git stash` | Guarda cambios sin commitear para más tarde | `git stash` |
| `git stash apply` | Recupera los cambios guardados | `git stash apply` |

---

## 🔎 Historial y Revisiones

| Comando | Descripción | Ejemplo |
|--------|-------------|---------|
| `git show <hash>` | Muestra detalles de un commit | `git show 3a5b2c1` |
| `git blame <archivo>` | Muestra quién modificó cada línea | `git blame main.py` |

---

## 🌐 GitHub: Acciones comunes

| Acción | Comando / Nota |
|--------|----------------|
| Crear repo en GitHub | Hazlo desde la interfaz web |
| Asociar repo local con GitHub | `git remote add origin https://github.com/usuario/repositorio.git` |
| Autenticación con token | Requiere usar un token personal como contraseña si usas HTTPS |
| Pull Request (PR) | Desde GitHub, haz un PR desde tu rama hacia `main` |
| Issues y Projects | Úsalos para gestionar tareas y bugs |
| GitHub Actions | Automatiza flujos de trabajo: CI/CD, testing, builds, etc |

---

## ⚡ Comandos Rápidos

| Objetivo | Comando |
|---------|---------|
| Clonar y empezar a trabajar | `git clone URL && cd repo` |
| Ver ramas remotas | `git branch -r` |
| Ver commits de una rama | `git log nombre_rama` |
| Revertir último commit (manteniendo cambios) | `git reset --soft HEAD~1` |
| Eliminar todos los cambios locales | `git reset --hard && git clean -fd` |

---

## 🧪 Consejitos Finales

- Usa `.gitignore` para excluir archivos (logs, .env, etc).
- Haz commits pequeños y frecuentes 🧱.
- Nombra tus ramas claramente: `feature/login`, `bugfix/navbar`, `hotfix/typo`.
- Revisa los cambios antes de hacer push: `git diff`, `git status`.
- Usa `pull --rebase` si quieres mantener el historial limpio.

---

> 💡 **TIP:** Usa `git help <comando>` o `man git-<comando>` para ver documentación oficial de cada comando.

