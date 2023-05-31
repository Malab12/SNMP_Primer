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
1. [Constructor](#snmpcommander-constructor-)
2. [start](#start)
3. [stop](#stop)
4. [publishChangedParam](#publishchangedparams)
5. [handleRequest](#handlerequest)

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
**Boolean:** identifies if the request executed successfully or not

--------
### SnmpAgent

### Associations
1. **SnmpParameter:** A structure encapsulating all fields required by the agent to act upon the various parameters
2. **MIBStruct:** A structure encapsulating all fields required by the agent to generate the various MIB files

### Private variables
1. **name:** name of the agent process and library intialization (KEEP IT AS snmpd)
2. **node:** The YAML node containing the configuration information for the snmp agent
3. **thread:** The thread on which the agent proces will run
4. **isInitialized:** boolean status on the agent intialization process
5. **isRunning:** boolean status on the agent run procedure
6. **generateMIB:** boolean stating if MIB files need to be generated or not
7. **fileStructures:** Vector of MIB structure pointers containing list of MIB files to be created and their associated fields

### Public variables
1. **lookupTable:** A map of SNMP parameter structs containing all parameters the agent has access to and the relavent fields associated with them required for making requests on the parameter on the LSC side

### Private methods
1. [convertLscStringTypeToMIBType](#convertlscstringtypetomibtype)
2. [convertLscStringTypetoSNMPType](#convertlscstringtypetosnmptype)
3. [populateLookupFromYaml](#populatelookupfromyaml)
4. [headerGenerate](#headergenerate)
5. [writeScalarParameter](#writescalarparameter)
6. [writeVectorParameter](#writevectorparameter)

### Public methods
1. [Constructor](#snmpagent-constructor-)
2. [start]()
3. [stop]()
4. [reciever]()
5. [registerParameters]()
6. [registerTable]()
7. [scalarToScalarGetHandler]()
8. [vectorToScalarGetHandler]()
9. [scalarToScalarSetHandler]()
10. [vectorToScalarSetHandler]()
11. [tableHandler]()
12. [scalarHandler]()
13. [ipv4AddressHandler]()
14. [handleRequest]()
15. [generateMIBFiles]()

### convertLscStringTypeToMIBType
#### Arguments:
#### Returns:
#### Description:

### convertLscStringTypetoSNMPType
#### Arguments:
#### Returns:
#### Description:

### populateLookupFromYaml
#### Arguments:
#### Returns:
#### Description:

### headerGenerate
#### Arguments:
#### Returns:
#### Description:

### writeScalarParameter
#### Arguments:
#### Returns:
#### Description:

### writeVectorParameter
#### Arguments:
#### Returns:
#### Description:

### SNMPAgent[ Constructor ]
#### Arguments:
#### Returns:
#### Description:

### start
#### Arguments:
#### Returns:
#### Description:

### stop
#### Arguments:
#### Returns:
#### Description:

### reciever
#### Arguments:
#### Returns:
#### Description:

### registerParameters
#### Arguments:
#### Returns:
#### Description:

### registerTable
#### Arguments:
#### Returns:
#### Description:

### scalarToScalarGetHandler
#### Arguments:
#### Returns:
#### Description:

### vectorToScalarGetHandler
#### Arguments:
#### Returns:
#### Description:

### scalarToScalarSetHandler
#### Arguments:
#### Returns:
#### Description:

### vectorToScalarSetHandler
#### Arguments:
#### Returns:
#### Description:

### tableHandler
#### Arguments:
#### Returns:
#### Description:

### scalarHandler
#### Arguments:
#### Returns:
#### Description:

### ipv4AddressHandler
#### Arguments:
#### Returns:
#### Description:

### handleRequest
#### Arguments:
#### Returns:
#### Description:

### generateMIBFiles
#### Arguments:
#### Returns:
#### Description:
