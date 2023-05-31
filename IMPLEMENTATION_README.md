# Implementation

## Main Classes of intereset
1. [SnmpCommander](#snmpcommander)
2. [SnmpAgent](#snmpagent)
--------
## Secondary classes of interest
1. [LinuxSystemController]()
2. [CommanderBase]()
3. [Parameter]()
--------
## External YAML configuration files:
1. [LinuxSystemController.yaml]()
2. [parameterList.yaml]()
3. individual [groupParameter]() files (basebandParameters.yaml, networkParameters.yaml etc) 
--------
## SnmpCommander
**Path:** system-controller/Linux/Commander/SnmpCommander.

### Associations
1. Inherits [SnmpAgent](#snmpagent) publicly
2. Inherits [CommanderBase](#commander) publicly

### Public methods
1. [Constructor]
2. [start]
3. [stop]
4. [publishChangedParam]
5. [handleRequest]

### SNMPCommander[ Constructor ]
#### Arguments:
1. String name
2. YAML::Node config
3. ControllerDevicePtr controllerDevice
#### Return
Nill

### SnmpAgent
