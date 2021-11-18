# Manufacturing Data Analysis to monitor a productive system
In this small project a Framework based on Data Analytics is presented to support the control/repair process of machines in a productive system.

## Detection of anomalies in production systems
Despite having equipment with increasing quality, stability and precision, no equipment/machine is immune to variations or alterations within its operating routine. Today more than ever, concepts such as Industry 4.0, Automation and Digitization reinforce the need not only to operate increasingly machine-dependent systems, but also to consider the monitoring and control of the different equipment / machines from the conception of the production system.

![alt text](https://github.com/marceloigallegos/ManufacturingDA/blob/main/mdImages/Figura1.png)

Detecting variations in a piece of equipment, according to the parameters controlled by the sensors, keeps its importance mainly in:
- Identify specifically the equipment with variations, in order to apply maintenance or remove it from the production process.
- Identify and relate the quality of a certain batch of products with the equipment with variations.
- Investigate root causes that generate alterations in the reliability cycle of the equipment.

## Locate defects to correct within a production system
Typically, the most obvious (and relevant) task is to determine which machine (within a set of identical machine) suffers from operational variations or deviations. Detecting variations for a particular machine can follow different standard procedures:
- Count of anomalies of the machines in operation. If the frequency/density of the anomalies exceeds an arbitrary limit, then the maintenance/repair requirement to the machine is lifted.
- Fit a statistical distribution (normal, Gaussian, etc.) to data generated in a certain period of time. Then, already in operation, if there are changes in the distribution, a requirement is raised.
- Apply statistical tests every shift/day/week and analyze if there is significant change.

If the machine is associated with monitoring, control and/or sensor systems, then the use of data expands/strengthens each procedure.

## Data Analysis to support maintenance management: A basic Framework

### Dependencies
- Numpy
- Pandas
- Matplotlib

The Framework that is presented considers that a reference or model machine (Golden Standard Machine) exists within the production system. In addition, we will consider that in the set of machines to be analyzed (all of them identical), control systems and sensors have been installed to measure parameters/variables of interest and generate data, and that data is reliable. That is, from the data generated, statistical methods and tests can be applied with an aceptable level of confidence.

I want to pause briefly on the consideration of sensing or installing monitoring sensors/devices. I suggest (without technicalities or formalities) that certain aspects should be considered before venturing into these applications:

- **If it is not measured, it does not exist**: This is the premise that gives rise to considering installing a monitoring system, sensors or any device that collects data. The idea is precisely to measure something of interest to be able to consider it within any analysis.
- **If it does not work, it is not measured**: Millions of data or records do not mean anything if they only become consumption of space on the hard disk. Just focus on measuring what really matters now, the rest can be left for the future.
- **If it is not reliable, it does not work**: If you cannot have a significant degree of confidence in the system that collects the information, then any analysis will generate conclusions as weak as the data itself.

Let us consider a production system with ten identical machines, in addition to a Model Machine. In turn, all of them have installed 10 sensors that record ten parameters of different nature. If we consider a time horizon where 100 data has been recorded per machine, we have:

Model Machine
```
equipo_modelo = pd.DataFrame(np.random.normal(size=(100,10)))
```

System Machines:
```
equipos = {'equipo_'+str(i):[] for i in range(1,11)}
for i in range(1,11):
    loc = np.random.uniform(0,1)
    scale = np.random.uniform(0,1)
    df = equipo_modelo+pd.DataFrame(np.random.normal(loc=loc,scale=scale,size=(100,10)))
    equipos['equipo_'+str(i)] = df
```

Using Pandas, we can implement a method that calculates the Pearson Correlation score between each Machine and the Model Machine:
```
for equipo in equipos:
    print(f"Nivel Correlacion de {equipo} respecto a equipoModelo:", round(equipo_modelo.corrwith(equipos[equipo],axis=1).sum(),2))
```

As a result, we will get something like this:
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

From a quick inspection, we can see that Machines 2 and 3 were the least correlated with the Model Machine; and Machines 4, 9, and 10 are the most correlated. Logically, coherent comparisons can only be established between the Machines when data from the same sensor are compared. As an example, note the correlation between the data associated with Sensor-1 of Model-Machine and those of Sensor-1 of Machine-4 (**Figure 2**). Conversely, observe the graph for the correlation between the data associated with Sensor-1 of Model-Machine and Sensor-2 of Machine-4 (**Figure 3**).

<p align="center">
  <b>Figure 2</b><br>
  <br><br>
  <img src="https://github.com/marceloigallegos/ManufacturingDA/blob/main/mdImages/Figura2.png">
</p>

<p align="center">
  <b>Figure 3</b><br>
  <br><br>
  <img src="https://github.com/marceloigallegos/ManufacturingDA/blob/main/mdImages/Figura3.png">
</p>

From here we can start creating a more entertaining analysis. Let's look at Machine 1, which seems to have a moderate/weak correlation with the Model-Machine. We can start by generating a small dashboard to visually study the correlation in some specific Sensors.

![alt text](https://github.com/marceloigallegos/ManufacturingDA/blob/main/mdImages/Figura4.png)

Machine 10 has the highest level of correlation with Model-Machine. Is this appreciable in detail for each Sensor?

![alt text](https://github.com/marceloigallegos/ManufacturingDA/blob/main/mdImages/Figura5.png)

What about the Machine with the lowest correlation level (Machine 5)?

![alt text](https://github.com/marceloigallegos/ManufacturingDA/blob/main/mdImages/Figura6.png)

At the end of our process, we can now select those machines with significant deviations (Machines 2, 3 and 5) and remove them from the operating system to carry out adjustments/repairs. If we review some basic examples on the Pearson coefficient, we can even incline them by setting a reference limit for the control of the equipment (say at 0.8).

<p align="center">
  <img width="700" height="300" src="https://github.com/marceloigallegos/ManufacturingDA/blob/main/mdImages/Figura7.png">
</p>

This process to control and locate machines with defects increases its potential, obviously, with an increase in the number of samples and sensors on the set of equipment in the system. Furthermore, an increase in the complexity of the system (i.e. more machines) does not affect the procedure and its results. In a planning horizon of several time periods, we can see radical changes in which machines and in which number have anomalies, but this simple procedure is able to identify those machines using the same analytical approach.


## Repositorio

This repository includes:
- The example case analyzed for this project (X).
- An additional case with 50 machines and 20 sensors (X).
- An additional case of 20 machines and 10 sensors, with integration to data recorded in an external source (.xls file).
