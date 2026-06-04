---
title:    Introduction
sidebar_position: 1
---

# Introduction
**OpenSensorHub** (OSH) is a framework for building a sensor network that can run on different platforms and grow as needed.
The Java framework allows one to connect hardware devices to a common bus via a versatile driver API.

:::info
bus - a shared communication pathway\
driver - allows computer to hardware communication\
API - rules for communication between software
:::

Sensors can be connected through many hardware interfaces such as
[Wi-Fi](http://en.wikipedia.org/wiki/Wi-Fi), [USB](http://en.wikipedia.org/wiki/USB), [HTTP](http://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol), [Ethernet](http://en.wikipedia.org/wiki/Ethernet), [Bluetooth](http://en.wikipedia.org/wiki/Bluetooth), [ZigBee](http://en.wikipedia.org/wiki/ZigBee), [SPI](http://en.wikipedia.org/wiki/Serial_Peripheral_Interface_Bus), [I2C](http://en.wikipedia.org/wiki/I%C2%B2C), [RS232/422](http://en.wikipedia.org/wiki/RS-232), etc.
When a sensor is connected by a driver, it is automatically connected to the bus, and it is then simple to send commands and read data from it.
An intuitive user interface allows the user to configure the network to suit their needs and more advanced processing capabilities are available via plugins.

**OpenSensorHub** uses [OGC's web standards](https://www.ogc.org/standards/) to communicate with all connected sensors in the network and provide robust metadata (owner, location and orientation, calibration, etc.).
Through these standards, several **OpenSensorHub** instances can also communicate with each other to form larger networks.

Low level functions of SensorHub (send commands and read data from sensor) can run on [Java SE®](http://www.oracle.com/technetwork/java/javase), [Java ME®](http://www.oracle.com/us/technologies/java/embedded/micro-edition/overview/index.html), or [Android®](http://www.android.com/).
More advanced data processing capabilities are multithreaded and can benefit from a more powerful hardware platform (e.g. multiprocessor servers or even clusters).

The core of **OpenSensorHub** is Java, but other parts of this software are in the family of [**OSH Connect**](../osh-connect/introduction) libraries.

Please report all problems related to **OpenSensorHub** software including documentation errors via the [GitHub](https://github.com/Botts-Innovative-Research) Issue Tracker of the corresponding repository.