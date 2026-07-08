---
title: Build
sidebar_position: 3
---


# How to Build an OSH Node

This page guides you through how to build your OSH Node in your command line. Starting from your requirements and ending with a zip file housing the architecture to build your own customizable OSH Node.

## Getting the Code
The `git` command is used to download the code from the GitHub repositories hosting **OpenSensorHub**. 

You can download the code for an OSH Node Development Template, using the HTTPS with the following command:

```git 
git clone --recursive https://github.com/opensensorhub/osh-node-dev-template.git
```

:::note
The `--recursive` flag is required because the repository contains submodules.
:::

## Building from the Command Line
You can build the Node, using Gradle, on the command line.

Change into the directory where you cloned the repository:

```sh
cd osh-node-dev-template
```
:::warning
In order to build using Java 21+, you must use an up-to-date Gradle version. 
The Gradle wrapper version can be changed in `/osh-node-dev-template/gradle/wrapper/gradle-wrapper.properties`.
```gradle title="/osh-node-dev-template/gradle/wrapper/gradle-wrapper.properties"
#Wed May 06 18:14:44 CDT 2020
// We can change from 7.3.3 (Java 17) to the latest gradle version
// highlight-next-line
distributionUrl=https\://services.gradle.org/distributions/gradle-7.3.3-bin.zip
distri...
```
:::
To build your local OSH node, enter the following:   
```sh
./gradlew build -x test
```
:::note
`-x test` excludes unit tests from the build process 
:::

This will result in a build of your OSH node, formed in a zip file. Later steps will help you unzip the file and begin running your OSH node

Finally, locate the file, by opening build then distributions to find your file. 

Path:`/osh-node-dev-template/build/distributions/osh-node-..*.zip`

Titled: `osh-node-..*.zip`