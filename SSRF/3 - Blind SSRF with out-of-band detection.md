![[Pasted image 20241112193600.png]]

La **detección "out of band" (fuera de banda)** es una técnica de seguridad que implica monitorear y analizar la actividad de una red o sistema desde un canal o entorno separado del flujo de datos principal. Este enfoque permite observar el tráfico o las actividades sin interferir directamente en la operación del sistema o sin introducir latencia en el flujo de datos principal.

### Características clave de la detección "out of band":

1. **Aislamiento del sistema principal**: La detección "out of band" trabaja fuera del canal de datos principal, lo que ayuda a evitar que un atacante detecte las herramientas de seguridad o monitoreo.

2. **Menor impacto en el rendimiento**: Al no procesar los datos directamente en el flujo de trabajo de la red, la detección "out of band" no genera latencia, lo cual es ideal para sistemas críticos en los que el rendimiento es crucial.

3. **Análisis profundo y diferido**: Como no está atada al tiempo real del sistema, la detección "out of band" permite realizar análisis más exhaustivos, como la búsqueda de patrones de ataque avanzados que puedan pasar desapercibidos en tiempo real.

4. **Uso en sistemas de respuesta a incidentes**: La detección "out of band" es común en sistemas de respuesta a incidentes o auditoría de seguridad, donde se requiere obtener información detallada de una manera menos obvia para los atacantes.

### Ejemplo de uso
Imagina que tienes un sistema de detección de intrusiones (IDS) que se ejecuta en un servidor dedicado para monitorear todo el tráfico de red. Este servidor no participa en el procesamiento de datos principal, sino que simplemente observa y analiza el tráfico desde un punto separado.

==Básicamente hay un sistema de monitorio por detrás guardando las urls del referrer y haciendo peticiones==

