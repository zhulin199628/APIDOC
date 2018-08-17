# Axalent Cloud XGM API Manual

## 1. Instruction

XGM is the device-side link library of Axalent Cloud, using XGM to connect gateway-like devices to Axalent Cloud, and connect sub-devices(Bluetooth, zigbee, z-wave, etc.) under the gateway to Axalent Cloud. XGM is applicable to gateway devices that have rich resources and need to access sub-devices.

## 2. XGM API

### 2.1 AxMsgCallbackFun

Definition of function type: typedef int (*AxMsgCallbackFun)(t_deviceId* devId, const char* strName, const char* strValue)

#### Usage

Define the form of callback function. When XGM receive the message, the callback function will be called, it can be defined as shown below:
int AxMsgCallback(t_deviceId* devId, const char* strName, const char* strValue)
Use AxInit function to transfer this function to XGM. When XGM receive the attributes send by server the function will be called. devId is the only identification for the local sub-device, specified when AxLogin. If the devId is null, indicating that it is the gateway attribute. strName is the name of attribute, strValue is the value of attribute.
PS: Generally speaking, if devId is null, indicating that it is the gateway attribute. If the devId is not null, indicating that it is the sub-device attribute. However, the “deletedevice” is very unique. When the deletedevice attribute has been received, regardless of the devId value of devId, all indicating that it is the gateway attribute. The value of devId indicates which sub-device will be deleted. devId is passed in when AxLogin.
When XGM received the attribute of permitjoin, it will pass the permitjoin attribute and attribute value through the callback function, then the developer performs the operation of adding device locally and uses the AxLogin function to register the added devices to server. If XGM receives the attribute of permitjoin, but the AxLogin function hasn’t been called, the new device will not be added to the server. 
When XGM receives the attribute of deletedevice, it will pass the deletedevice attribute and attribute value through the callback function, then the developer performs the operation of deleting device locally and uses the AxDelete function to delete the device from the server. If the XGM receives deletedevice attribute only, the AxDelete hasn’t been called, the server will not delete the device. 

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
devId|t_deviceId*|Device local ID, introducing when calling the AxLogin function. If it is a gateway-like device, the devId is null.|
strName|const char*|Received attribute name|
strValue|const char*|Received attribute value|

#### Return

Generally, int is 0

### 2.2. GwConnectCallbackFun

Definition of the function type: typedef int (*GwConnectCallbackFun)(gboolean ConnectStatus)

#### Usage

Define the form of callback function. When the connection status between the gateway and server has been changed, the callback function will be called, it can be defined as shown below: 
int GwConnectCallback(gboolean ConnectStatus)
Use AxInit function to pass the callback function to XGM, when gateway login the server successfully, this function will be called. ConnectStatus is the connection status. connect succeeded: True，connect failed: False.

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
ConnectStatus|gboolean|Connect succeeded: True，connect failed: False.|

#### Return
int

### 2.3 connectionChangedCb

Definition of the function type: typedef gpointer (*connectionChangedCb)(gboolean status, gpointer data)

#### Usage
Define the form of callback function. When the connection status of sub-device and server has been changed, this callback function will be called, it can be defined as shown below: 
gpointer connectionChangedCb(gboolean status, gpointer data)

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
status|gboolean|The connection status of sub-devices. Connect succeeded: True，connect failed: False.|
data|gpointer|Custom data, can be null|

#### Return

gpointer

### 2.4 UserSuccessCb

Definition of the function type: typedef gpointer (*UserSuccessCb)(gpointer data)

#### Usage
Define the form of callback function. AxLogin, AxLogout, AxDelete, AxSetProperty, all this function are async. The return value of function only indicates whether the XGM receives the operation or not. Then the operation will be done by XGM and the server, the result will be returned by the callback function. The succeed callback function are as shown below:
gpointer UserSuccessCb(gpointer data)
Use AxLogin、AxLogout、AxDelete、AxSetProperty all these function to pass the last parameter: gpointer data to XGM. This function will be called when these operations succeed. Data is the custom data, introducing by AxLogin、AxLogout、AxDelete、AxSetProperty and return in the callback function.

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
data|gpointer|Custom data, can be null|

#### Return
gpointer

### 2.5 UserServerTimeCb

Definition of the function type: typedef gpointer (*UserServerTimeCb)(axa_timestamp* serverTime, gpointer data)

#### Usage
The form of callback function that defines the success of AxGetTime execution, this callback function can be defined as shown below: 
gpointer UserServerTimeSuccessCb(axa_timestamp serverTime, gpointer data)
Use AxGetTime function to pass the last parameter: gpointer data to XGM. This function will be called when the operation succeed. ServerTime is the time structure returned by function and we can get the GMT for current server from it. Data is custom data, introducing by AxGetTime, and return in the callback function

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
serverTime|axa_timestamp|Time structure：<br>typedef struct{<br>uint16_t year;<br>uint16_t month;<br>uint16_t day;<br>uint16_t hour;<br>uint16_t minute;<br>uint16_t second;<br>}axa_timestamp_t;|
data|gpointer|Custom data, can be null|

#### Return
gpointer

### 2.6 UserFailedCb

函数类型定义：typedef gpointer (*UserFailedCb)(int errorCode, gpointer data)

#### Usage

Define the form of callback function. AxLogin, AxLogout, AxDelete, AxSetProperty, all this function are async. The return value of function only indicates whether XGM receives the operation or not. Then the operation will be done by XGM and the server, the result will be returned by the callback function. The failed callback function are as shown below: 
gpointer UserFailedCb(int errorCode, gpointer data)
Use AxLogin、AxLogout、AxDelete、AxSetProperty、AxGetTime function to pass the last parameter: gpointer data to XGM. This function will be called when the operation failed. ErrorCode is the returned error code. Data is the custom data, introducing by AxLogin、AxLogout、AxDelete、AxSetProperty、AxGetTime and return in callback function.


#### Parameters

|Parameter name|Type|Description|
|----|----|----|
errorCode|errorCode|Error code|
data|gpointer|Custom data, can be null|

#### Return

gponiter

### 2.7 AxInit

Function prototype: axa_result_e AxInit(AxMsgCallbackFunc pMsgCb, GwConnectcallbackFun pGwConnectCb,const char * workDir)

#### Usage
Start running the XGM, it will import the configuration information in the configuration file gw_config.xml and let the gateway login the Axalent Cloud, and keep the connection with the Axalent Cloud. This function will be called at the very beginning of the program.


#### Parameters

|Parameter name|Type|Description|
|----|----|----|
pMsgCb|AxMsgCallbackFunc|Detailed in 2.1|
pGwConnectCb|GwConnectcallbackFun|Detailed in 2.2|
workDir|const char *|Specify the path to the gw_config.xml configuration file，If the configuration file and the executable file are in the same directory, then the parameter should be set as “./”. If the configuration file cannot be found in the specified path, the program will report an error.|

#### Return

Return successfully: 0 (AXA_SUCCESS), return failed: -1(AXA_FAILURE).

### 2.8 AxLogin

Function prototype: axa_result_e AxLogin(t_deviceId* devId, char* type, connectionChangedCb changedCb, gpointer data)

#### Usage

Use devId and type to login a sub-device. Then the Axalent Cloud will be told that there is a new sub-device has been added, and changes its status to online. The unique ID (local ID) of the sub-device needs to be introduced when the function calls. This ID supports up to 64 bits and is defined by the developer. Be sure to remember this devId, AxSetProperty and pMsgCallback will use this ID as the unique identifier to determine the sub-devices. The change of the login status of the device will be returned by the callback function.( If the status callback function returns that the device is abnormally dropped, XGM will automatically re login the device, and the developer does not need to have the same sub-device again with 

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
devId|t_deviceId*|Device local ID|
type|char*|Device type, need define in the server first |
changedCb|connectionChangedCb|Detailed in 2.3, can be null|
data|gpointer|Custom data, can be null|

#### Return

AXA_SUCCESS indicates that XGM has received the operation of sub-devices login successfully, other return value means the function need be recalled to login. 

### 2.9 AxLogout

Function prototype: axa_result_e AxLogout(t_deviceId* devId, UserSuccessCb successCb, UserFailedCb failedCb, gpointer data)

#### Usage

Logout a sub-device with devId. This devId is introduced when AxLogin and this sub-device can login again by AxLogin. If a sub-device needs to be deleted, the AxDelete function is required.
The difference between AxLogout and AxDelete: AxLogout simply changes the status of device to offline and does not remove the device from the server, AxLogout not only changes the status of device to offline and also delete the device from the server.

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
devId|t_deviceId*|Device Local ID|
successCb|UserSuccessCb|Detailed in 2.4, can be null|
failedCb|UserFailedCb|Detailed in 2.6, can be null|
data|gpointer|Custom data, can be null|

#### Return

AXA_SUCCESS indicates that XGM has received the requirement and the result will be returned with the callback function. Other return value means failure.

### 2.10 AxDelete

Function prototype: axa_result_e AxDelete(t_deviceId* devId, UserSuccessCb successCb, UserFailedCb failedCb, gpointer data)

#### Usage
Delete a sub-device by devId. If a sub-device needs to be deleted from the local network, this function will be called to tell the Axalent Cloud deleting the sub-device. If the sub-device only wants to offline, use the AxLogout function, and if it wants to rejoin the network, use the AxLogin function。

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
devId|t_deviceId*|Device Local ID|
successCb|UserSuccessCb|Detailed in 2.4, can be null|
failedCb|UserFailedCb|Detailed in 2.6, can be null|
data|gpointer|Custom data, can be null|

####Return

AXA_SUCCESS indicates that XGM has received the requirement and the result will be returned with the callback function. Other return value means failure.

### 2.11 AxSetProperty

Function prototype: axa_result_e AxSetProperty(t_deviceId* devId, char* property, UserSuccessCb successCb, UserFailedCb failedCb, gpointer data)

#### Usage

Send a message of device attribute to Axalent Cloud. The device attribute consists of attribute name and attribute value separated by space bar. If the device attribute to be sent is the gateway attribute, devId should be NULL.


#### Parameters

|Parameter name|Type|Description|
|----|----|----|
devId|t_deviceId*|Device Local ID, gateway device is null|
property|char*|attribute name+ space bar+ attribute value|
successCb|UserSuccessCb|Detailed in 2.4, can be null|
failedCb|UserFailedCb|Detailed in 2.6, can be null|
data|gpointer|Custom data, can be null|

#### Return

AXA_SUCCESS indicates that XGM has received the requirement and the result will be returned with the callback function. Other return value means failure

### 2.12 AxGetTime

Function prototype: axa_result_e AxGetTime(userServerTimeCb successCb, UserFailedCb failedCb, gpointer data)

### Usage

Get the current time from Axalent Cloud, the GMT. The acquisition time is successful and will be returned via the callback function.

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
successCb|userServerTimeCb|Detailed in 2.5|
failedCb|UserFailedCb|Detailed in 2.6, can be null|
data|gpointer|Custom data, can be null|

#### Return

axa_result_e, AXA_SUCCESS indicates that XGM has received the requirement and the result will be returned with the callback function. Other return value means failure.
