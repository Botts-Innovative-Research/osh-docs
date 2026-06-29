---
title: Databases
sidebar_position: 2
---

# Databases

This page goes over how to use and create databases and service modules in **OpenSensorHub**.

**OpenSensorHub** databases are used to store data including after the node ends.

**OpenSensorHub** comes pre-packaged with three types of databases:

| Database Type          | Database Use                   |
|------------------------|--------------------------------|
| Federated Database     | Viewing data from all systems  |
| Basic H2 Database      | Getting data from data files   |
| System Driver Database | Saving sensor and process data |

## Federated Database

The federated database will always appear at the top of the *Databases* tab, and contains data from all running Systems on the **OpenSensorHub** node.

If a *Sensor Driver* or *Process Module* do not have an associated database, then only the latest observations produced by these modules will be shown in the federated database.

After clicking a system, below the table it shows the data in it and Sensor Location (if applicable).

![federated.png](..%2F..%2Fassets%2Fosh%2Fadminui%2Fdatabases%2Ffederated.png)

## Basic H2 Database

The H2 database module is the most basic module for interfacing between a database file and **OpenSensorHub**.

### Configuration

Enter the storage path to an existing .dat file, or choose where a new one should be created.

If you only put the file name it will default to the folder `osh-node-0.0.0`.

The other default configuration options will be sufficient for most use cases.

![h2db.png](..%2F..%2Fassets%2Fosh%2Fadminui%2Fdatabases%2Fh2db.png)

### Usage

The Basic H2 Database can be used to load **OpenSensorHub**-compatible .dat files from another node.
It can also be used for storing *Service Module* data which is explained in [Service Modules](https://docs.opensensorhub.org/docs/osh-node/user-docs/service-modules)

## System Driver Database

*System Driver Databases* are used as a means of capturing data from *Sensor Drivers* or *Process Modules* for long-term storage.

*System Driver Databases* are wrapped around H2 databases to give more functionality.

### Configuration

Below, you will see the default configuration of a *System Driver Database*.
There are 3 important parts of the *System Driver Database* configuration.

| Configuration          | Description                                                                  |
|------------------------|------------------------------------------------------------------------------|
| Database Config        | Adding and configuring the wrapped H2 database.                              |
| System UIDs            | A list of system UIDs or UID patterns that the data base will get data from. |
| Automatic Purge Policy | A list of policies used to regularly purge (delete) data from the database.  |


![systemdriverdb.png](..%2F..%2Fassets%2Fosh%2Fadminui%2Fdatabases%2Fsystemdriverdb.png)

### Database Config

1) Open **Database Config**
2) Click **Add** in the top left
3) Select **H2 Historical Obs Database**
4) Click **OK**.
5) Configure the same way as [Basic H2 Database](https://docs.opensensorhub.org/docs/osh-node/user-docs/databases#basic-h2-database).

![systemdbconfig.png](..%2F..%2Fassets%2Fosh%2Fadminui%2Fdatabases%2Fsystemdbconfig.png)

### System UIDs

a UID (Unique Identifier) is how systems tell each other apart.
In **OSH** a UID for a fakeweather driver with 001 as the *Serial Number* would be `urn:osh:sensor:simweather:001`

In General there is *System UIDs* which is where UIDs are selected for the database.\
To add a module press the plus sign then select the module.\
The module will show up in the list in *System UIDs*.\
To remove a module select them in the list then press the x sign.

When the *System Driver Database* has been started you will see those systems appear below the *System Driver Database* configuration.

There should be at least one module in the table below *Database Content*.
If not try pressing *Apply Changes* again.

Select a module then below the table will show the data stored and will show information about the sensor in Sensor Location (if applicable).
Keep in mind the data will be stored oldest first and newest last.

If you want to add a *Sensor System* and its subsystems to a *System Driver Database*, you only need to put the UID of the parent system.
This will typically look like `urn:osh:system:<parent UID>`

![systemuids.png](..%2F..%2Fassets%2Fosh%2Fadminui%2Fdatabases%2Fsystemuids.png)

:::tip
You may use a UID pattern to specify all UIDs with a certain prefix.
For example, `urn:osh:sensor:*` will add all *Sensor Drivers* to the *System Driver Database*.
An asterisk basically means "All". 
:::

### Automatic Purge Policy

Automatic purge policies will instruct the *System Driver Database* module to purge specified systems from the database routinely.

To add a Purge Policy click the plus sign.

Here, we can specify a few things
- Which systems are purged via their UIDs or a UID pattern. This is `*` by default, which means **ALL** systems will be purged.
- How often the data is checked for being too old (in seconds).
- The maximum age of data to be kept in the database (in seconds).

![purgepolicy.png](..%2F..%2Fassets%2Fosh%2Fadminui%2Fdatabases%2Fpurgepolicy.png)

:::tip
an hour is 3600 seconds\
a day is 86,400 seconds\
a week is 604,800 seconds\
30 days is 2,592,000 seconds
:::