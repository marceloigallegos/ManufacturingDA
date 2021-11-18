# ManufacturingDA
Manufacturing Data Analytics: A basic statistics-based tool

## Detección de anomalías en sistemas de producción
A pesar de contar con equipos con una creciente calidad, estabilidad y precisión, ningún sistema mecánico/productivo es inmune a variaciones o alteraciones dentro de su rutina de operación. Hoy más que nunca, conceptos como Industria 4.0, Automatización y Digitalización refuerzan la necesidad por no sólo operar sistemas cada vez más máquino-dependientes, sino la consideración del monitoreo y control de los diferentes equipos/máquinas desde la concepción del sistema productivo.

FIGURA 1

Detectar las variaciones en un equipo, según los parámetros controlados por los sensores, guarda su importancia principalmente en:
- Identificar en específico el equipo con variaciones, a fin de aplicar mantenimiento o remover del proceso productivo.
- Identificar y relacionar la calidad de un determinado lote de productos con el equipo con variaciones.
- Investigar causas de raíz que generen alteraciones en el ciclo de confiabilidad del equipo.

## Localizar defectos a corregir dentro de un sistema productivo
Normalmente, la tarea más obvia (y relevante) corresponde en determinar qué equipo (dentro de un conjunto de equipos idénticos) sufre de variaciones o desviaciones operativas. Detectar variaciones para un equipo en particular puede seguir diferentes procedimientos estándar:
- Conteo de anomalías del equipo en funcionamiento. Si la frecuencia/densidad de las anomalías excede un límite arbitrario, entonces se levanta el requerimiento de mantención/reparación al equipo.
- Ajustar una distribución estadística (normal, gaussiana, etc.), de datos generados en un determinado espacio de tiempo. Luego, ya en operación, si hay cambios en la distribución, se levanta un requerimiento.
- Aplicar pruebas estadísticas cada turno/día/semana y analizar si hay cambio significativo.

Si el equipo se encuentra asociado con sistemas de monitoreo, control y/o sensores, entonces el uso de datos expande/robustece cada procedimiento.

## Data Analysis para apoyar la gestión de mantenimiento: A basic Framework

### Dependencies
- Numpy
- Pandas
- Matplotlib

El Framework que se presenta considera que existe dentro del sistema productivo un equipo de referencia o modelo (Golden Standard Machine). Además, consideraremos que en el conjunto de equipos a analizar (todos ellos idénticos), se han realizado instalación de sistemas de control y sensores para medir parámetros/variables de interés y generar datos, y que dichos datos son confiables. Esto es, a partir de los datos generados, pueden aplicarse métodos y pruebas estadísticas.

Me quiero detener brevemente en la consideración del sensoramiento o instalación de dispositivos de monitoreo. Sugiero (sin tecnicismos o formalidades) que se deben considerar ciertos aspectos antes de incursionar en estas aplicaciones:

- Si no se mide, no existe: Esta es la premisa que da lugar a considerar instalar un sistema de monitoreo, sensores o cualquier dispositivo que recolecte datos. La idea es precisamente medir algo de interés para poder considerarlo dentro de cualquier análisis.
- Si no sirve, no se mide: Millones de datos o registros no significan nada si sólo se convierten en consumo de espacio en el disco duro. Sólo enfocarse en medir lo que realmente interesa ahora, el resto puede quedar para el futuro.
- Si no es confiable, no sirve: Si no se puede contar con un grado de confianza significativo en el sistema que levanta la información, entonces cualquier análisis generará conclusiones tan débiles como los datos mismos.

Consideremos un sistema productivo con cinco Equipos idénticos, además de un Equipo Modelo (golden machine). A su vez, todos ellos tienen instalados 5 sensores que registran 5 parámetros de diferente naturaleza.

100 datos por máquina, con 10 sensores.

Equipo-Modelo:
```
equipo_modelo = pd.DataFrame(np.random.normal(size=(100,10)))
```

Equipos del Sistema:
```
equipos = {'equipo_'+str(i):[] for i in range(1,6)}
for i in range(1,6):
    loc = np.random.uniform(0,1)
    scale = np.random.uniform(0,1)
    df = equipo_modelo+pd.DataFrame(np.random.normal(loc=loc,scale=scale,size=(100,10)))
    equipos['equipo_'+str(i)] = df
```

Usando Pandas, podemos implementar un método that calculates the Pearson Correlation score between each Equipo y el Equipo Modelo:
```
for equipo in equipos:
    print(f"Nivel Correlacion de {equipo} respecto a equipoModelo:", round(equipo_modelo.corrwith(equipos[equipo],axis=1).sum(),2))
```

Como resultado, obtendremos algo como esto:
```
Correlacion de equipo_1 respecto a equipoModelo: 88.18
Correlacion de equipo_2 respecto a equipoModelo: 72.3
Correlacion de equipo_3 respecto a equipoModelo: 77.11
Correlacion de equipo_4 respecto a equipoModelo: 99.89
Correlacion de equipo_5 respecto a equipoModelo: 67.31
Correlacion de equipo_6 respecto a equipoModelo: 94.82
Correlacion de equipo_7 respecto a equipoModelo: 80.23
Correlacion de equipo_8 respecto a equipoModelo: 83.06
Correlacion de equipo_9 respecto a equipoModelo: 98.66
Correlacion de equipo_10 respecto a equipoModelo: 99.94
```

De una inspección rápida, podemos observar que los Equipos 2 y 3 fueron los menos correlated with the golden machine, y los equipos 4, 9 y 10 are the most correlated. Lógicamente, sólo se pueden establecer comparaciones coherentes entre los Equipos cuando son comparados los datos del mismo sensor. A modo de ejemplo, nótese la correlación entre los datos asociados al Sensor-1 del Equipo-Modelo y los del Sensor-1 del Equipo-4 (Figura 2). De forma opuesta, observe la gráfica para la correlación entre los datos asociados al Sensor-1 del Equipo-Modelo y al Sensor-2 del Equipo-4 (Figura 3).

FIGURA 2

FIGURA 3

A partir de aquí podemos empezar a crear un análisis más entretenido. Veamos el Equipo 1, que pareciera guardar una correlación moderada/débil con el Equipo Modelo. Podemos empezar generando un pequeño dashboard para estudiar visualmente la correlación en algunos Sensores en específico.

FIGURA

El Equipo 10 tiene el mayor nivel de correlación con el Equipo Modelo ¿Es esto apreciable en detalle para cada Sensor?

FIGURA

¿Qué pasa para el Equipo con el menor nivel de correlación (Equipo 5)?

FIGURA

Al finalizar nuestro proceso, podemos ahora seleccionar aquellos equipos con desviaciones significativas (Equipos 2, 3 y 5) y removerlos del sistema en operación para ejecutar ajustes/reparaciones. Si revisamos un poco de ejemplos básicos sobre el coeficiente de Pearson, podemos incluso inclinarlos por fijar un límite de referencia para el control de los equipos (digamos al 0.8).

FIGURA

Este proceso controlar y localizar equipos con defectos incrementa su potencial, evidentemente, con un aumento en el número de muestras y sensores sobre el conjunto de equipos del sistema. Más aún, un aumento en la complejidad del sistema (i.e. más equipos) no afecta el procedimiento y sus resultados. En un horizonte de planificación de varios periodos de tiempo, podemos ver cambios radicales en qué equipos y en qué número presentan anomalías, pero este sencillo procedimiento es capaz de identificar esos equipos utilizando el mismo enfoque analítico.
