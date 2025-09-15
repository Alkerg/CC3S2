## Parte teórica
#### Introducción a DevOps: ¿Qué es y qué no es?
**Explica DevOps desde el código hasta la producción, diferenciándolo de waterfall. Discute "you build it, you run it" en el laboratorio, y separa mitos (ej. solo herramientas) vs realidades (CALMS, feedback, métricas, gates).**

DevOps es una cultura de desarrollo que fomenta la colaboración, las buenas prácticas y la automatización, a diferencia de waterfall que se basa en fases de desarrollo secuencial, lo cual es más costoso y lento, en especial cuando hay fallos. El ciclo de devops empieza con la creación de un repositorio de control de versiones con herramientas como github. Al realizar cada commit se ejecutan pipelines que prueban y validan el código de los desarrolladores para detectar errores temprano. Luego el código es lanzado en pre producción con nuevamente con pruebas automatizadas hasta que finalmente se realiza el despliegue en producción siguiendo los principios de observabilidad y monitoreo. "You build it, you run it" significa que el mismo equipo que codifica se encarga de definir pipelines, tests y monitoreo. 
Por otro lado algunos mitos sobre devops son los siguientes:
- "Devops son solo herramientas (jira, docker)", esto no es así ya que si bien las herramientas facilitan algunas tareas, devops es una cultura de desarrollo que incluye principios y estrategías para diversas áreas del proyecto que ayudan a una mejor sinergia entre los colaboradores.
- "Devops es rápido pero inseguro", esto no es así ya que la seguridad se piensa y se integra desde el inicio del desarrollo.

#### Marco CALMS en acción:
**Describe cada pilar y su integración en el laboratorio (ej. Automation con Makefile, Measurement con endpoints de salud). Propón extender Sharing con runbooks/postmortems en equipo.**

El pilar de automation se integra con la automatización de tareas a través de make, que permite compilar, probar, empaquetar y limpiar el proyecto con simples comandos en lugar de ejecutar comandos largos a mano. Por ejemplo, make all asegura que el flujo completo de build, test y package se ejecute siempre igual y sin errores humanos. El pilar de measurement se ve en el target benchmark, donde se mide el tiempo de ejecución y se deja registro en un archivo junto con el commit correspondiente, lo que actúa como un endpoint de salud porque muestra rendimiento. El pilar de sharing está presente en la documentación básica que nos da el comando help, que facilita a cualquier desarrollador el poder usar el proyecto rápidamente.

#### Visión cultural de DevOps y paso a DevSecOps:
Analiza colaboración para evitar silos, y evolución a DevSecOps (integrar seguridad como cabeceras TLS, escaneo dependencias en CI/CD).
Propón escenario retador: fallo certificado y mitigación cultural. Señala 3 controles de seguridad sin contenedores y su lugar en CI/CD.

Tip: Usa el archivo de Nginx y systemd para justificar tus controles.

#### Metodología 12-Factor App:
Elige 4 factores (incluye config por entorno, port binding, logs como flujos) y explica implementación en laboratorio.
Reto: manejar la ausencia de estado (statelessness) con servicios de apoyo (backing services).

Tip: No solo describas: muestra dónde el laboratorio falla o podría mejorar