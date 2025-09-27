# Actividad 6: Introducción a Git conceptos básicos y operaciones esenciales

## git config: Preséntate a Git

Ahora, hay algo que debes hacer antes de comenzar un proyecto. Preséntate a Git. Para presentarte a Git, usa el comando git config:

```
Albert@DESKTOP-F43V0VL MINGW64 /d/DS-2025-2/CC3S2/Actividad6-CC3S2 (main)
$ git config --global user.name
Alkerg

Albert@DESKTOP-F43V0VL MINGW64 /d/DS-2025-2/CC3S2/Actividad6-CC3S2 (main)
$ git config --global user.email
albert.arg500@gmail.com
```

## git init: Donde comienza tu viaje de código

Al igual que cada gran viaje tiene su origen, en el mundo de Git, el viaje de tu código comienza con el comando git init. El comando se usa para inicializar un nuevo repositorio de Git y comenzar a rastrear directorios existentes. Cuando ejecutas el comando, configura un directorio .git lleno de todo lo necesario para el control de versiones.

```

Albert@DESKTOP-F43V0VL MINGW64 /d/DS-2025-2/test-repo
$ git init
Initialized empty Git repository in D:/DS-2025-2/test-repo/.git/
```

## git add: Preparando tu código

El comando git add es tu puente entre hacer cambios en tu directorio de trabajo y prepararlos para ser almacenados permanentemente en tu repositorio de Git.

```
Albert@DESKTOP-F43V0VL MINGW64 /d/DS-2025-2/test-repo (main)
$ echo " README" > README.md

Albert@DESKTOP-F43V0VL MINGW64 /d/DS-2025-2/test-repo (main)
$ git status
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md

nothing added to commit but untracked files present (use "git add" to track)

```

```
Albert@DESKTOP-F43V0VL MINGW64 /d/DS-2025-2/test-repo (main)
$ git add README.md

Albert@DESKTOP-F43V0VL MINGW64 /d/DS-2025-2/test-repo (main)
$ git status
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   README.md

```

## git commit: registra cambios

```
Albert@DESKTOP-F43V0VL MINGW64 /d/DS-2025-2/test-repo (main)
$ git commit -m "Commit inicial con README.md"
[main (root-commit) 41ab400] Commit inicial con README.md
 1 file changed, 1 insertion(+)
 create mode 100644 README.md

Albert@DESKTOP-F43V0VL MINGW64 /d/DS-2025-2/test-repo (main)
$ git status
On branch main
nothing to commit, working tree clean
```

## git log: Recorrer el árbol de commits

```
Albert@DESKTOP-F43V0VL MINGW64 /d/DS-2025-2/test-repo (main)
$ git log
commit 41ab4008ce427b16a8d519d78bc49986db794b04 (HEAD -> main)
Author: Alkerg <albert.arg500@gmail.com>
Date:   Fri Sep 26 22:08:22 2025 -0500

    Commit inicial con README.md

Albert@DESKTOP-F43V0VL MINGW64 /d/DS-2025-2/test-repo (main)
$ git log --graph --pretty=format:'%x09 %h %ar ("%an") %s'
*        41ab400 2 minutes ago ("Alkerg") Commit inicial con README.md

```

```
Albert@DESKTOP-F43V0VL MINGW64 /d/DS-2025-2/test-repo (main)
$ echo " CONTRIBUTING" > CONTRIBUTING.md

Albert@DESKTOP-F43V0VL MINGW64 /d/DS-2025-2/test-repo (main)
$ echo " README\n\nBienvenido al proyecto" > README.md

Albert@DESKTOP-F43V0VL MINGW64 /d/DS-2025-2/test-repo (main)
$ git add .

Albert@DESKTOP-F43V0VL MINGW64 /d/DS-2025-2/test-repo (main)
$ git commit -m "Configura la documentación base del repositorio"
[main 1d0663d] Configura la documentación base del repositorio
 2 files changed, 2 insertions(+), 1 deletion(-)
 create mode 100644 CONTRIBUTING.md

Albert@DESKTOP-F43V0VL MINGW64 /d/DS-2025-2/test-repo (main)
$ echo "print('Hello World')" > main.py

Albert@DESKTOP-F43V0VL MINGW64 /d/DS-2025-2/test-repo (main)
$ git add .

Albert@DESKTOP-F43V0VL MINGW64 /d/DS-2025-2/test-repo (main)
$ git commit -m "Agrega main.py"
[main 4dfb704] Agrega main.py
 1 file changed, 1 insertion(+)
 create mode 100644 main.py
```

```
Albert@DESKTOP-F43V0VL MINGW64 /d/DS-2025-2/test-repo (main)
$  git log --oneline
4dfb704 (HEAD -> main) Agrega main.py
1d0663d Configura la documentación base del repositorio
41ab400 Commit inicial con README.md
```

# Trabajar con ramas: La piedra angular de la colaboración

## git branch: Entendiendo los conceptos básicos de Git branch

Cuando inicializas un repositorio de Git, automáticamente crea una rama (branch) predeterminada, generalmente llamada main (anteriormente conocida como master). Cuando ejecutas el comando git branch, mostrará la lista de todas las ramas en tu repositorio, con la branch actual destacada:

```
Albert@DESKTOP-F43V0VL MINGW64 /d/DS-2025-2/test-repo (main)
$ git branch
* main
```

## git checkout/git switch: Cambiar entre ramas

En tu flujo de trabajo diario, a menudo necesitarás cambiar de una rama a otra, especialmente cuando trabajas en múltiples características o corrigiendo errores. Cuando hayas comenzado a trabajar en múltiples ramas, volverse consciente de la branch en la que estás activamente se vuelve fundamental. En Git, el término HEAD se refiere a la punta de la rama con la que estás trabajando activamente.


```
Albert@DESKTOP-F43V0VL MINGW64 /d/DS-2025-2/test-repo (main)
$ git branch feature/new-feature

Albert@DESKTOP-F43V0VL MINGW64 /d/DS-2025-2/test-repo (main)
$ git checkout feature/new-feature
Switched to branch 'feature/new-feature'
```

```
Albert@DESKTOP-F43V0VL MINGW64 /d/DS-2025-2/test-repo (feature/new-feature)
$ git branch
* feature/new-feature
  main

Albert@DESKTOP-F43V0VL MINGW64 /d/DS-2025-2/test-repo (develop)
$ git branch feature/login develop

Albert@DESKTOP-F43V0VL MINGW64 /d/DS-2025-2/test-repo (develop)
$ git checkout feature/login
Switched to branch 'feature/login'

Albert@DESKTOP-F43V0VL MINGW64 /d/DS-2025-2/test-repo (feature/login)
$ git log --oneline
4dfb704 (HEAD -> feature/login, main, feature/new-feature, develop) Agrega main.py
1d0663d Configura la documentación base del repositorio
41ab400 Commit inicial con README.md

```
## git merge : Fusionando ramas

Una vez que hayas realizado cambios en una rama y los hayas probado a fondo, es posible que desees integrar esos cambios nuevamente en la branch main u otra rama. Esta operación se conoce como merge (fusión):

```
Albert@DESKTOP-F43V0VL MINGW64 /d/DS-2025-2/test-repo (feature/another-new-feature)
$ git checkout main
Switched to branch 'main'

Albert@DESKTOP-F43V0VL MINGW64 /d/DS-2025-2/test-repo (main)
$ git merge feature/new-feature
Already up to date.
```

## git branch -d: Eliminando una rama

Una vez que una rama ha sido fusionada con éxito y ya no es necesaria, se puede eliminar para mantener limpio el repositorio:

```
Albert@DESKTOP-F43V0VL MINGW64 /d/DS-2025-2/test-repo (main)
$ git branch -d feature/new-feature
Deleted branch feature/new-feature (was 4dfb704).
```

## Preguntas

- **¿Cómo te ha ayudado Git a mantener un historial claro y organizado de tus cambios?**

- **¿Qué beneficios ves en el uso de ramas para desarrollar nuevas características o corregir errores?**

- **Realiza una revisión final del historial de commits para asegurarte de que todos los cambios se han registrado correctamente.**

- **Revisa el uso de ramas y merges para ver cómo Git maneja múltiples líneas de desarrollo.**

## Ejercicios

### Ejercicio 1: Manejo avanzado de ramas y resolución de conflictos
