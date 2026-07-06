---
title: Deploying
sidebar_position: 4
---

# Deploying an OSH Node

This page guides you through how to run your OSH Node, locally on your computer. Starting from your OSH Node zip file and ending with your Node running on your browser.  

## Basic Deployment

After building the node, you can navigate to the newly generated ``` /osh-node-dev-template/build/distributions``` directory.

You will see the `osh-node-..*.zip`

To unzip the node, use the command `unzip osh-node-..*.zip`, when you are in the same folder that houses your zip file. 

To launch the script, in your extracted folder, execute the launch script for your corresponding OS by either double-clicking or running the script in the command line:
:::note

**Windows**: 

``` cmd 
./launch.bat
```
**Linux / MacOS (Shell)**:
``` cmd
 ./launch.sh
```
:::

To test that the OSH server is running, you can visit [`http://localhost:8181/sensorhub/test`](http://localhost:8181/sensorhub/test), or by visiting the Admin UI.

To use your OSH Node, open your web browser and navigate to [`http://localhost:8181/sensorhub/admin`](http://localhost:8181/sensorhub/admin)

:::info
The default administrative credentials are:

**username**: admin

**password**: admin
:::

Below is an example of what you should see in the **OpenSensorHub** Admin UI. 

![OSH Admin User Interface](../../assets/osh/adminui/adminui.PNG)


### Default OSH Configuration

Within your OSH Node folder, there is a `config.json` file containing a default configuration of **OpenSensorHub**. Within this configuration, only default users and service modules are configured. 

Customizable settings will be covered in other sections. With using/deploying the different modules under *User Documentation* and API reference available in the *Developer Documentation*.

## Docker Deployment

TBD