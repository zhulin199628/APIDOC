# Axalent Cloud XGM API手册

## 1. 简介

XGM是Axalent Cloud设备端的连接程序库，使用XGM可以将网关类设备接入到Axalent Cloud，并且将网关下的子设备(蓝牙、zigbee、z-wave等)接入Axalent Cloud。XGM适用于资源较丰富、需要接入子设备的网关类设备。

## 2. XGM API

### 2.1 AxMsgCallbackFun

函数类型定义：typedef int (*AxMsgCallbackFun)(t_deviceId* devId, const char* strName, const char* strValue)

#### Usage

定义回调函数形式，当XGM接收消息属性时，此回调函数会被调用，可参照如下定义：
int AxMsgCallback(t_deviceId* devId, const char* strName, const char* strValue)
使用AxInit函数将此函数传递给XGM，当XGM接收到从服务器下发的属性，此函数会被调用。devId是本地区分子设备的唯一标识，在AxLogin时指定。如果devId是NULL，那么表示这是Gateway属性。strName是Attribute Name，strValue是Attribute Value。
注意：一般地，devId为NULL，表明是Gateway属性，devId不为NULL，表明是子设备属性。但是“deletedevice”属性比较特殊，当接收到deletedevice属性，不管devId为何值，这都是Gateway属性，devId的值用来表明要删除哪一个子设备，devId在AxLogin时传入。
当XGM接收到permitjoin属性，会通过回调函数将permitjoin这个attribute和attribute值传递出来，开发者然后进行本地添加设备的操作，将成功添加的设备使用AxLogin函数登录到服务器。如果XGM接收到permitjoin属性，并没有调用AxLogin函数，服务器上是不会添加这个新设备的。
当XGM接收到deletedevice属性，会通过回调函数将deletedevice这个attribute和attribute值传递出来，开发者然后进行本地删除设备的操作，将成功删除的设备使用AxDelete函数从服务器删除。如果XGM接收到deletedevice属性，并没有调用AxDelete函数，服务器是不会删除设备的。

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
devId|t_deviceId*|设备本地ID，调用AxLogin函数时传入。若是网关设备，devId为NULL|
strName|const char*|接收到的attribute name|
strValue|const char*|接收到的attribute value|

#### Return

int,一般为0

### 2.2. GwConnectCallbackFun

函数类型定义：typedef int (*GwConnectCallbackFun)(gboolean ConnectStatus)

#### Usage

定义回调函数形式，当Gateway与服务器连接状态发生改变时，此类回调函数会被调用，可参照如下定义：
int GwConnectCallback(gboolean ConnectStatus)
使用AxInit函数将此回调函数传递给XGM，当Gateway成功登陆服务器时，此函数会被调用。ConnectStatus是连接状态，成功：True，失败：False。

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
ConnectStatus|gboolean|连接状态，成功：True，失败：False|

#### Return
int

### 2.3 connectionChangedCb

函数类型定义：typedef gpointer (*connectionChangedCb)(gboolean status, gpointer data)

#### Usage
定义回调函数形式，当子设备和服务器的连接状态发生改变时，此类回调函数会被调用，可参照如下定义：
gpointer connectionChangedCb(gboolean status, gpointer data)

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
status|gboolean|子设备连接状态，连接成功：True，连接断开：False|
data|gpointer|用户自定义数据，可为NULL|

#### Return

gpointer

### 2.4 UserSuccessCb

函数类型定义：typedef gpointer (*UserSuccessCb)(gpointer data)

#### Usage
定义回调函数形式，AxLogin、AxLogout、AxDelete、AxSetProperty等函数都是异步的，函数返回值只是表示XGM是否接收此次操作。后续由XGM和服务器通信来完成操作，操作结果通过此类回调函数返回。成功回调函数可参照如下定义：
gpointer UserSuccessCb(gpointer data)
使用AxLogin、AxLogout、AxDelete、AxSetProperty函数将最后一个参数gpointer data（用户定义数据）传给XGM，当这些操作成功的时候，此函数会被调用。data是用户定义数据，在AxLogin、AxLogout、AxDelete、AxSetProperty传入，在回调函数中返回。

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
data|gpointer|用户自定义数据，可为NULL|

#### Return
gpointer

### 2.5 UserServerTimeCb

函数类型定义：typedef gpointer (*UserServerTimeCb)(axa_timestamp* serverTime, gpointer data)

#### Usage
定义AxGetTime成功的回调函数的形式，此回调函数可参照如下定义：
gpointer UserServerTimeSuccessCb(axa_timestamp serverTime, gpointer data)
使用AxGetTime函数会将最后一个参数gpointer data用户定义数据传给XGM，当函数执行成功的时候，此函数会被调用。serverTime是函数返回的时间结构体，可以从中得到当前服务器的格林威治时间。data是用户定义数据，在AxGetTime传入，在回调函数中返回。

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
serverTime|axa_timestamp|时间结构体：<br>typedef struct{<br>uint16_t year;<br>uint16_t month;<br>uint16_t day;<br>uint16_t hour;<br>uint16_t minute;<br>uint16_t second;<br>}axa_timestamp_t;|
data|gpointer|gpointer|

#### Return
gpointer

### 2.6 UserFailedCb

函数类型定义：typedef gpointer (*UserFailedCb)(int errorCode, gpointer data)

#### Usage

定义回调函数形式，AxLogin、AxLogout、AxDelete、AxSetProperty等函数都是异步的，函数返回值只是表示XGM是否接收此次操作。后续由XGM和服务器通信来完成操作，操作结果通过回调函数返回。失败回调函数可参照如下定义：
gpointer UserFailedCb(int errorCode, gpointer data)
使用AxLogin、AxLogout、AxDelete、AxSetProperty、AxGetTime函数会将最后一个参数gpointer data用户定义数据传给XGM，当这些函数执行失败的时候，此函数会被调用。errorCode是返回的错误码，data是用户定义数据，在AxLogin、AxLogout、AxDelete、AxSetProperty、AxGetTime传入，在回调函数中返回。


#### Parameters

|Parameter name|Type|Description|
|----|----|----|
errorCode|errorCode|错误码|
data|gpointer|用户自定义数据，可为NULL|

#### Return

gponiter

### 2.7 AxInit

函数原型：axa_result_e AxInit(AxMsgCallbackFunc pMsgCb, GwConnectcallbackFun pGwConnectCb,const char * workDir)

#### Usage
开始运行XGM，XGM会导入配置文件gw_config.xml中的配置信息，将Gateway登陆到Axalent Cloud，并且和Axalent Cloud保持连接。这个函数应该在程序最开始被调用。


#### Parameters

|Parameter name|Type|Description|
|----|----|----|
pMsgCb|AxMsgCallbackFunc|见2.1|
pGwConnectCb|GwConnectcallbackFun|见2.2|
workDir|const char *|指定gw_config.xml配置文件的路径，如果配置文件和可执行文件在同一目录，参数应该设置为“./”。若在指定的路径中找不到配置文件，程序运行会报错。|

#### Return

成功返回0（AXA_SUCCESS），失败返回-1（AXA_FAILURE)。

### 2.8 AxLogin

函数原型：axa_result_e AxLogin(t_deviceId* devId, char* type, connectionChangedCb changedCb, gpointer data)

#### Usage

通过devId和type登陆一个子设备。它会告诉Axalent Cloud一个新的子设备加入，并且将它的状态变为online。函数调用时需传入子设备的唯一ID(本地ID)，这个ID最大支持64位，由开发者自己定义。一定要记得这个devId，后面的AxSetProperty和pMsgCallback会使用此ID作为判断哪一个子设备的唯一标识。设备登录状态改变会通过回调函数传回(若状态回调函数返回设备异常掉线，XGM会自动重新登录设备，开发者无需再次AxLogin同一个子设备)。

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
devId|t_deviceId*|设备本地ID|
type|char*|设备类型，需先在服务器定义|
changedCb|connectionChangedCb|见2.3，可为NULL|
data|gpointer|用户自定义数据，可为NULL|

#### Return

AXA_SUCCESS表示XGM成功接受子设备登录的操作，其它返回值则需要重新调用此函数进行登录。

### 2.9 AxLogout

函数原型：axa_result_e AxLogin(t_deviceId* devId, char* type, connectionChangedCb changedCb, gpointer data)

#### Usage

通过devId注销一个子设备，此devId是AxLogin时传入的。这个子设备还可以使用**AxLogin**再次登录。如果要删除一个子设备，请使用**AxDelete**函数。**AxLogout**和**AxDelete**的区别：**AxLogout**只是将该设备的状态变为离线，并不会从服务器中删除此设备。而**AxDelete**不仅将该设备的状态变为离线，同时从服务器中删除此设备。

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
devId|t_deviceId*|设备本地ID|
successCb|UserSuccessCb|见2.4，可为NULL|
failedCb|UserFailedCb|见2.6，可为NULL|
data|gpointer|用户自定义数据，可为NULL|

#### Return

AXA_SUCCESS表示XGM接受请求，结果会通过回调函数返回。其它返回值表示失败。


### 2.10 AxDelete

函数原型：axa_result_e AxDelete(t_deviceId* devId, UserSuccessCb successCb, UserFailedCb failedCb, gpointer data)

#### Usage
通过devId删除一个子设备。如果一个子设备从本地网络中删除，需调用这个函数告诉Axalent Cloud删除这个子设备。如果子设备只是想要offline，请使用**AxLogout**函数。此子设备想要再次加入网络，请使用**AxLogin**函数。

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
devId|t_deviceId*|设备本地ID|
successCb|UserSuccessCb|见2.4，可为NULL|
failedCb|UserFailedCb|见2.6，可为NULL|
data|gpointer|用户自定义数据，可为NULL|

####Return

AXA_SUCCESS表示XGM接受请求，处理结果会在回调函数中返回。其他值表示失败。

### 2.11 AxSetProperty

函数原型：axa_result_e AxSetProperty(t_deviceId* devId, char* property, UserSuccessCb successCb, UserFailedCb failedCb, gpointer data)

#### Usage

发送一条设备属性信息到Axalent Cloud。设备属性有attribute name和attribute value两部分组成，中间使用空格隔开。若要发送的的设备属性为网关属性，devId应为NULL。


#### Parameters

|Parameter name|Type|Description|
|----|----|----|
devId|t_deviceId*|设备本地ID，网关设备为NULL|
property|char*|attribute name+空格+attribute value|
successCb|UserSuccessCb|见2.4，可为NULL|
failedCb|UserFailedCb|见2.6，可为NULL|
data|gpointer|用户自定义数据，可为NULL|

#### Return

AXA_SUCCESS表示XGM接受请求，结果会在回调函数中返回。其他返回值表示失败。

### 2.12 AxGetTime

函数原型：axa_result_e AxGetTime(userServerTimeCb successCb, UserFailedCb failedCb, gpointer data)

### Usage

从Axalent Cloud获得当前的时间，标准时间。获取时间成功，将通过回调函数返回。

#### Parameters

|Parameter name|Type|Description|
|----|----|----|
successCb|userServerTimeCb|见2.5|
failedCb|UserFailedCb|见2.6，可为NULL|
data|gpointer|用户自定义数据，可为NULL|

#### Return

axa_result_e，AXA_SUCCESS表示XGM接受请求，处理结果会在回调函数中返回。其它返回值表示失败
