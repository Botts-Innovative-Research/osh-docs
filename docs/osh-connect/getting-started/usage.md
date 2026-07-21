---
title: Usage
sidebar_position: 5
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

:::info
For information regarding usage of OSHConnect-JavaScript, please refer to the [OSHConnect-JavaScript section](../osh-js/introduction.md)
:::

## Instantiating OSHConnect
The intended method of interacting with OpenSensorHub is through the main OSHConnect class, which allows for ways to interact with the OSH Node, through different coding langauges. To do this you must first create an instance of OSHConnect:
<Tabs groupId="oshconnect">
<TabItem value="python" label="Python">
```python
from oshconnect import OSHConnect, TemporalModes

app = OSHConnect(name='MyApp')
```
</TabItem>

<TabItem value="java" label="Java">
```java
OSHConnect oshConnect = new OSHConnect("OSHConnect");
```
</TabItem>

<TabItem value="cpp" label="C++">
```cpp
OSHConnect::OSHConnect oshConnect{ "OSHConnect" };
```
</TabItem>
</Tabs>
:::info
The name parameter is optional, but can be useful for debugging purposes.
:::

## Adding a Node
The next step is to add your running Node to the OSHConnect instance. 
A Node is a representation of a server that you want to be connected to, which can be augmented from your OSH Connect Project. 
The OSHConnect instance can support multiple Nodes at once.
<Tabs groupId="oshconnect">
<TabItem value="python" label="Python">
```python
node = Node(protocol='http', address='localhost', port=8585,
            username='test', password='test',
            enable_mqtt=True, mqtt_port=1883)
app.add_node(node)
```
</TabItem>

<TabItem value="java" label="Java">
```java
OSHNode node = oshConnect.createNode("localhost:8181/sensorhub", true, "admin", "admin");
```
</TabItem>

<TabItem value="cpp" label="C++">
```cpp
OSHConnect::OSHNode node{ oshConnect.getNodeManager().addNode("localhost:8181/sensorhub", "admin", "admin") };
```
Or, using an auth token:
```cpp
OSHConnect::OSHNode node{ oshConnect.getNodeManager().addNode("localhost:8181/sensorhub", myAuthToken, false) };
```
</TabItem>
</Tabs>

## System Discovery
Once you have added a Node to the OSHConnect instance, you can discover the systems that are available on that Node. 
This is done by calling the system discovery method on the OSHConnect instance.
<Tabs groupId="oshconnect">
<TabItem value="python" label="Python">
```python
app.discover_systems()
```
</TabItem>

<TabItem value="java" label="Java">
```java
oshConnect.discoverSystems();
```
</TabItem>

<TabItem value="cpp" label="C++">
```cpp
oshNode.discoverSystems();
```
</TabItem>
</Tabs>

## DataStream Discovery
Once you have discovered the systems that are available on a Node, you can discover the datastreams that are available to those systems. 
This is done by calling the datastream discovery method on the OSHConnect instance.
<Tabs groupId="oshconnect">
<TabItem value="python" label="Python">
```python
app.discover_datastreams()
```
</TabItem>

<TabItem value="java" label="Java">
```java
oshConnect.discoverDatastreams();
```
</TabItem>

<TabItem value="cpp" label="C++">
```cpp
oshSystem.discoverDatastreams();
```
</TabItem>
</Tabs>

## Retrieving Observations

Once you have discovered the datastreams available for a system, you can fetch observations from a datastream.
This is done by calling the fetch observations method on the OSHDataStream instance.
<Tabs groupId="oshconnect">
<TabItem value="python" label="Python">
```python
from oshconnect import StreamableModes
import time

for ds in app.get_datastreams():
	ds.set_connection_mode(StreamableModes.PULL)
    ds.initialize()
    ds.start()

time.sleep(2)  # allow messages to arrive
for ds in app.get_datastreams():
    while ds.get_inbound_deque():
        msg = ds.get_inbound_deque().popleft()
        print(msg)
```
</TabItem>

<TabItem value="java" label="Java">
```java
TODO
```
</TabItem>

<TabItem value="cpp" label="C++">
```cpp
oshDataStream.fetchObservations();
```
</TabItem>
</Tabs>

## Resource Insertion
Other use cases of the OSHConnect library may involve inserting new resources into OpenSensorHub or another Connected Systems API server.

### Systems
The first major step in a common workflow is to add a new system to the OSH Connect instance.
<Tabs groupId="oshconnect">
<TabItem value="python" label="Python">
```python
from oshconnect import OSHConnect, Node

app = OSHConnect(name='MyApp')
node = Node(protocol='http', address='localhost', port=8585,
            username='admin', password='admin')
app.add_node(node)

new_system = app.create_and_insert_system(
    system_opts={
        'name': 'Test System',
        'description': 'A test system',
        'uid': 'urn:system:test:001',
    },
    target_node=node
)
```
</TabItem>

<TabItem value="java" label="Java">
```java
TODO
```
</TabItem>

<TabItem value="cpp" label="C++">
```cpp
auto systemResource = ConnectedSystemsAPI::DataModels::SystemBuilder()
	.withType("Feature")
	.withProperties(ConnectedSystemsAPI::DataModels::PropertiesBuilder()
		.withFeatureType("http://www.w3.org/ns/sosa/Sensor")
		.withUid("test-sensor-001")
		.withName("Test Sensor 001")
		.withDescription("Test sensor")
		.withAssetType("Equipment")
		.build())
	.build();
OSHConnect::OSHSystem oshSystem = oshNode.createSystem(systemResource).value();
```
</TabItem>
</Tabs>

### DataStreams
Once you have a System object, you can add a new datastream to it. 
This is one of the more complex operations in the library as the schema is very flexible by design. 
Luckily, the schemas are validated by the underlying data models, 
so you can be sure that your datastream is valid before inserting it.
<Tabs groupId="oshconnect">
<TabItem value="python" label="Python">
```python
from oshconnect import DataRecordSchema, TimeSchema, QuantitySchema, TextSchema
from oshconnect.api_utils import URI, UCUMCode

datarecord = DataRecordSchema(
    label='Example Record',
    description='Example datastream record',
    definition='http://example.org/records/example',
    fields=[]
)

# TimeSchema must be the first field for OSH
datarecord.fields.append(
    TimeSchema(label='Timestamp', definition='http://www.opengis.net/def/property/OGC/0/SamplingTime',
               name='timestamp', uom=URI(href='http://www.opengis.net/def/uom/ISO-8601/0/Gregorian'))
)
datarecord.fields.append(
    QuantitySchema(name='distance', label='Distance', definition='http://example.org/Distance',
                   uom=UCUMCode(code='m', label='meters'))
)
datarecord.fields.append(
    TextSchema(name='label', label='Label', definition='http://example.org/Label')
)

datastream = new_system.add_insert_datastream(datarecord)
```
</TabItem>

<TabItem value="java" label="Java">
```java
TODO
```
</TabItem>

<TabItem value="cpp" label="C++">
```cpp
using namespace ConnectedSystemsAPI::DataModels;
auto dataStreamResource = DataStreamBuilder()
	.withName("Test DataStream 001")
	.withOutputName("test_output_001")
	.withDescription("This is a test data stream")
	.withSchema(std::make_unique<ObservationSchema>(ObservationSchemaBuilder()
		.withObservationFormat("application/om+json")
		.withResultSchema(std::make_unique<Component::DataRecord>(Component::DataRecordBuilder()
			.withType("DataRecord")
			.addField(std::make_unique<Component::Boolean>(Component::BooleanBuilder()
				.withType("Boolean")
				.withName("booleanField")
				.withDescription("This is a test boolean field")
				.build()))
			.build()))
		.build()))
	.build();
OSHConnect::OSHDataStream = oshSystem.createDataStream(dataStreamResource).value();
```
</TabItem>
</Tabs>
:::info
A TimeSchema is required to be the first field in the DataRecordSchema for OSH.
:::

### Observations
Upon successfully adding a new datastream to a system, it is now possible to send observation data to the node.
<Tabs groupId="oshconnect">
<TabItem value="python" label="Python">
```python
from oshconnect import TimeInstant

datastream.insert_observation_dict({
    'resultTime': TimeInstant.now_as_time_instant().get_iso_time(),
    'phenomenonTime': TimeInstant.now_as_time_instant().get_iso_time(),
    'result': {
        'timestamp': TimeInstant.now_as_time_instant().epoch_time,
        'distance': 1.0,
        'label': 'example observation',
    }
})
```
</TabItem>

<TabItem value="java" label="Java">
```java
TODO
```
</TabItem>

<TabItem value="cpp" label="C++">
```cpp
using namespace ConnectedSystemsAPI::DataModels;

//Get the schema to know what kind of observation to create
auto dataStreamResource = dataStream.getDataStreamResource();
auto schema = dataStreamResource.getSchema()->getResultSchema();
auto schemaDataRecord = dynamic_cast<const Component::DataRecord*>(schema);

// Create a data block according to the schema and set the values of the fields
auto dataBlock = schemaDataRecord->createDataBlock();
dataBlock.setField("booleanField", Data::DataValue(true));

auto observation = ObservationBuilder()
	.withResultTime(TimeInstant(std::chrono::system_clock::now()))
	.withResult(dataBlock)
	.build();
std::string observationId = dataStream.createObservation(observation);
```
</TabItem>
</Tabs>
:::info
The `resultTime` and `phenomenonTime` fields are required for OSH.  
You’ll notice that they are referred to by their name field in the schema as it is the “machine” name of the output.
:::