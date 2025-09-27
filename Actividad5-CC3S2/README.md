## Parte 1: Construir - Makefile y Bash desde cero

### Ejecuta make help y guarda la salida para análisis. Luego inspecciona .DEFAULT_GOAL y .PHONY dentro del Makefile.

```
albert@DESKTOP-F43V0VL:/mnt/d/DS-2025-2/CC3S2/Actividad5-CC3S2$ make help
  all           Construir, testear y empaquetar todo
  build         Generar out/hello.txt
  clean         Limpiar archivos generados
  help          Mostrar ayuda
```

El comando help como se puede ver arriba imprime los targets y que hace cada uno de manera automática. Esto gracias a que se busca en el makefile los nombres de los targets usando expresiones regulares y también los comentarios que explican para que funcionan, y los imprime con un formato específico. Por otro lado ```.DEFAULT_GOAL := help``` define que el target help se ejecutará por defecto si no se especifica al usar ```make```. ```.PHONY``` sirve para especificar que los targets ejecutan un conjunto de instrucciones y no son archivos del directorio.

### Comprueba la generación e idempotencia de build. Limpia salidas previas, ejecuta build, verifica el contenido y repite build para constatar que no rehace nada si no cambió la fuente.

Al ejecutar el comando por segunda vez, obtuve un resultado distinto en el documento generado. Primero aparece el mensaje ```make: nothing to be done for 'build'``` y segundo, aparece una marca de tiempo. Esto difiere de la primer build ya que este almacena los comandos para crear la carpeta out y guardar el contenido del script ```hello.py``` en ```/out```.

### Fuerza un fallo controlado para observar el modo estricto del shell y ```.DELETE_ON_ERROR```. Sobrescribe ```PYTHON``` con un intérprete inexistente y verifica que no quede artefacto corrupto.

```.DELETE_ON_ERROR``` evita que los archivos que los archivos de salida cuyo contenido no pudo ser escrito satisfactoriamente por algún error de construcción. Por otro lado el modo estricto de shell hace que cualquier fallo durante la ejecución de un comando y también durante el uso de pipes generé la devolución de un error inmediatamente cancelando así que el programa continue con su ejecución.

### Realiza un "ensayo" (dry-run) y una depuración detallada para observar el razonamiento de Make al decidir si rehacer o no.

Durante la ejecución de los comandos make, se verifica primero si los comandos estan asociados a archivos del proyecto o a conjuntos de instrucciones explicitas, luego se verifica si el archivo de salida ya existe para rehacerlo.   

### Demuestra la incrementalidad con marcas de tiempo. Primero toca la fuente y luego el target para comparar comportamientos

Cambiar la fuente obliga a rehacer porque make detecta que el target está desactualizado respecto de la fuente. Mientras que cambiar el target no requiere de trabajo extra ya que make solo rehace si el target falta o esta desactualizado.

### Ejecuta verificación de estilo/formato manual (sin objetivos lint/tools). Si las herramientas están instaladas, muestra sus diagnósticos; si no, deja evidencia de su ausencia.

Se crearon los archivos pero no se muestra ninguna salida aunque las utilidades ```shfmt``` y ```shellcheck``` estan instaladas

### Construye un paquete reproducible de forma manual, fijando metadatos para que el hash no cambie entre corridas idénticas. Repite el empaquetado y compara hashes.

Hash: 3ac144c8d2072892e1cd8cd5d3caf69175814fecaded9abdf35af0249ac5e809

El parámetro ```--sort=name``` ordena los archivos por nombre y luego ```--mtime=@0``` fija la marca de tiempo de modificación de todos los archivos empaquetados. Después, ```--numeric-owner``` hace que se usen datos numéricos en los metadatos. Finalmnete ```gzip -n``` asegura que el archivo comprimido sea idéntico si el contenido no cambia. Todos estos parámetros en conjunto aseguran que los archivos empaquetados idénticos generen el mismo archivo binario y, por tanto, el mismo hash.

### Reproduce el error clásico "missing separator" sin tocar el Makefile original. Crea una copia, cambia el TAB inicial de una receta por espacios, y confirma el error.

Make exige el uso de TAB al inicio de las recetas por qué al usar espacios, make no espera un comando sino una nueva regla o variable. Para diagnósticar este problema rápidamente puedes ayudarte del editor de código y su configuración que muestra los carácteres invisibles para saber donde están los espacios donde están los TABs.

### 1.3 Crear un script Bash

- Ejecuta ./scripts/run_tests.sh en un repositorio limpio. Observa las líneas "Demostrando pipefail": primero sin y luego con pipefail. Verifica que imprime "Test pasó" y termina exitosamente con código 0 (echo $?).

```
albert@DESKTOP-F43V0VL:/mnt/d/DS-2025-2/CC3S2/Actividad5-CC3S2$ ./scripts/run_tests.sh 
Demostrando pipefail:
Sin pipefail: el pipe se considera exitoso (status 0).
Con pipefail: se detecta el fallo (status != 0).      
Test pasó: Hello, World!
```

- Edita src/hello.py para que no imprima "Hello, World!". Ejecuta el script: verás "Test falló", moverá hello.py a hello.py.bak, y el trap lo restaurará. Confirma código 2 y ausencia de .bak.

```
albert@DESKTOP-F43V0VL:/mnt/d/DS-2025-2/CC3S2/Actividad5-CC3S2$ ./scripts/run_tests.sh 
Demostrando pipefail:
Sin pipefail: el pipe se considera exitoso (status 0).
Con pipefail: se detecta el fallo (status != 0).
Test falló: salida inesperada
```

- Sustituye ```output=$("$PY" "$script")``` por ```("$PY" "$script")```. Ejecuta script. ```output``` queda indefinida; con set -u, al referenciarla en echo aborta antes de grep. El trap limpia y devuelve código distinto no-cero.

```
albert@DESKTOP-F43V0VL:/mnt/d/DS-2025-2/CC3S2/Actividad5-CC3S2$ ./scripts/run_tests.sh 
Demostrando pipefail:
Sin pipefail: el pipe se considera exitoso (status 0).
Con pipefail: se detecta el fallo (status != 0).
Test falló: salida inesperada
```

## Parte 2: Leer - Analizar un repositorio completo

Ejercicios realizados con el script ```Respuestas-actividad5.sh```. Las salidas fueron guardadas en la carpeta ```out``` y ```dist```.

- Ejecuta make -n all para un dry-run que muestre comandos sin ejecutarlos; identifica expansiones $@ y $<, el orden de objetivos y cómo all encadena tools, lint, build, test, package.

```
albert@DESKTOP-F43V0VL:/mnt/d/DS-2025-2/CC3S2/Actividad5-CC3S2$ make -n all
command -v python3 >/dev/null || { echo "Falta python3"; exit 1; }
command -v shellcheck >/dev/null || { echo "Falta shellcheck"; exit 1; }
command -v shfmt >/dev/null || { echo "Falta shfmt"; exit 1; }
command -v grep >/dev/null || { echo "Falta grep"; exit 1; }
command -v awk >/dev/null || { echo "Falta awk"; exit 1; }
command -v tar >/dev/null || { echo "Falta tar"; exit 1; }
tar --version 2>/dev/null | grep -q 'GNU tar' || { echo "Se requiere GNU tar"; exit 1; }
command -v sha256sum >/dev/null || { echo "Falta sha256sum"; exit 1; }
shellcheck scripts/run_tests.sh
shfmt -d scripts/run_tests.sh
command -v ruff >/dev/null 2>&1 && ruff check src || echo "ruff no instalado; omitiendo lint Python"
mkdir -p out
python3 src/hello.py > out/hello.txt
scripts/run_tests.sh
python3 -m unittest discover -s tests -v
mkdir -p dist
tar --sort=name --owner=0 --group=0 --numeric-owner --mtime='UTC 1970-01-01' -czf dist/app.tar.gz -C out hello.txt
```

- Ejecuta make -d build y localiza líneas "Considerando el archivo objetivo" y "Debe deshacerse", explica por qué recompila o no out/hello.txt usando marcas de tiempo y cómo mkdir -p $(@D) garantiza el directorio.

```
albert@DESKTOP-F43V0VL:/mnt/d/DS-2025-2/CC3S2/Actividad5-CC3S2$ make -d build
GNU Make 4.3
Built for x86_64-pc-linux-gnu
Copyright (C) 1988-2020 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Reading makefiles...
Reading makefile 'Makefile'...
Updating makefiles....
 Considering target file 'Makefile'.
  Looking for an implicit rule for 'Makefile'.
  No implicit rule found for 'Makefile'.
  Finished prerequisites of target file 'Makefile'.
 No need to remake target 'Makefile'.
Updating goal targets....
Considering target file 'build'.
 File 'build' does not exist.
  Considering target file 'out/hello.txt'.
    Considering target file 'src/hello.py'.
     Looking for an implicit rule for 'src/hello.py'.
     No implicit rule found for 'src/hello.py'.
     Finished prerequisites of target file 'src/hello.py'.
    No need to remake target 'src/hello.py'.
   Finished prerequisites of target file 'out/hello.txt'.
   Prerequisite 'src/hello.py' is newer than target 'out/hello.txt'.
  Must remake target 'out/hello.txt'.
mkdir -p out
Putting child 0x5643e7d2a390 (out/hello.txt) PID 578 on the chain.
Live child 0x5643e7d2a390 (out/hello.txt) PID 578
Reaping winning child 0x5643e7d2a390 PID 578 
python3 src/hello.py > out/hello.txt
Live child 0x5643e7d2a390 (out/hello.txt) PID 579
Reaping winning child 0x5643e7d2a390 PID 579 
Removing child 0x5643e7d2a390 PID 579 from chain.
  Successfully remade target file 'out/hello.txt'.
 Finished prerequisites of target file 'build'.
Must remake target 'build'.
Successfully remade target file 'build'.
```

- Fuerza un entorno con BSD tar en PATH y corre make tools; comprueba el fallo con "Se requiere GNU tar" y razona por qué --sort, --numeric-owner y --mtime son imprescindibles para reproducibilidad determinista.



- Ejecuta make verify-repro; observa que genera dos artefactos y compara SHA256_1 y SHA256_2. Si difieren, hipótesis: zona horaria, versión de tar, contenido no determinista o variables de entorno no fijadas.

```
SHA256_1=5518f3f447d0f26fe96819cb5d8782372529dfa9e596b47a18932de52043214e
SHA256_2=5518f3f447d0f26fe96819cb5d8782372529dfa9e596b47a18932de52043214e
OK: reproducible
```

- Corre make clean && make all, cronometrando; repite make all sin cambios y compara tiempos y logs. Explica por qué la segunda es más rápida gracias a timestamps y relaciones de dependencia bien declaradas.


- Ejecuta PYTHON=python3.12 make test (si existe). Verifica con python3.12 --version y mensajes que el override funciona gracias a ?= y a PY="${PYTHON:-python3}" en el script; confirma que el artefacto final no cambia respecto al intérprete por defecto.

```
Python 3.12.3
```

- Ejecuta make test; describe cómo primero corre scripts/run_tests.sh y luego python -m unittest. Determina el comportamiento si el script de pruebas falla y cómo se propaga el error a la tarea global.

```
scripts/run_tests.sh
Demostrando pipefail:
Sin pipefail: el pipe se considera exitoso (status 0).
Con pipefail: se detecta el fallo (status != 0).
Test falló: salida inesperada
make: *** [Makefile:29: test] Error 2
```

- Ejecuta touch src/hello.py y luego make all; identifica qué objetivos se rehacen (build, test, package) y relaciona el comportamiento con el timestamp actualizado y la cadena de dependencias especificada.

```
shellcheck scripts/run_tests.sh
shfmt -d scripts/run_tests.sh
All checks passed!
mkdir -p out
python3 src/hello.py > out/hello.txt
scripts/run_tests.sh
Demostrando pipefail:
Sin pipefail: el pipe se considera exitoso (status 0).
Con pipefail: se detecta el fallo (status != 0).
Test falló: salida inesperada
make: *** [Makefile:29: test] Error 2
```

- Ejecuta make -j4 all y observa ejecución concurrente de objetivos independientes; confirma resultados idénticos a modo secuencial y explica cómo mkdir -p $(@D) y dependencias precisas evitan condiciones de carrera.


- Ejecuta make lint y luego make format; interpreta diagnósticos de shellcheck, revisa diferencias aplicadas por shfmt y, si está disponible, considera la salida de ruff sobre src/ antes de empaquetar.

```
shellcheck scripts/run_tests.sh
shfmt -d scripts/run_tests.sh
All checks passed!
```

```
shfmt -w scripts/run_tests.sh
```

## Parte 3: Extender

### 3.1 lint mejorado

Rompe a propósito un quoting en scripts/run_tests.sh (por ejemplo, quita comillas a una variable que pueda contener espacios) y ejecuta make lint. shellcheck debe reportar el problema; corrígelo y vuelve a correr.
Luego ejecuta make format para aplicar shfmt y estandarizar estilo. Si tienes ruff, inspecciona Python y corrige advertencias.

```
albert@DESKTOP-F43V0VL:/mnt/d/DS-2025-2/CC3S2/Actividad5-CC3S2$ make lint
shellcheck scripts/run_tests.sh

In scripts/run_tests.sh line 21:
        rm -f $tmp
              ^--^ SC2086 (info): Double quote to prevent globbing and word splitting.

Did you mean: 
        rm -f "$tmp"


In scripts/run_tests.sh line 44:
        output="$($PY $script)"
                      ^-----^ SC2086 (info): Double quote to prevent globbing and word splitting.

Did you mean: 
        output="$($PY "$script")"

For more information:
  https://www.shellcheck.net/wiki/SC2086 -- Double quote to prevent globbing ...
make: *** [Makefile:39: lint] Error 1
```

```
albert@DESKTOP-F43V0VL:/mnt/d/DS-2025-2/CC3S2/Actividad5-CC3S2$ make format
shfmt -w scripts/run_tests.sh
```

```
albert@DESKTOP-F43V0VL:/mnt/d/DS-2025-2/CC3S2/Actividad5-CC3S2$ ruff check src || true
All checks passed!
```

### 3.2 Rollback adicional

Este chequeo asegura que, si el temporal desaparece, el script falla limpiamente y el trap revierte el estado (restaura hello.py desde .bak) preservando el código de salida.


```
albert@DESKTOP-F43V0VL:/mnt/d/DS-2025-2/CC3S2/Actividad5-CC3S2$ ./scripts/run_tests.sh 
Demostrando pipefail:
Sin pipefail: el pipe se considera exitoso (status 0).
Con pipefail: se detecta el fallo (status != 0).
Test falló: salida inesperada
```

### 3.3 Incrementalidad

Ejecuta dos veces make benchmark para comparar un build limpio frente a uno cacheado; revisa out/benchmark.txt. Después, modifica el timestamp del origen con touch src/hello.py y repite. Observa que build, test y package se rehacen.


```
Benchmark: 2025-09-26 18:54:16 / Commit: 0616785
make[1]: Entering directory '/mnt/d/DS-2025-2/CC3S2/Actividad5-CC3S2'
shellcheck scripts/run_tests.sh
shfmt -d scripts/run_tests.sh
All checks passed!
scripts/run_tests.sh
Demostrando pipefail:
Sin pipefail: el pipe se considera exitoso (status 0).
Con pipefail: se detecta el fallo (status != 0).
Test falló: salida inesperada
make[1]: *** [Makefile:29: test] Error 2
make[1]: Leaving directory '/mnt/d/DS-2025-2/CC3S2/Actividad5-CC3S2'
Command exited with non-zero status 2
Tiempo: 0:00.48
```

```
Benchmark: 2025-09-26 18:53:38 / Commit: 0616785
make[1]: Entering directory '/mnt/d/DS-2025-2/CC3S2/Actividad5-CC3S2'
shellcheck scripts/run_tests.sh
shfmt -d scripts/run_tests.sh
All checks passed!
mkdir -p out
python3 src/hello.py > out/hello.txt
scripts/run_tests.sh
Demostrando pipefail:
Sin pipefail: el pipe se considera exitoso (status 0).
Con pipefail: se detecta el fallo (status != 0).
Test falló: salida inesperada
make[1]: *** [Makefile:29: test] Error 2
make[1]: Leaving directory '/mnt/d/DS-2025-2/CC3S2/Actividad5-CC3S2'
Command exited with non-zero status 2
Tiempo: 0:00.72
```

### Checklist de Smoke-Tests - Bootstrap

Confirma permisos de ejecución del script, presencia de herramientas requeridas y ayuda autodocumentada. Espera que make tools falle temprano si falta una dependencia, con mensaje específico.

```
albert@DESKTOP-F43V0VL:/mnt/d/DS-2025-2/CC3S2/Actividad5-CC3S2$ chmod +x scripts/run_tests.sh
albert@DESKTOP-F43V0VL:/mnt/d/DS-2025-2/CC3S2/Actividad5-CC3S2$ make tools
albert@DESKTOP-F43V0VL:/mnt/d/DS-2025-2/CC3S2/Actividad5-CC3S2$ make help
  all           Construir, testear y empaquetar todo
  build         Generar out/hello.txt
  test          Ejecutar tests
  package       Crear dist/app.tar.gz
  lint          Lint Bash y (opcional) Python
  tools         Verificar dependencias
  check         Ejecutar lint y tests
  benchmark     Medir tiempo de ejecución
  format        Formatear scripts con shfmt
  dist-clean    Limpiar todo (incluye caches opcionales)
  verify-repro  Verificar que dist/app.tar.gz sea 100% reproducible
  clean         Limpiar archivos generados
  help          Mostrar ayuda
```

### Checklist de Smoke-Tests - Lint y formato
Ejecuta make lint y corrige problemas de shell reportados por shellcheck. Aplica make format para normalizar estilo con shfmt. Si ruff está disponible, revisa src y corrige advertencias.

```
albert@DESKTOP-F43V0VL:/mnt/d/DS-2025-2/CC3S2/Actividad5-CC3S2$ make lint
shellcheck scripts/run_tests.sh
shfmt -d scripts/run_tests.sh
All checks passed!
albert@DESKTOP-F43V0VL:/mnt/d/DS-2025-2/CC3S2/Actividad5-CC3S2$ make format
shfmt -w scripts/run_tests.sh
albert@DESKTOP-F43V0VL:/mnt/d/DS-2025-2/CC3S2/Actividad5-CC3S2$ ruff check src || true
All checks passed!
```

### Checklist de Smoke-Tests - Limpieza

Asegura un entorno de compilación limpio y reproducible para CI y pruebas locales. make dist-clean elimina artefactos (out/, dist/) y cachés (.ruff_cache, __pycache__).

```
albert@DESKTOP-F43V0VL:/mnt/d/DS-2025-2/CC3S2/Actividad5-CC3S2$ make all
shellcheck scripts/run_tests.sh
shfmt -d scripts/run_tests.sh
All checks passed!
scripts/run_tests.sh
Demostrando pipefail:
Sin pipefail: el pipe se considera exitoso (status 0).
Con pipefail: se detecta el fallo (status != 0).
Test falló: salida inesperada
make: *** [Makefile:29: test] Error 2
```

### Checklist de Smoke-Tests - Reproducibilidad

Valida que dos empaquetados consecutivos generen el mismo hash SHA256. Si difieren, revisa versión de tar (debe ser GNU), zona horaria (TZ=UTC), permisos y que no se cuelen archivos extra.

```
albert@DESKTOP-F43V0VL:/mnt/d/DS-2025-2/CC3S2/Actividad5-CC3S2$ make verify-repro
SHA256_1=5518f3f447d0f26fe96819cb5d8782372529dfa9e596b47a18932de52043214e
SHA256_2=5518f3f447d0f26fe96819cb5d8782372529dfa9e596b47a18932de52043214e
OK: reproducible
```