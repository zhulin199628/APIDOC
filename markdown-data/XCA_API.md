# Axalent Cloud XCA API手册

## 1. 简介

XCA是Axalent Cloud设备端的程序连接库，通常运行在资源有限的设备平台，接入单个设备节点，例如WIFI、GPRS、NB等可联网设备。占用资源少，可为多种不同平台编译程序连接库。  

## 2. XCA API

###  2.1 OnConnectionChangedCb

函数类型定义：typedef void (*OnConnectionChangedCb)(int status)

#### Usage

回调函数，当XCA和服务器连接状态发生变化时，此类回调函数会被调用。连接状态通过参数(int型)传出，连接成功：<strong>TRUE，</strong>断开连接：<strong>FALSE</strong>

#### Parameters
|Parameter name|Type|Description|
|----|----|----|
status|int|连接状态，连接成功：<strong>TRUE，</strong>断开连接：<strong>FALSE</strong|

#### Return
无

### 2.2 SessionSuccessCb

函数类型定义：typedef gpointer (*SessionSuccessCb)(gpointer data)

#### Usage

回调函数，aca3_session_login、aca3_session_login_ex、aca3_session_logout、aca3_session_sendProperty、aca3_session_sendPropertyWithOptions等函数都是异步的，函数返回值只是表明XCA接受了此次操作。后续XCA会和服务器通信，然后将结果通过回调函数的方式返回，此类回调函数被调用表示操作成功。data是用户自定义数据，在aca3_session_login、aca3_session_login_ex、aca3_session_logout、aca3_session_sendProperty、aca3_session_sendPropertyWithOptions等函数调用时传入。

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
data|gpointer|用户自定义数据，可为NULL|

#### Return

gpointer

###  2.3 SessionFailedCb

函数类型定义：typedef gpointer (*SessionFailedCb)(int errorCode, gpointer data)

#### Usage

回调函数，aca3_session_login、aca3_session_login_ex、aca3_session_logout、aca3_session_sendProperty、aca3_session_sendPropertyWithOptions等函数都是异步的，函数返回值只是表明XCA接收了此次操作。后续XCA会和服务器通信，然后将结果通过回调函数的方式返回，此类回调函数被调用表示操作失败。errorCode是错误码，data是用户自定义数据，在aca3_session_login、aca3_session_login_ex、aca3_session_logout、aca3_session_sendProperty、aca3_session_sendPropertyWithOptions等函数调用时传入。

#### Parameters
|Parameter name|Type|Description|
|----|----|----|
errorCode|int|错误码
data|gpointer|用户自定义数据，可为NULL|

#### Return

gpointer

### 2.4 SessionConnectionChangedCb

函数类型定义：typedef gpointer (*SessionConnectionChangedCb)(gboolean status, gpointer data)

#### Usage

回调函数，当设备和服务器的连接状态发生变化时，此函数会被调用。status连接状态，连接成功：TRUE，连接断开：FALSE。data是用户自定义数据，在aca3_session_login、aca3_session_login_ex调用时传入。

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
status|gboolean|连接状态，连接成功：<strong>TRUE，</strong>连接断开：<strong>FALSE</strong>|
data|gpointer|用户自定义数据，可为<strong>NULL</strong>|

#### Return

gpointer

### 2.5 SessionServerTimeCb

函数类型定义：typedef gpointer (*SessionServerTimeCb)(st_session_serverTime* serverTime, gpointer data)

#### Usage

回调函数，当从服务器请求时间成功之后，此函数会被调用。serverTime是返回的服务器时间结构体，data是用户自定义数据，在aca3_session_getServerTime函数调用时传入。

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
serverTime|st_session_serverTime*|时间结构体：<br>typedef struct{<br>guint16 year;<br>guint16 month;<br>guint16 day;<br>guint16 hour;<br>guint16 minute;<br>guint16 second;<br>}st_session_serverTime;|
data|gpointer|用户自定义数据，可为<strong>NULL</strong>|

#### Return

gpointer

### 2.6 SessionReceivedSingleProperty

函数类型定义：typedef void (*SessionReceivedSingleProperty)(guint32 childId, char* property, gpointer data)

#### Usage

回调函数，当XCA接收到服务器下发的消息时，此类回调函数会被调用。childId一般为0，property是消息，包含attribute name和attribute value两部分，使用空格隔开，data是用户自定义数据。

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
childId|guint32|一般为0|
property|char*|服务器下发的设备消息，attribute name+空格+attribute value|
data|gpointer|用户自定义数据，可为NULL|


#### Return
无

### 2.7 aca3_gw_setLb

函数原型：aca3_result_e aca3_gw_setLb(int index, const char* lb)

#### Usage

设置XCA连接的服务器域名或IP地址，需要设置3个(可以设置成一样的)，需在aca3_gw_init之前调用。

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
index|int|取值可以是0、1、2，其它值将返回错误|
lb|const char *|可以是域名，也可以是点分式IP地址|

#### Return

aca3_result_e，ACA3_OK表示成功，其它表示失败。

### 2.8aca3_gw_setTcpPort

函数原型：aca3_result_e aca3_gw_setTcpPort(guint16 port)

#### Usage

设置XCA连接的服务器TCP端口号，需在aca3_gw_init之前调用。

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
port|guint16|TCP端口号|

#### Return
aca3_result_e，ACA3_OK表示成功，其它表示失败。

### 2.9 aca3_gw_setUdpPort

函数原型：aca3_result_e aca3_gw_setUdpPort(guint16 port)

#### Usage

设置XCA连接的服务器UDP端口号，需在aca3_gw_init之前调用

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
port|guint16|UDP端口号|

#### Return

aca3_result_e，ACA3_OK表示成功，其它表示失败。

### 2.10 aca3_gw_setConnectionChangedListener

函数原型：void aca3_gw_setConnectionChangedListener(OnConnectionChangedCb statusChangedCb)

#### Usage

设置回调函数，当XCA和服务器的连接状态发生变化时，设置的函数会被调用，需在aca3_gw_init之前调用。

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
statusChangedCb|OnConnectionChangedCb|见2.1|

#### Return

aca3_result_e，ACA3_OK表示成功，其它表示失败。


### 2.11 aca3_gw_init

函数原型：aca3_result_e aca3_gw_init(void)

#### Usage
XCA初始化，与服务器建立连接。调用此函数后，XCA会按照上面API所设置的信息来连接服务器，连接成功后会通过设置的回调函数返回结果。

#### Parameters
 无

#### Return

aca3_result_e，ACA3_OK表示成功，其它表示失败。

### 2.12 aca3_session_login

函数原型：aca3_result_e aca3_session_login(st_session** session, char* code, char* pwd, guint16 productId, char* type, SessionConnectionChangedCb changedCb, gpointer userData)

#### Usage

XCA使用不加密的方式登录设备到服务器。调用此函数成功之后，XCA会使用不加密的方式将设备登录到服务器，并且维持设备和服务器的心跳连接。若设备异常掉线，XCA会自动将设备登录重新登录到服务器。

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
session|st_session**|会话连接|
code|char*|设备端code，每个设备都具有自己唯一的code|
pwd|char*|设备端password，每个设备都有自己的password|
productId|guint16|产品ID，和appID一样|
type|char*|设备类型|
changedCb|SessionConnectionChangedCb|见2.4|
userData|gpointer|用户自定义数据，可为<strong>NULL</strong>|

#### Return

aca3_result_e，ACA3_OK表示成功，其它表示失败。

### 2.13 aca3_session_login_ex

函数原型：aca3_result_e aca3_session_login_ex(st_session** session, char* code, char* pwd, char* aes, guint16 productId, char* productAes, char* type, gboolean isEncrypt, SessionConnectionChangedCb changedCb, gpointer userData)

####Usage

XCA使用可选(加密/不加密)的方式登录设备到服务器。调用此函数成功之后，XCA会使用可选(加密/不加密)的方式将设备登录到服务器，并且维持设备和服务器的心跳连接。若设备异常掉线，XCA会自动将设备登录重新登录到服务器。

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
session|st_session**|会话连接|
code|char*|设备端code，每个设备都具有自己唯一的code|
pwd|char*|设备端password，每个设备都有自己的password|
aes|char*|设备端加密device aes key|
productId|guint16|产品ID，和appID一样|
productAes|char*|设备端加密product aes key|
type|char*|设备类型|
isEncrypt|gboolean|是否加密|
changedCb|SessionConnectionChangedCb|见2.4|
userData|gpointer|用户自定义数据，可为<strong>NULL</strong>|

#### Return

aca3_result_e，ACA3_OK表示成功，其它表示失败。

### 2.14&nbsp;aca3_session_setReceivedSinglePropertyCb

函数原型：aca3_result_e aca3_session_setReceivedSinglePropertyCb(st_session* session, SessionReceivedSinglePropertyCb onReceivedPropertyCb, gpointer userData)

#### Usage

设置回调函数，当XCA接收到从服务器下发的消息时，此函数会被调用，将收到的消息通过参数传出。

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
session|st_session*|aca3_session_login/aca3_session_login_ex执行成功传出的会话连接|
onReceivedPropertyCb|SessionReceivedSinglePropertyCb|见2.6|
userData|gpointer|用户自定义数据，可为<strong>NULL</strong>|

#### Return

aca3_result_e，ACA3_OK表示成功，其它表示失败。

### 2.15 aca3_session_isLogin

函数原型：gooblean aca3_session_isLogin(st_session* session)

#### Usage

获取设备的在线状态

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
session|st_session*|aca3_session_login/aca3_session_login_ex执行成功传出的会话连接|

#### Return

gboolean，TRUE表示在线，FALSE表示离线。

### 2.16 aca3_session_getEndpointId

函数原型：gint32 aca3_session_getEndpointId(st_session* session)

#### Usage

获取设备在服务器的设备ID(在服务器的唯一标识)。

|Parameter name|Type|Description|
|----|----|----|
session|st_session*|aca3_session_login/aca3_session_login_ex执行成功传出的会话连接|

#### Return

gint32，设备在服务器的设备ID。

### 2.17 aca3_session_logout

函数原型：aca3_result_e aca3_session_logout(st_session* session, SessionSuccessCb successCb, SessionFailedCb failedCb, gpointer userData)

#### Usage

XCA从服务器将设备注销，设备的状态将变成离线。

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
session|st_session*|aca3_session_login/aca3_session_login_ex执行成功传出的会话连接|
successCb|SessionSuccessCb|见2.2，可为NULL|
failedCb|SessionFailedCb|见2.3，可为NULL|
userData|gpointer|用户自定义数据，可为NULL|

#### Return

aca3_result_e，ACA3_OK表示成功，其它表示失败。

### 2.18 aca3_session_sendProperty

函数原型：aca3_result_e aca3_session_sendProperty(st_session* session, char* property, SessionSuccessCb successCb, SessionFailedCb failedCb, gpointer userData)

#### Usage

发送一条消息到服务器，消息由attribute name和attribute value两部分组成，中间使用空格隔开。例如上传灯的亮度(亮度为70)，property应该是“dimmer 70”。

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
session|st_session*|aca3_session_login/aca3_session_login_ex执行成功传出的会话连接|
property|char*|要上传的设备消息，attribute name+空格+attribute value|
successCb|SessionSuccessCb|见2.2，可为NULL|
failedCb|SessionFailedCb|见2.3，可为NULL|
userData|gpointer|用户自定义数据，可为NULL|

#### Return

aca3_result_e，ACA3_OK表示成功，其它表示失败。

### 2.19 aca3_session_sendPropertyWithOptions

函数原型：aca3_result_e aca3_session_sendPropertyWithOptions(st_session* session, char* property, SessionSuccessCb successCb, SessionFailedCb failedCb, guint8 retryMax, gpointer userData)

#### Usage

发送一条消息到服务器，消息由attribute name和attribute value两部分组成，中间使用空格隔开。例如上传灯的亮度(亮度为70)，property应该是“dimmer 70”。
PS：与aca3_session_sendProperty的区别，当XCA发送设备消息到服务器时，如果由于异常，消息发送失败，那么XCA会在几秒钟之后重新进行发送。默认最大重试次数为2(即最多发送3次，成功/失败回调函数就会执行)，如果不进行重发可以设置为0。如果设置的最大重试次数大于2，XCA会自动更正为2。

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
session|st_session*|aca3_session_login/aca3_session_login_ex执行成功传出的会话连接|
property|char*|要上传的设备消息，attribute name+空格+attribute value|
successCb|SessionSuccessCb|见2.2，可为NULL|
failedCb|SessionFailedCb|见2.3，可为NULL|
retryMax|guint8|最大重试次数，值为0 ~ 2|
userData|gpointer|用户自定义数据，可为NULL|

####Return

aca3_result_e，ACA3_OK表示成功，其它表示失败。

### 2.20 aca3_session_getServerTime

函数原型：aca3_result_e aca3_session_getServerTime(st_session* seesion, SessionServerTimeCb serverTimeCb, SessionFailedCb failedCb, gpointer userData)

### Usage

获取服务器时间，此时间为标准时间。

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
session|st_session*|aca3_session_login/aca3_session_login_ex执行成功传出的会话连接|
serverTimeCb|SessionServerTimeCb|见2.5|
failedCb|SessionFailedCb|见2.3，可为NULL|
userData|gpointer|用户自定义数据，可为NULL|

#### Return
aca3_result_e，ACA3_OK表示成功，其它表示失败。
