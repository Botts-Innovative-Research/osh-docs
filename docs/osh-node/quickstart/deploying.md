---
title: Deploying
sidebar_position: 4
---

# Deploying an OSH Node

After building the node, follow these steps to run it
1) Starting in `osh-node-dev-template`, open the `build` folder, then the `distributions` folder.
2) Unzip the `osh-node-0.0.0` folder.

:::note
The 0.0.0 indicates the version.

As you work on **OSH** you can optionally increase the version.
This will be in `build.gradle` which we will go over later.
:::
3) Within the unzipped `osh-node-0.0.0` double-click the launch script or use a command below.

:::note
**Windows**:
``` cmd 
./launch.bat
```
**Linux / macOS (Shell)**:
``` cmd
 ./launch.sh
```
:::
4) Open the OSH server using the http://localhost:8181/sensorhub/admin address

:::info
The default administrative credentials are:\
**username**: admin\
**password**: admin

to change the credentials go to [General Node Configuration #Security Config](https://docs.opensensorhub.org/docs/osh-node/user-docs/node-configuration#security-config)
:::

:::warning
closing the launch window that shows up will end the **OSH** server
:::

Below is the **OpenSensorHub** Admin UI.

We will cover using the different modules under *User Documentation*, with API details available in the *Developer Documentation*.

![OSH Admin User Interface](../../assets/osh/adminui/adminui.PNG)