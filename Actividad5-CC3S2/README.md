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
