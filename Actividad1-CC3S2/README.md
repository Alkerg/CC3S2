## DevOps vs. cascada tradicional
![devops-vs-cascada](./imagenes/devops-vs-cascada.png) 
**Devops** permite un desarrollo más rápido y reduce el riesgo al crear proyectos para la nube debido a la trazabilidad y la automatización tanto de las pruebas como del despliegue que permiten detectar errores a tiempo y corregirlas sin bloquear el trabajo de los demás. Mientras tanto, el **desarrollo en cascada** no permite solucionar problemas o hacer nuevas implementaciones de forma rápida, se debe tener claro que funcionalidades tendrá el proyecto desde el inicio.
Un contexto real donde el desarrollo en cascada sigue siendo razonable podría ser el desarrollo de un sistema de control de vuelo ya que se requiere cumplir con ciertas normas regulatorias y es vital que en la entrega final no se encuentre ningún fallo. En dicho caso, se debe planificar con mucha antelación todo el sistema y cada paso, aunque tarde bastante tiempo, debe completarse con éxito y pasar todas las pruebas necesarias.
Algunos **criterios verificables** podrías ser:

- Cumplir con las normas de certificación aeronáutica
- Pasar todas las pruebas de simulación

Los **trade-offs** de aplicar desarrollo en cascada en este contexto serían:

- Se garantiza la seguridad y el cumplimiento de las normas internacionales
- El proceso de desarrollo es muy lento y costoso


## Ciclo tradicional de dos pasos y silos
![silos-equipos](./imagenes/silos-equipos.png) 
### Limitaciones del ciclo "construcción -> operación"
- **Grandes lotes de cambios:** Al no haber integración continua, el software se construye en grandes bloques de código que no han sido probados minuciosamente y pueden contener errores difíciles de encontrar y solucionar.
- **Colas de defectos:** Los errores se encuentran hacia el final del desarrollo y resulta en un acumulado de defectos que pudieron detectarse antes.

### Antipatrones del desarrollo de software
- **"Throw over the wall":** Este antipatrón consiste en la falta de comunicación efectiva en un equipo de desarrollo. Esto incluye la falta de documentación en el código y la falta de contexto sobre los avances que se han realizado, asumiendo que los demás sabrán como hacer que todo funcione. Esta situación no solo retrasa el desarrollo del producto sino que impide la colaboración y aumenta el MTTR (tiempo promedio de reparación) ya que el equipo de operaciones no entiende como funciona el código de los desarrolladores, provocando que la resolución de errores en producción tome más tiempo.
- **Seguridad como auditoría tardía:** Este antipatrón consiste en comenzar a realizar pruebas y configuraciones de seguridad al final del desarrollo, en vez de hacerlo desde el inicio. Esto genera un alto costo de integración tardía ya que los errores de seguridad descubiertos tardíamente pueden implicar el rediseño de la infraestructura y alargar el tiempo de desarrollo.

