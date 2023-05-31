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
2. [start](#start-1)
3. [stop](#stop-1)
4. [reciever](#reciever)
5. [registerParameters](#registerparameters)
6. [registerTable](#registertable)
7. [scalarToScalarGetHandler](#scalartoscalargethandler)
8. [vectorToScalarGetHandler](#vectortoscalargethandler)
9. [scalarToScalarSetHandler](#scalartoscalarsethandler)
10. [vectorToScalarSetHandler](#vectortoscalarsethandler)
11. [tableHandler](#tablehandler)
12. [scalarHandler](#scalarhandler)
13. [ipv4AddressHandler](#ipv4addresshandler)
14. [handleRequest](#handlerequest)
15. [generateMIBFiles](#generatemibfiles)

### convertLscStringTypeToMIBType
#### Arguments:
**lscParameter:** structure containing relevant information for the parameter 
#### Returns: 
**String:** equivalent string reperesentation of the datatype for the MIB files
#### Description:
takes in a LSC parameter and based on the datatype represents an equivalent string representation which is usable in a MIB file

### convertLscStringTypetoSNMPType
#### Arguments:
**type:** string reperesenting the datatype of the lsc parameter
#### Returns:
**uchar:** enum value associated with a particular snmp datatype
#### Description:
Takes in a string representation of the LSC parameter datatype and returns back a u_char value associated with the equivalent snmp datatype

### populateLookupFromYaml
#### Arguments:
1. **parameterList:** YAML node containing a list of parameters
2. **deviceID:** the parent deviceID under which this list of parameters will be registered
3. **groupName:** the particular mib group these parameters belong to
#### Returns:
void
#### Description:
1. Populates the lookupTable with the list of parameters provided.
2. Iterate through the parameters and get the individual fields
3. The ID of each parameter is calculated as: 100 * deviceID + parameterID
4. add new snmpParameter using the fields with the key as the ID in the lookupTable

### headerGenerate
#### Arguments:
1. **name:** name of the MIB module
2. **fileName:** name of the MIB file
3. **productName:** the product branch under which this MIB will be registered
4. **ID:** ID of the MIB module under the product
5. **writer:** ostream writer utilized to write into the file
#### Returns:
void
#### Description:
Generates the *almost* static header of each MIB file

### writeScalarParameter
#### Arguments:
1. **parameter:** specific snmpParameter being written into
2. **writer:** ostream writer utilized to write into the file
#### Returns:
void
#### Description:
Writes the scalar parameter into the MIB file in the proper format

### writeVectorParameter
#### Arguments:
1. **parameter:** specific snmpParameter being written into
2. **writer:** ostream writer utilized to write into the file
#### Returns:
void
#### Description:
Writes the vector parameter into the MIB file in the proper format

### SNMPAgent[ Constructor ]
#### Arguments: 
1. **name:** SNMPD the name for the application and library
2. **config:** YAML node containing the configuration information for 
#### Returns:
#### Description:
Constructor for the SNMPAgent class. Configures information essential for the agent to run properly (community, port and user information)

### start
#### Arguments:
void
#### Returns:
void
#### Description:
1. reads the parameter YAML files and populates the lookupTable
2. if required, generates the MIB files 
3. initialises the agent and starts it run procedure

### stop
#### Arguments:
void
#### Returns:
void
#### Description:
1. starts the joining procedure of the the SNMP thread with the main LSC thread
2. Does the necessary snmp cleanup and shutdown procedures

### reciever
#### Arguments:
void
#### Returns:
void
#### Description:
1. waits for intialization to complete
2. runs the agent on a seperate thread
3. listens for snmp requests 

### registerParameters
#### Arguments:
void
#### Returns:
void
#### Description:
1. iterates through the parameters in the lookup table
2. based on the type of parameter, registers it as either a scalar or vector

### registerTable
#### Arguments:
snmpParameter: The parameter to be registered
#### Returns:
void
#### Description:
1. creates a table_data_set
2. populates with default values
3. registers the table with the table handler

### scalarToScalarGetHandler
#### Arguments:
1. **requests:** struct containing information from the SNMP request
2. **parameter:** pointer to the parameter being queried to
3. **response:** populated by LSC
#### Returns:
void
#### Description:
1. Specific LSC->SNMP middle handler which handles get requests for parameters which are scalar to SNMP as well as LSC
2. Based on the datatype of the parameter, we populate the requests with the value extracted from the LSC

### vectorToScalarGetHandler
#### Arguments:
1. requests: netsnmp_request_info struct containing essentail information from the SNMP request
2. parameter: pointer to the specific snmpParameter struct being queried to
3. index: integer id identifying which value in the vector param specifically is being queried
4. responseArray: response populated by LSC
#### Returns:
void 
#### Description:
1. specific LSC->SNMP middle handler which handles get requests for parameters scalar to SNMP but vector to LSC
2. Based on the datatype of the parameter and the index, we populate the requests with the value extracted from the LSC

### scalarToScalarSetHandler
#### Arguments:
#### Returns:
#### Description:
you get the gist same vibe really
### vectorToScalarSetHandler
#### Arguments:
#### Returns:
#### Description:
you get the gist same vibe really

### tableHandler (STATIC METHOD)
#### Arguments:
#### Returns:
#### Description:
generic table parameter handler

### scalarHandler (STATIC METHOD)
#### Arguments:
#### Returns:
#### Description:
generic scalar parameter handler

### ipv4AddressHandler
#### Arguments:
#### Returns:
#### Description:
specific handler for ipv4Address because this baby is a special little snowflake (integer in LSC but string on SNMP)

### handleRequest (VIRTUAL FUNCTION)
#### Arguments:
#### Returns:
#### Description:


### generateMIBFiles
#### Arguments:
#### Returns:
#### Description:
amalgamate method which generates all the MIB files
