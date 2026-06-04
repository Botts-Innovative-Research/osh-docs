---
title: Build
sidebar_position: 3
---

# How to Build an OSH Node

This page guides you through building an OSH Node from source using the command line terminal.

## Getting the Code
The `git` command is used to download the code from the [GitHub](https://github.com/Botts-Innovative-Research) repositories hosting **OpenSensorHub**.

you can download the code for an OSH Node Development Template using the following command:

```git 
git clone --recursive https://github.com/opensensorhub/osh-node-dev-template.git
```

:::note
`clone` is used so that it also copies the metadata.
`--recursive` is used because the repository contains submodules.
:::

## Building from the Command Line
You can build the code using Gradle on the command line.

Change into the directory where you cloned the repository using the command:

```sh
cd osh-node-dev-template
```
:::warning
If using an old version of Gradle, it would need to be updated if using Java 21+.
The Gradle wrapper version can be changed in `/osh-node-dev-template/gradle/wrapper/gradle-wrapper.properties`.
```gradle title="/osh-node-dev-template/gradle/wrapper/gradle-wrapper.properties"
#Wed May 06 18:14:44 CDT 2020
// We can change from 7.3.3 (Java 17) to the latest gradle version
// highlight-next-line
distributionUrl=https\://services.gradle.org/distributions/gradle-7.3.3-bin.zip
distri...
```
:::
To build, run the following:
```sh
./gradlew build -x test
```
:::note
`./gradlew build` compiles and runs checks.
`-x test` excludes unit tests from the build process
:::


The OSH node will be built in the folder `/osh-node-dev-template/build/distributions`