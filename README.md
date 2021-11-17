# ManufacturingDA
Manufacturing Data Analytics: A basic statistics-based tool

## Detección de anomalías en sistemas de producción
A pesar de contar con equipos con una creciente calidad, estabilidad y precisión, ningún sistema mecánico/productivo es inmune a variaciones o alteraciones dentro de su rutina de operación. Hoy más que nunca, conceptos como Industria 4.0, Automatización y Digitalización refuerzan la necesidad por no sólo operar sistemas cada vez más máquino-dependientes, sino la consideración del monitoreo y control de los diferentes equipos/máquinas desde la concepción del sistema productivo.

-Imagenes

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
El Framework que se presenta considera que existe dentro del sistema productivo un equipo de referencia o modelo (Golden Standard Machine). Además, consideraremos que en el conjunto de equipos a analizar (todos ellos idénticos), se han realizado instalación de sistemas de control y sensores para medir parámetros/variables de interés y generar datos, y que dichos datos son confiables. Esto es, a partir de los datos generados, pueden aplicarse métodos y pruebas estadísticas.

Me quiero detener brevemente en la consideración del sensoramiento o instalación de dispositivos de monitoreo. Sugiero (sin tecnicismos o formalidades) que se deben considerar ciertos aspectos antes de incursionar en estas aplicaciones:

- Si no se mide, no existe: Esta es la premisa que da lugar a considerar instalar un sistema de monitoreo, sensores o cualquier dispositivo que recolecte datos. La idea es precisamente medir algo de interés para poder considerarlo dentro de cualquier análisis.
- Si no sirve, no se mide: Millones de datos o registros no significan nada si sólo se convierten en consumo de espacio en el disco duro. Sólo enfocarse en medir lo que realmente interesa ahora, el resto puede quedar para el futuro.
- Si no es confiable, no sirve: Si no se puede contar con un grado de confianza significativo en el sistema que levanta la información, entonces cualquier análisis generará conclusiones tan débiles como los datos mismos.

Consideremos un sistema productivo con cinco Equipos idénticos, además de un Equipo Modelo (golden machine). A su vez, todos ellos tienen instalados 5 sensores que registran 5 parámetros de diferente naturaleza. Esto es, sólo se pueden establecer comparaciones coherentes entre los Equipos cuando son comparados los datos del mismo sensor.

100 datos por máquina, con 5 sensores.

Equipo-Modelo:
```
equipo_modelo = pd.DataFrame(np.random.normal(size=(100,5)))
```

Equipos del Sistema:
```
machines = {'machine'+str(i):[] for i in range(1,11)}
for i in range(1,11):
    loc = np.random.uniform(0,2)
    scale = np.random.uniform(0,2)
    df1 = machine_golden+pd.DataFrame(np.random.normal(loc=loc,scale=scale,size=(200,10)))
    machines['machine'+str(i)] = df1
```

A modo de ejemplo, nótese la correlación entre los datos asociados al Sensor-3 del Equipo-Modelo y del Equipo-2 (Figura X). De forma opuesta, observe la correlación entre los datos asociados al Sensor-2 del Equipo-Modelo y al Sensor-1 del Equipo-3 (Figura X).

-Figuras X - X




### Dependencies
- Numpy
- Pandas
- Matplotlib
