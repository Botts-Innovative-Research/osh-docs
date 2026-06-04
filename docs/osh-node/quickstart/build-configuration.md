---
title: Build Configuration
sidebar_position: 5
---

# Configuring an OSH Node

In this page we will show how to add new modules to a project with settings.gradle and build.gradle

The easiest way to modify the configurations in your project will be within an IDE.

Some later guides may include workflow & project setup in [IntelliJ IDEA](https://www.jetbrains.com/idea/) (free version available) but other IDEs can be used as well.

Once you have built the project, open it in your IDE.

## Including Modules in the Build

Within the project's root directory there will be a project wide `build.gradle` and `settings.gradle`.

:::info
The root directory in **OSH**node is `osh-node-dev-template [osh-node]`. It contains the folders `dist`, `include`, `processing`, `sensors`, etc.
:::

To include modules into your build from osh-addons or osh-core which are in the include folder or a directory built yourself, you must modify both the `build.gradle` & `settings.gradle`.

### settings.gradle
The `settings.gradle` is used to tell Gradle which modules are a part of the project and to configure the settings of the root project and the subprojects.

#### Adding Individual Projects
Open the `settings.gradle` in the root directory

At the top of the page you will see definitions which will be used as shorthand for adding the paths to specific directories in your project's build:

:::info
A directory is the path of folders to reach a file or folder.
:::
```gradle title="/osh-node-dev-template/settings.gradle"
rootProject.name = 'osh-node'       // Here you can rename the project 
def includeDir = "$rootDir/include"         
def sensorDir = "$includeDir/osh-addons/sensors"      
def persistenceDir = "$includeDir/osh-addons/persistence"
def processDir = "$includeDir/osh-addons/processing"
def serviceDir = "$includeDir/osh-addons/services"

def toolsDir = "$rootDir/tools"
```
The rootDir is not defined here, but it is the directory for the root folder/osh-node folder

Use these definitions by doing $defname to quickly reference the paths to the projects you need to implement.

An example of adding a subproject to our `settings.gradle`. This project will be available to your `build.gradle` and all subproject `build.gradle`s that are included in this build.
```gradle title="/osh-node-dev-template/settings.gradle"
include '[module-name]'  
project(':[module-name]').projectDir = "[Dir def]/[other folders] (optional)/[module-name]" as File
```
The module name is typically something like `sensorhub-driver-{name}` (for sensor drivers) or `sensorhub-process-{name}` (for processing modules)

the Directory path `[Dir def]/[other folders] (optional)/[module-name]` is made up of:
- A directory variable which are shown above
- subfolder(s) that contain the module
    - if there are multiple add a slash between each
- The module's name

####  Adding all submodules in a directory
Toward the bottom of the page you will see a block of code:
``` gradle title="/osh-node-dev-template/settings.gradle"
FileTree subprojects = fileTree("$rootDir/sensors").include('**/build.gradle')
subprojects.files.each { File f ->
  File projectFolder = f.parentFile
  if (projectFolder != rootDir) {
     String projectName = ':' + projectFolder.name
     include projectName
     project(projectName).projectDir = projectFolder
     }
  }
```

This code will automatically include all subprojects from the `/osh-node-dev-template/sensors`. However, we can also use this method for including all submodules in a subdirectory.
For example, using the `osh-addons` directory, we can include all sensors from `osh-addons` by referencing `"$sensorDir"` which includes modules from `/osh-node-dev-template/include/osh-addons/sensors`

If you build your project within that directory it will automatically add your project's build.gradle to the build.

#### Removing a Sensor from the Build
To ignore submodules, you can add an if-statement which will act as a filter to the code-block:

``` gradle title="/osh-node-dev-template/settings.gradle"
FileTree subprojects = fileTree("$rootDir/sensors").include('**/build.gradle')
subprojects.files.each { File f ->
   File projectFolder = f.parentFile
   // highlight-next-line
   if (!projectFolder.name.contains("template")) {
      if (projectFolder != rootDir) {
         String projectName = ':' + projectFolder.name
         include projectName
         project(projectName).projectDir = projectFolder
      }
      // highlight-next-line
   }
}
```

### build.gradle
The `build.gradle` file is the build script for a single project (either the root project or a subproject).

`build.gradle` contains the logic for building the project such as dependencies(we are about to add a dependency), plugins, and other build configurations.

#### Adding a Dependency

Inside `build.gradle` found in the root directory is the dependencies.
```gradle title="/osh-node-dev-template/build.gradle"
dependencies {
...
}
``` 
dependencies will be added by `implement project(:[project-name])` which will be inside the dependencies's braces.

A dependency will be ignored if it is commented out with double slashes `//`.

An example we will use in Sensor/Process Modules is `sensorhub-driver-fakeweather`:

Add into dependencies in build.gradle

```gradle title="/osh-node-dev-template/build.gradle"
implementation project(':sensorhub-driver-fakeweather')
``` 
Add to settings.gradle

```gradle title="/osh-node-dev-template/settings.gradle"
include 'sensorhub-driver-fakeweather'  
project(':sensorhub-driver-fakeweather').projectDir = "$sensorDir/simulated/sensorhub-driver-fakeweather" as File
```
:::note
notice when colons are and are not used before the module name.
:::

Upon adding these dependencies to your `build.gradle` and `settings.gradle`, your **OpenSensorHub** node will need to be run then `sensorhub-driver-fakeweather` will be installed.
`sensorhub-driver-fakeweather` will need to be added to be seen in the Admin UI which will be explained in [Sensor/Process Modules](https://docs.opensensorhub.org/docs/osh-node/user-docs/sensors-and-process-modules).