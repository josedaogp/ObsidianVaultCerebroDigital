# 🚀 Git & GitHub Cheatsheet 🚀

Esta hoja de trucos cubre los comandos y procesos más comunes de Git y GitHub, incluyendo `.gitignore`, Gitflow, y más. ¡Tenla a mano para un flujo de trabajo eficiente!

## 📖 Índice

1.  [Configuración Inicial](#-configuración-inicial) ⚙️
2.  [Conceptos Básicos](#-conceptos-básicos) 💡
3.  [Comandos Esenciales](#-comandos-esenciales) 🛠️
4.  [Ramas (Branches)](#-ramas-branches) 🌱
5.  [Deshacer Cambios](#-deshacer-cambios) ↩️
6.  [Trabajando con Remotos (GitHub)](#-trabajando-con-remotos-github) ☁️
7.  [.gitignore](#-gitignore) 🚫
8.  [Gitflow](#-gitflow) 🌊
9.  [Resolución de Conflictos](#-resolución-de-conflictos) ⚔️
10. [Etiquetas (Tags)](#-etiquetas-tags)🏷️
11. [GitHub Actions](#-github-actions) 🤖
12. [Consejos Adicionales](#-consejos-adicionales) ✨

---

## 1. ⚙️ Configuración Inicial

*   **Configurar tu identidad:**

    ```bash
    git config --global user.name "Tu Nombre"
    git config --global user.email "tu@email.com"
    ```

    👁️‍🗨️ **Nota:** `--global` aplica la configuración a todos tus repositorios.  Usa `--local` para configurar un repositorio específico (omite `--global`).

* **Configurar tu editor de texto:**

    ```bash
    git config --global core.editor "code --wait"  # Ejemplo con VS Code
    #Otras opciones: nano, vim, etc.
    ```

    👁️‍🗨️**Nota:** `--wait` le dice a git que espere a que cierres el editor para continuar.

*   **Inicializar un repositorio Git:**

    ```bash
    git init  # En la carpeta raíz de tu proyecto
    ```
*   **Ver la configuración actual de Git**

    ```bash
    git config --list
    git config --list --show-origin # Te muestra de que archivo viene cada configuración
    ```
---

## 2. 💡 Conceptos Básicos

*   **Repositorio:**  Un directorio que Git rastrea.  Contiene todo el historial de cambios.
*   **Commit:** Una "foto" del estado de tu proyecto en un momento dado.
*   **Stage (Área de Preparación):**  Donde seleccionas los cambios que quieres incluir en el próximo commit.
*   **Branch (Rama):**  Una línea de desarrollo independiente.  Útil para trabajar en nuevas características sin afectar el código principal.
*   **HEAD:**  Un puntero que indica el commit actual (la punta de la rama actual).
*   **Remote (Remoto):**  Un repositorio en otro lugar (como GitHub).
*   **Push:** Enviar tus commits locales a un repositorio remoto.
*   **Pull:** Traer los cambios de un repositorio remoto a tu local.
*   **Merge:**  Combinar los cambios de una rama en otra.
*   **Clone:** Crear una copia local de un repositorio remoto.

---

## 3. 🛠️ Comandos Esenciales

*   **Estado del repositorio:**

    ```bash
    git status  # Muestra los archivos modificados, en stage, etc.
    ```

*   **Añadir archivos al stage:**

    ```bash
    git add <archivo>      # Añade un archivo específico
    git add .             # Añade todos los archivos modificados
    git add -u            # Añade solo los archivos modificados, no los nuevos.
    git add *.txt         # Añade todos los archivos .txt
    ```

*   **Crear un commit:**

    ```bash
    git commit -m "Mensaje descriptivo del commit"
    git commit -am "Mensaje" # Añade y commit en un paso (solo archivos modificados)
    ```

    👁️‍🗨️**Nota:** ¡Escribe mensajes de commit claros y concisos!

* **Ver el historial de commits:**

    ```bash
    git log          # Muestra el historial completo
    git log --oneline # Muestra el historial en formato compacto
    git log -p       # Muestra los cambios (diff) en cada commit
    git log --graph  # Muestra un gráfico de las ramas y merges
    git log --author="Nombre" #Filtra por autor
    git log --grep="palabra clave" #Busca commits por palabra clave en el mensaje.
    git log --after="2023-10-01" --before="2023-10-31" #Filtra por rango de fechas.
    git log -- <ruta_al_archivo> # Muestra el historial de un archivo específico.
    ```

*   **Ver los cambios (diff):**

    ```bash
    git diff           # Muestra los cambios no añadidos al stage
    git diff --staged  # Muestra los cambios en el stage
    git diff <commit1> <commit2>  # Compara dos commits
    git diff <rama1> <rama2> #Compara dos ramas
    ```

---

## 4. 🌱 Ramas (Branches)

*   **Listar ramas:**

    ```bash
    git branch        # Lista las ramas locales
    git branch -r     # Lista las ramas remotas
    git branch -a     # Lista todas las ramas (locales y remotas)
    ```

*   **Crear una nueva rama:**

    ```bash
    git branch <nombre-rama> #Crea una rama a partir del commit actual.
    git branch <nombre-rama> <commit> #Crea una rama a partir de un commit específico.
    ```

*   **Cambiar a otra rama:**

    ```bash
    git checkout <nombre-rama>
    git switch <nombre-rama>  # Alternativa más moderna a checkout (Git 2.23+)
    ```

*   **Crear y cambiar a una nueva rama en un solo paso:**

    ```bash
    git checkout -b <nombre-rama>  # ¡Muy útil!
    git switch -c <nombre-rama> # Alternativa moderna.
    ```

* **Combinar ramas (merge):**

    ```bash
    git checkout <rama-destino>  # Cambia a la rama donde quieres integrar los cambios
    git merge <rama-a-combinar>  # Combina la rama especificada en la rama actual
    ```

*   **Borrar una rama:**

    ```bash
    git branch -d <nombre-rama>   # Borra la rama localmente (si ya se combinó)
    git branch -D <nombre-rama>   # Fuerza el borrado (incluso si no se combinó)
    git push origin --delete <nombre-rama> # Borra una rama remota
    ```
    👁️‍🗨️**Nota:** Usa `-D` con precaución, puedes perder trabajo no guardado.

*   **Renombrar una rama:**

    ```bash
    git branch -m <nombre-antiguo> <nombre-nuevo> #Renombra la rama actual.
    ```

* **Ver la rama actual:**

    ```bash
    git branch --show-current
    ```

---

## 5. ↩️ Deshacer Cambios

*   **Descartar cambios locales (no en stage):**

    ```bash
    git checkout -- <archivo>  # ¡Cuidado!  Cambios no guardados se pierden
    git clean -f #Elimina todos los archivos sin seguimiento (untracked)
    git clean -fd # Elimina archivos y directorios untracked.
    git clean -n # Simula la limpieza, mostrando qué se eliminaría, sin borrar nada.
    ```

*   **Quitar un archivo del stage:**

    ```bash
    git restore --staged <archivo>
    git reset HEAD <archivo> #Forma antigua, pero aun funciona
    ```

*   **Deshacer un commit (sin borrarlo del historial):**

    ```bash
    git revert <commit>  # Crea un nuevo commit que deshace los cambios
    ```
    👁️‍🗨️**Nota:** `revert` es seguro para usar en ramas compartidas.

*   **Modificar el último commit (¡solo si no lo has compartido!):**

    ```bash
    git commit --amend -m "Nuevo mensaje"  # Cambia el mensaje del último commit
    git commit --amend  # Abre el editor para modificar el mensaje y/o añadir cambios
    ```
    👁️‍🗨️ **¡Importante!**  Nunca uses `amend` en commits que ya hayas enviado (`push`) a un repositorio remoto compartido. Reescribe el historial, y eso causa problemas a otros.

* **Deshacer commits (borrándolos del historial - ¡peligroso en ramas compartidas!):**
    * `git reset --soft <commit>`:  Mueve HEAD al commit especificado, pero mantiene los cambios en el stage.
    * `git reset --mixed <commit>`: Mueve HEAD y quita los cambios del stage, pero los mantiene en tu directorio de trabajo.
    *   `git reset --hard <commit>`:  Mueve HEAD, quita los cambios del stage *y* los borra de tu directorio de trabajo.  **¡Cuidado!**

    👁️‍🗨️**Nota:**  `reset` es peligroso si lo usas en commits que ya has compartido.  Reescribe el historial.

---

## 6. ☁️ Trabajando con Remotos (GitHub)

*   **Clonar un repositorio remoto:**

    ```bash
    git clone <url-del-repositorio>
    ```

*   **Listar repositorios remotos:**

    ```bash
    git remote -v  # Muestra los remotos y sus URLs
    ```

*   **Añadir un repositorio remoto:**

    ```bash
    git remote add <nombre-remoto> <url-del-repositorio>
    # Ejemplo: git remote add origin https://github.com/tuusuario/tu-repo.git
    ```
     👁️‍🗨️**Nota:** "origin" es el nombre convencional para el remoto principal.

*   **Enviar cambios a un remoto (push):**

    ```bash
    git push <nombre-remoto> <nombre-rama>
    git push -u origin main # "-u" configura el seguimiento para futuros push/pull
    git push --all <nombre-remoto> #Envía todas las ramas locales
    git push --tags #Envía todas las etiquetas
    ```

*   **Traer cambios de un remoto (pull):**

    ```bash
    git pull <nombre-remoto> <nombre-rama> #Trae cambios y combina (fetch + merge)
    git pull --rebase <nombre-remoto> <nombre-rama> #Trae cambios y aplica rebase (ver abajo)
    ```

*   **Fetch (traer cambios sin combinar):**

    ```bash
    git fetch <nombre-remoto>  # Trae los cambios, pero no los combina
    git fetch --all #Trae los cambios de todos los remotos
    ```
    👁️‍🗨️**Nota:** `fetch` es útil para ver qué hay de nuevo sin afectar tu rama actual.

*  **Rebase**
    ```bash
      git checkout <feature-branch>
      git rebase <base-branch> # Por ejemplo: git rebase main
    ```
    👁️‍🗨️**Nota:** Rebase reescribe la historia de tu rama, haciendo que parezca que trabajaste de manera lineal, como si hubieras creado la rama despues de todos los cambios de main. Evita `rebase` en ramas públicas o compartidas a menos que sepas muy bien lo que haces.

---

## 7. 🚫 .gitignore

*   Un archivo de texto llamado `.gitignore` en la raíz de tu repositorio.
*   Especifica archivos y directorios que Git debe ignorar (no rastrear).
*   Útil para evitar subir archivos generados, dependencias, archivos de configuración locales, etc.

**Ejemplo de `.gitignore`:**

```
# Ignorar archivos de configuración de editores
.idea/
*.swp

# Ignorar archivos compilados
*.o
*.exe
*.class

# Ignorar logs
logs/
*.log

# Ignorar dependencias (ejemplo con Node.js)
node_modules/

# Ignorar archivos de configuración específicos
config.local.php

#Ignorar carpetas
carpeta_a_ignorar/

#Ignorar archivos por extensión
*.pdf

#Excepciones
!importante.pdf #Este archivo sí se incluirá

```

👁️‍🗨️ **¡Importante!** Si un archivo ya está siendo rastreado por Git, añadirlo a `.gitignore` *no* lo eliminará de tu repositorio.  Primero debes usar `git rm --cached <archivo>`.

---

## 8. 🌊 Gitflow

Un modelo de ramificación popular para gestionar proyectos.

*   **Ramas principales:**
    *   `main` (o `master`):  Código listo para producción.
    *   `develop`:  Rama de integración.  Contiene las últimas características en desarrollo.

*   **Ramas de soporte:**
    *   `feature/<nombre>`:  Para desarrollar nuevas características.  Se crean a partir de `develop` y se combinan de nuevo en `develop`.
    *   `release/<versión>`:  Para preparar una nueva versión para producción.  Se crean a partir de `develop` y se combinan en `main` y `develop`.
    *   `hotfix/<descripción>`:  Para corregir errores críticos en producción. Se crean a partir de `main` y se combinan en `main` y `develop`.

**Flujo de trabajo básico de Gitflow:**

1.  **Nueva característica:**
    *   `git checkout -b feature/mi-caracteristica develop`
    *   (Desarrollar la característica, hacer commits)
    *   `git checkout develop`
    *   `git merge --no-ff feature/mi-caracteristica` (El `--no-ff` crea un commit de merge, preservando el historial)
    * `git branch -d feature/mi-caracteristica`

2.  **Nueva versión (release):**
    *   `git checkout -b release/1.2.0 develop`
    *   (Actualizar número de versión, preparar documentación, etc.)
    *   `git checkout main`
    *   `git merge --no-ff release/1.2.0`
    *   `git tag -a 1.2.0 -m "Versión 1.2.0"` (Crear una etiqueta)
    *   `git checkout develop`
    *   `git merge --no-ff release/1.2.0`
    *   `git branch -d release/1.2.0`
    *   `git push origin main develop 1.2.0` (Enviar la etiqueta también).

3.  **Hotfix:**
    *   `git checkout -b hotfix/problema-urgente main`
    *   (Corregir el error, hacer commits)
    *   `git checkout main`
    *   `git merge --no-ff hotfix/problema-urgente`
    *   `git tag -a 1.2.1 -m "Hotfix 1.2.1"`
    *   `git checkout develop`
    *   `git merge --no-ff hotfix/problema-urgente`
    *   `git branch -d hotfix/problema-urgente`
    *  `git push origin main develop 1.2.1`

👁️‍🗨️**Nota:** Gitflow es solo un modelo.  Puedes adaptarlo a tus necesidades. Hay herramientas que automatizan parte de Gitflow (como `git-flow`).

---

## 9. ⚔️ Resolución de Conflictos

*   Ocurren cuando Git no puede combinar automáticamente los cambios de dos ramas.
*   Git marca las áreas conflictivas en los archivos afectados.

**Ejemplo de conflicto:**

```
<<<<<<< HEAD
Este es el contenido de mi rama actual.
=======
Este es el contenido de la otra rama.
>>>>>>> otra-rama
```

**Pasos para resolver conflictos:**

1.  **Editar el archivo:**  Abre el archivo y decide qué código mantener. Elimina los marcadores `<<<<<<<`, `=======`, `>>>>>>>`.
2.  **Añadir el archivo al stage:** `git add <archivo>`
3.  **Crear un commit:** `git commit -m "Resueltos conflictos de merge"`

---
## 10. 🏷️ Etiquetas (Tags)
*   **Crear una etiqueta ligera**
    ```bash
        git tag <nombre-etiqueta>
    ```
*   **Crear etiqueta anotada:**
    ```bash
        git tag -a <nombre-etiqueta> -m "Mensaje de la etiqueta"
    ```
*   **Listar etiquetas:**

    ```bash
    git tag #Lista las etiquetas
    git tag -l "v1.*" #Lista usando patrones.
    git show <nombre-etiqueta> #Muestra información de la etiqueta
    ```

*   **Crear etiqueta en un commit pasado:**
    ```bash
    git tag -a v1.2 <commit-hash> -m "Versión 1.2"
    ```
* **Compartir etiquetas:**
    ```bash
      git push origin <nombre-etiqueta> #Sube una etiqueta en especifico
      git push origin --tags #Sube todas las etiquetas al repositorio remoto
    ```
*  **Borrar etiqueta:**
    ```bash
        git tag -d <nombre-etiqueta> #Borra la etiqueta localmente
        git push origin --delete <nombre-etiqueta> #Borra la etiqueta del remoto
    ```
---
## 11. 🤖 GitHub Actions
*   **Definición:** Plataforma de CI/CD integrada en GitHub.
*   **Archivos YAML:** Los flujos de trabajo se definen en archivos `.yml` dentro del directorio `.github/workflows`.

**Ejemplo básico de un workflow:**

```yaml
name: Mi Workflow

on: [push]  # Se ejecuta en cada push

jobs:
  build:
    runs-on: ubuntu-latest  # Sistema operativo

    steps:
      - uses: actions/checkout@v3  # Clona el repositorio

      - name: Ejecutar pruebas
        run: |
          npm install
          npm test
```

---

## 12. ✨ Consejos Adicionales

*   **Usa un buen editor de código:**  VS Code, Sublime Text, Atom, etc., con integración con Git.
*   **Aprende a usar la línea de comandos:**  Es la forma más poderosa de interactuar con Git.
*   **Lee la documentación oficial de Git:**  Es muy completa.
*   **Practica, practica, practica:**  La mejor forma de aprender Git es usándolo.
*   **No tengas miedo de experimentar:**  Crea repositorios de prueba para probar cosas.
* **Usa alias de Git**: Para abreviar y personalizar comandos

    ```bash
      git config --global alias.co checkout
      git config --global alias.br branch
      git config --global alias.ci commit
      git config --global alias.st status
      git config --global alias.l "log --oneline --graph --decorate" #Un log más completo
    ```



