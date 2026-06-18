---
title: Sensor/Process Modules
sidebar_position: 1
---

# Sensor and Process Modules

This section will go over how to deploy and configure Sensors, Sensor Systems, and Processors in the [admin UI](http://localhost:8181/sensorhub/admin).

Before a module can be added to the admin UI it must first be implemented as a dependency in `build.gradle` and included in `settings.gradle` which is explained in [build configuration](https://docs.opensensorhub.org/docs/osh-node/quickstart/build-configuration).

## Sensors

**OpenSensorHub** uses *Sensor Drivers* to act as a bridge between a physical sensor (or non-physical stream of data) and **OpenSensorHub**'s inner workings.

*Sensor Drivers* allow us to do the following
- Store data via **OpenSensorHub**’s database capable of long term storage

- Show data via [OGC API - Connected Systems](https://docs.opensensorhub.org/docs/osh-connect/connected-systems)
  - This acts as an easy-to-use interface to visualize sensor data

- Connect data to be used in SensorML process chains
  - SensorML process chains (aka processes) do calculations on data

- Send data to other **OSH**Nodes

- Group sensors via Sensor Systems

### Selecting a Sensor Driver

The easiest way to deploy *Sensor Drivers* is to use the Admin UI.
The Admin UI is available by default on all **OpenSensorHub** nodes at
```
{protocol}://{host}:{port}/sensorhub/admin
```
On a local **OpenSensorHub** node, this will typically be
```http
http://localhost:8181/sensorhub/admin         
```

:::info
**OpenSensorHub** nodes are password protected.

The default Admin UI credentials:
**username**: admin
**password**: admin
:::

![OSH web-based Admin UI](../../assets/osh/adminui/adminui.PNG)

On the Admin UI, if you right-click the space under the *Sensors* tab, and select *Add New Module*, you can add a new *Sensor Driver* from a list of all *Sensor Drivers* that have been included in your **OpenSensorHub** build.

In this example, I'll select and show the outputs of a simulated weather sensor.

![Selecting a Simulated Weather Sensor Driver](../../assets/osh/adminui/sensors/sensorselection.png)
:::tip
Simulated weather should already be added assuming you did the example in [*Build Configuration*](../quickstart/build-configuration).
If not the Directory is `sensors/simulated/sensorhub-driver-fakeweather`.
:::

### Configuring Sensor Drivers
After you've selected a *Sensor Driver*, you'll see a panel to configure this driver.
The driver will also appear under the *Sensors* tab with the `LOADED` state.

In the panel to the right you can edit information about your driver such as a Module Name and Description found in General as well as Latitude, Longitude, and Altitude found in Location.
The unique ID and Serial Number found in general are required information and can't be the same as any other modules.

![Sensor configuration](..%2F..%2Fassets%2Fosh%2Fadminui%2Fsensors%2Fsensorconfig.png)

Almost all *Sensor Drivers* will include important configuration options for establishing a connection to the sensor, configuring parameters, describing the sensor's location, etc.
Make sure that you provide accurate and up-to-date configuration to ensure a connection is made to your sensor.

Once you are happy with the *Sensor Driver*'s configuration, select *Apply Changes* at the top-right of the panel, and your *Sensor Driver* will initialize with the provided configuration.
Changing it's state to `Initialized`.
It will not start reading from the sensor yet.

![Sensor initialization with new configuration](..%2F..%2Fassets%2Fosh%2Fadminui%2Fsensors%2Fsensorinit.png)

### Module Actions

For any **OpenSensorHub** module, you can control its state in the Admin UI by right-clicking the module on the sidebar, and selecting an action.

![sensoractions.png](..%2F..%2Fassets%2Fosh%2Fadminui%2Fsensors%2Fsensoractions.png)

| Action           | Usage/Purpose                                                                                                                                                                   |
|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| *Start*          | Starts the selected module. For *Sensor Drivers*, this will start the data stream of sensor outputs from the sensor to OSH's internal event bus.                                |
| *Stop*           | Stops the selected module. For *Sensor Drivers*, this will close the connection to the sensor, and stop the sensor from publishing data to OSH.                                 |
| *Force Init*     | Re-initializes the module, setting it to `INITIALIZED` state. Typically, you use this to clear up issues that prevented the module from starting under its previous conditions. |
| *Remove Module*  | Completely removes the module from **OpenSensorHub**. Persisted data will not be removed when performing this action. Please see [*Databases*](databases.md).                   |
| *Add New Module* | Allows you to add a new module of the type for whichever sidebar tab you are in. (Sensors, Databases, Processes, etc.)                                                          |

### Sensor Driver's Outputs

To check that your *Sensor Driver* is publishing outputs, you can check under the *Outputs* section that appears under the *Sensor Driver's* configuration, once initialized.

![sensoroutputs.gif](..%2F..%2Fassets%2Fosh%2Fadminui%2Fsensors%2Fsensoroutputs.gif)

You may also check the *Federated Database* under the *Databases* tab, to check that your *Sensor Driver* is publishing its latest outputs to the **OpenSensorHub** State Database.

![statedboutputs.gif](..%2F..%2Fassets%2Fosh%2Fadminui%2Fsensors%2Fstatedboutputs.gif)

### Sensor Driver's Commands

Some *Sensor Drivers* will have *Control Streams* in addition to their output *Data Streams*. These *Control Streams* act as an interface for sending commands to a sensor or actuator.
Like moving a camera or changing settings.

If your *Sensor Driver* has *Control Streams*, you will see a user interface for sending commands under a *Sensor Driver*'s *Outputs* section.

### Sensor Systems

**OpenSensorHub** has a built-in *Sensor System* that acts as a parent to a group of *Sensor Drivers* and/or *Processing Modules*.
This can be useful for deployments where multiple *Sensor Drivers* and/or *Processing Modules* must be linked to a common parent.

![selectsensorsystem.png](..%2F..%2Fassets%2Fosh%2Fadminui%2Fsensors%2Fselectsensorsystem.png)

We will walk through setting up a basic *Sensor System*.

![sensorsystemconfig.png](..%2F..%2Fassets%2Fosh%2Fadminui%2Fsensors%2Fsensorsystemconfig.png)

A *Sensor System* is configured like all other **OpenSensorHub** modules.
While it is necessary to provide a unique ID, you may optionally provide additional descriptive information such as the *Sensor System*'s location and orientation.

Once configured, you may add *Sensor Drivers* and *Process Modules* under a *Sensor System* by right-clicking the *Sensor System* and selecting *Add Submodule*.

![addsubmodule.png](..%2F..%2Fassets%2Fosh%2Fadminui%2Fsensors%2Faddsubmodule.png)

A *Sensor System*'s children can be configured as if they were standalone modules. Below, you can see a *Sensor System* with 2 simulated weather *Sensor Drivers*.

:::info
Submodules may not be started unless the parent *Sensor System* is started. Also, if submodules have `autoStart` enabled, then they will start once the parent *Sensor System* is started.
:::

![systemwithchildren.png](..%2F..%2Fassets%2Fosh%2Fadminui%2Fsensors%2Fsystemwithchildren.png)

## Processing

SensorML Stream Processing can be used to do simple or complex calculations and automation with provided data on the fly.

The [Developer Documentation](https://docs.opensensorhub.org/docs/category/developer-documentation) will go over how to develop SensorML Processes.

### Gradle

In our example we will use `sensorhub-process-fakeweather` which converts the values provided by the fakewather driver into different measurement types.
Ex: converting Celsius to Fahrenheit.
`sensorhub-process-fakeweather` is located at:

`/osh-node/include/osh-addons/processing/sensorhub-process-fakeweather`

If you are following along with the example you will have to add it in build.gradle and settings.gradle.

If you go to `sensorhub-process-fakeweather`'s build.gradle which is in its folder.
`sensorhub-process-helpers` is listed as a dependency so you need to add that to the root project's gradles as well.
`sensorhub-process-helpers` is located in `osh-addons/processing` like `sensorhub-process-fakeweather`.

After changing build.gradle and settings.gradle rebuild the OSHNode.

### Generate Process Description

The *Process Description* tells the admin UI everything about how the process works from inputs to outputs and everything in between.

To get the process description of fakeweather navigate to:

`include/osh-addons-processing/sensorhub-process-fakeweather/src/test/java/ProcessDescriptionGenerator`

:::note
The ProcessDescriptionGenerator is in the same location of every process.
:::

The process knows what sensor to get its data from based on the sensor's *Serial Number*.
This file also determines what *Serial Number* a process looks for.
The default is 001, but it can be changed at the end of `.addDataSource()` 

```
return processHelper.createProcessChain()
        .name("Process Chain")
        .uid("urn:osh:process:weather")
        .description("Example process chain that converts units from the Simulated Weather Sensor")
        .addDataSource("source0", "urn:osh:sensor:simweather:001") // this is where the sensor's serial number can replace the default 001
        ...
```

Near the bottom of the code is `public void generate DescJSON()` and `public void generate DescXML()` and to the left of the methods should be a green start button if you are using IntelliJ.

Press the green start button of either one, the choice only determines what type of file you will have to save it as later.

Once ran the terminal should have some `> Task : ...` (which should not be copied) after all the tasks there will be either:

JSON - `{"type":"AggregateProcess"... "destination":"outputs/weather"}]}` as one long line

XML - `<sml:AggregateProcess... </sml:AggregateProcess>` across many lines

After the *Process Description* will be `ProcessDescriptionGenerator > generateDec...` do not copy this or anything after this

Paste the *Process Description* into Notepad, TextEdit, Gedit, etc. and go to file then save as then name it (whatever name you want).json or (whatever name you want).xml and save it into:

`osh-node/build/distributions/osh-node-0.0.0/osh-node-0.0.0`

### Deploy Process

Deploy the OSHNode and open the Admin UI.
Add New Module in Processing and select SensorML Stream Process.

![processmodule.png](..%2F..%2Fassets%2Fosh%2Fadminui%2Fprocessing%2Fprocessmodule.png)

type the name of the file including the .json/.xml into the SensorML File box.

Ensure a Simulated Weather Sensor with the correct Serial Number is running then run the SensorML Stream Process.
You should see the data from the sensor is being converted to different units.

![processoutputs.png](..%2F..%2Fassets%2Fosh%2Fadminui%2Fprocessing%2Fprocessoutputs.png)