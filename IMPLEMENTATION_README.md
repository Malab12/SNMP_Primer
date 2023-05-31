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
1. **name:** name of the process 
2. **config:** YAML Node structure containing the config info for the snmp agent
3. **controllerDevice:** pointer to a specific controllerDevice object, in our case **LinuxSystemController**
#### Description:
Constructor for the SNMPCommander Class. Provides the arguments before calling the constructors for the [CommanderBase](#commander) and the [SNMPAgent](#snmpagent) classes

### Start
#### Arguments:
None
#### Returns:
None
#### Description:
calls the start methods of the [CommanderBase](#commander) and the [SNMPAgent](#snmpagent) classes

### Stop
#### Arguments:
None
#### Returns:
None
#### Description:
calls the stop methods of the [CommanderBase](#commander) and the [SNMPAgent](#snmpagent) classes

### publishChangedParams
#### Arguments:
1. **devicePath:** The specific device the parameter belongs to (NODE_MANAGER, IOPARAMS, etc)
2. **param:** the specific parameter we want to get data from
#### Returns:
**response:** An any structure containing the information requested
#### Description:
uses the \_runCommand method inherited from [CommanderBase](#commander) to get the value of the parameter as specified by the arguments, returns the response for the command

### handleRequest
#### Arguments: 
1. **requestHandler:** the devicepath and the type of request to be executed (NODE_MANAGER, get/set)
2. **request:** the specific parameter and the specific index( optional ) on which the request handler will act
3. **response:** the object to be populated by the execution of the request (This is a return parameter)
#### Returns:
Boolean: identifies if the request executed successfully or not
**
### SnmpAgent
