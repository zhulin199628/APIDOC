# Axalent Cloud XCA API Manual

## 1. Instruction

XCA is the program connection library on the Axalent Cloud device side, usually it runs on a resource-limited device platform, accessing to a single device node, for example: WIFI, GPRS, NB, etc. that can link to the internet. Resources occupied less, and can compile the program link library for different platform XCA API  

## 2. XCA API

###  2.1 OnConnectionChangedCb

Definition of function type：typedef void (*OnConnectionChangedCb)(int status)

#### Usage

Call back function, When the connection status of XCA and server changes, it will be called. The connection status will be released by parameter (Type int)，connection succeeded：<strong>TRUE，</strong>disconnect：<strong>FALSE</strong>

#### Parameters
|Parameter name|Type|Description|
|----|----|----|
status|int|The status of connection, connection succeeded：<strong>TRUE，</strong>disconnect：<strong>FALSE</strong>|

#### Return
Null

### 2.2 SessionSuccessCb

Definition of function type：typedef gpointer (*SessionSuccessCb)(gpointer data)

#### Usage

Callback function, aca3_session_login、aca3_session_login_ex、aca3_session_logout、aca3_session_sendProperty、aca3_session_sendPropertyWithOptions, etc. All these parameter are async. The callback value of parameter only means the XCA has accepted the operation. Then the XCA will communicate with the server and return the result as a callback function, this type of callback function is called to indicate the operation was successful. Data is a custom data, and it will be introduced when these parameters are being called: aca3_session_login、aca3_session_login_ex、aca3_session_logout、aca3_session_sendProperty、aca3_session_sendPropertyWithOptions

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
data|gpointer|Custom data, can be null.|

#### Return

gpointer

###  2.3 SessionFailedCb

Definition of the function type: typedef gpointer (*SessionConnectionChangedCb)(gboolean status, gpointer data)

#### Usage

Callback function, aca3_session_login、aca3_session_login_ex、aca3_session_logout、aca3_session_sendProperty、aca3_session_sendPropertyWithOption, all these function are async, the parameter value only means the XCA has accepted the operation. Then the XCA will communicate with server, and return the result as a callback function, this type of callback function is called to indicate the operation was failed. ErrorCode is the error code, data means custom data. It will be introduced when these parameters are being called:aca3_session_login、aca3_session_login_ex、aca3_session_logout、aca3_session_sendProperty、aca3_session_sendPropertyWithOptions

#### Parameters
|Parameter name|Type|Description|
|----|----|----|
errorCode|int|The status of connection, connection succeeded: TRUE, disconnect: FALSE|
data|gpointer|Custom data, can be null|

#### Return

gpointer

### 2.4 SessionConnectionChangedCb

Definition of the function type: typedef gpointer (*SessionConnectionChangedCb)(gboolean status, gpointer data)

#### Usage

It is a callback function, when the connection status of device and server has been changed, this function will be called. The connection status: connection succeeded: TRUE, disconnect: FALSE. Data is the custom data, introducing when aca3_session_login、aca3_session_login_ex are called

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
status|gboolean|The status of connection, connection succeeded：<strong>TRUE，</strong>disconnect：<strong>FALSE</strong>|
data|gpointer|Custom data，can be<strong>NULL</strong>|

#### Return

gpointer

### 2.5 SessionServerTimeCb

Definition of function type：typedef gpointer (*SessionServerTimeCb)(st_session_serverTime* serverTime, gpointer data)

#### Usage

It is a callback function, after the request time from the server is successful this function will be called. ServerTime is the returned server time structure. Data is the custom data, introducing when aca3_session_getServerTime are called.

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
serverTime|st_session_serverTime*|Time istructure：<br>typedef struct{<br>guint16 year;<br>guint16 month;<br>guint16 day;<br>guint16 hour;<br>guint16 minute;<br>guint16 second;<br>}st_session_serverTime;|
data|gpointer|Custom data，can be <strong>NULL</strong>|

#### Return

gpointer

### 2.6 SessionReceivedSingleProperty

Definition of function type：typedef void (*SessionReceivedSingleProperty)(guint32 childId, char* property, gpointer data)

#### Usage

It is a callback function, it will be introduced when the XCA has received information from servers. childId is generally zero., property is the message, including 2 part: attribute name and attribute value, using space bar to divide. Data is custom data.

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
childId|guint32|Generally 0|
property|char*|The device message send by server, attribute name+ space bar+ attribute value|
data|gpointer|Custom data, can be null|


#### Return
Null

### 2.7 aca3_gw_setLb

Function prototype：aca3_result_e aca3_gw_setLb(int index, const char* lb)

#### Usage

Set 3 server domain name or IP address of the XCA connection (can be same), and need introduce before aca3_gw_init.

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
index|int|The value can be 0, 1, 2, other values will return as a error|
lb|const char *|It can be a domain name or a point split IP address|

#### Return

aca3_result_e，ACA3_OK means success, otherwise means failure.

### 2.8aca3_gw_setTcpPort

Function prototype：aca3_result_e aca3_gw_setTcpPort(guint16 port)

#### Usage

Set the server TCP port number of the XCA connection, which needs to be called before aca3_gw_init.

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
port|guint16|TCP port number|

#### Return
aca3_result_e，ACA3_OK means success, otherwise means failure.

### 2.9 aca3_gw_setUdpPort

Function prototype：aca3_result_e aca3_gw_setUdpPort(guint16 port)

#### Usage

Set the server UDP port number of the XCA connection, which needs to be called before aca3_gw_init.

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
port|guint16|UDP port number|

#### Return

aca3_result_e，ACA3_OK means success, otherwise means failure.

### 2.10 aca3_gw_setConnectionChangedListener

Function prototype : void aca3_gw_setConnectionChangedListener(OnConnectionChangedCb statusChangedCb)

#### Usage

Set the callback function, it will be introduced when the connection status between the XCA and server changes, and needs to be called before aca3_gw_init

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
statusChangedCb|OnConnectionChangedCb|Detailed in 2.1|

#### Return

aca3_result_e，ACA3_OK means success, otherwise means failure.


### 2.11 aca3_gw_init

Function prototype：aca3_result_e aca3_gw_init(void)

#### Usage
Initialize the XCA to connect to server. Introducing this function, then XCA will connect to server according to what is set by API, after the connection succeed the result will be returned by the set callback function 

#### Parameters
 Null

#### Return

aca3_result_e，ACA3_OK means success, otherwise means failure.

### 2.12 aca3_session_login

Function prototype：aca3_result_e aca3_session_login(st_session** session, char* code, char* pwd, guint16 productId, char* type, SessionConnectionChangedCb changedCb, gpointer userData)
#### Usage

After the function introduction is succeed, the XCA will use a non-encrypted way to login to the server and will maintain the connection between device and server. If the device is abnormally dropped, XCA will automatically log in to the server again..

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
session|st_session**|Session connection|
code|char*|Device-side code, each device has its own unique code|
pwd|char*|Device-side password，each device has its own unique password|
productId|guint16|Product ID，like the appID|
type|char*|Device type|
changedCb|SessionConnectionChangedCb|Detailed in 2.4|
userData|gpointer|Custom data，can be <strong>NULL</strong>|

#### Return

aca3_result_e，ACA3_OK means success, otherwise means failure.
### 2.13 aca3_session_login_ex

Function prototype：aca3_result_e aca3_session_login_ex(st_session** session, char* code, char* pwd, char* aes, guint16 productId, char* productAes, char* type, gboolean isEncrypt, SessionConnectionChangedCb changedCb, gpointer userData)

#### Usage
 
After the function introduction is succeed, the XCA can login the server by two different ways: encrypted or unencrypted, and will maintain the connection between device and server. If the device is abnormally dropped, XCA will automatically log in to the server again. 

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
session|st_session**|Session connection|
code|char*|Device-side code, each device has its own unique code|
pwd|char*|Device-side password，each device has its own unique password.|
aes|char*|Device side encryption: device aes key|
productId|Product ID，like the appID|
productAes|char*|Device side encryption: product aes key|
type|char*|Device type|
isEncrypt|gboolean|Encrypted or unencrypted|
changedCb|SessionConnectionChangedCb|Detailed in 2.4|
userData|gpointer|Custom data，can be<strong>NULL</strong>|

#### Return

aca3_result_e，ACA3_OK means success, otherwise means failure.

### 2.14&nbsp;aca3_session_setReceivedSinglePropertyCb

Function prototype：aca3_result_e aca3_session_setReceivedSinglePropertyCb(st_session* session, SessionReceivedSinglePropertyCb onReceivedPropertyCb, gpointer userData)

#### Usage

Set the callback function. When the XCA receives the message from server, the function will be called and release the message by parameter. 

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
session|st_session*|The session connection is sent after aca3_session_login/aca3_session_login_ex is successfully executed|
onReceivedPropertyCb|SessionReceivedSinglePropertyCb|Detailed in 2.6|
userData|gpointer|Custom data，can be <strong>NULL</strong>|

#### Return

aca3_result_e，ACA3_OK means success, otherwise means failure.

### 2.15 aca3_session_isLogin

Function prototype：gooblean aca3_session_isLogin(st_session* session)

#### Usage

Get the online status of the device

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
session|st_session*|The session connection is sent after aca3_session_login/aca3_session_login_ex is successfully executed.|

#### Return

gboolean，TRUE means onlie，FALSE means offline

### 2.16 aca3_session_getEndpointId

Function prototype: gint32 aca3_session_getEndpointId(st_session* session)

#### Usage

Get the device ID of device on the server( The unique identifier on the server)。

|Parameter name|Type|Description|
|----|----|----|
session|st_session*|The session connection is sent after aca3_session_login/aca3_session_login_ex is successfully executed. |

#### Return

gint32, the device ID of device on the server 

### 2.17 aca3_session_logout

Function prototype: aca3_result_e aca3_session_logout(st_session* session, SessionSuccessCb successCb, SessionFailedCb failedCb, gpointer userData)

#### Usage

The XCA logs out the device from the server and the state of the device becomes offline.

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
session|st_session*|The session connection is sent after aca3_session_login/aca3_session_login_ex is successfully executed.|
successCb|SessionSuccessCb|Detailed in 2.2, can be NULL|
failedCb|SessionFailedCb|Detailed in 2.3，can be null|
userData|gpointer|Custom，can be null|

#### Return

aca3_result_e，ACA3_OK means success, otherwise means failure.

### 2.18 aca3_session_sendProperty

Function prototype: aca3_result_e aca3_session_sendProperty(st_session* session, char* property, SessionSuccessCb successCb, SessionFailedCb failedCb, gpointer userData)

#### Usage

Send a message to server, the message consists of an attribute name and an attribute value separated by spaces. For example: Update the luminance of lights( Luminance is 70) then the property should be “dimmer 70”. 

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
session|st_session*|The session connection is sent after aca3_session_login/aca3_session_login_ex is successfully executed.|
property|char*|The device message needs to be update, attribute name+ space bar+ attribute value|
successCb|SessionSuccessCb|Detailed in 2.2, can be null|
failedCb|SessionFailedCb|Detailed in 2.3, can be null|
userData|gpointer|gpointer|Custom data, can be null|

#### Return

aca3_result_e，ACA3_OK means success, otherwise means failure.

### 2.19 aca3_session_sendPropertyWithOptions

Function prototype: aca3_result_e aca3_session_sendPropertyWithOptions(st_session* session, char* property, SessionSuccessCb successCb, SessionFailedCb failedCb, guint8 retryMax, gpointer userData)

#### Usage

Send a message to server, the message consists of an attribute name and an attribute value separated by spaces. For example: Update the luminance of lights( Luminance is 70) then the property should be “dimmer 70”.
PS：The difference from the aca3_session_sendPropert: When XCA sends message to server, If the message fails due to an exception, the XCA will resend the message after a few seconds. The default maximum number of retries is twice(That means it can be sent for 3 times, If successful or failed, the callback function will be executed), otherwise you can set to 0 if there is no re-sending. If the maximum number of resending set is more than twice, XCA will automatically correct to twice.

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
session|st_session*|The session connection is sent after aca3_session_login/aca3_session_login_ex is successfully executed.|
property|char*|The device message needs to be update, attribute name+ space bar+ attribute value|
successCb|SessionSuccessCb|Detailed in 2.2, can be null|
failedCb|SessionFailedCb|Detailed in 2.3, can be null|
retryMax|guint8|The maximum of repetition: 0~2|
userData|gpointer|Custom data, can be null|

#### Return

aca3_result_e，ACA3_OK means success, otherwise means failure.

### 2.20 aca3_session_getServerTime

Function prototype: aca3_result_e aca3_session_getServerTime(st_session* seesion, SessionServerTimeCb serverTimeCb, SessionFailedCb failedCb, gpointer userData)

### Usage

Get the server time, and it is the standard time.

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
session|st_session*|The session connection is sent after aca3_session_login/aca3_session_login_ex is successfully executed.|
serverTimeCb|SessionServerTimeCb|Detailed in 2.5|
failedCb|SessionFailedCb|Detailed in 2.3, can be null|
userData|gpointer|Custom data, can be null|

#### Return
aca3_result_e，ACA3_OK means success, otherwise means failure.
