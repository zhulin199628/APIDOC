# Axalent Cloud XGM API手册

## 1.  typedef int (*GwConnectCallbackFun)(gboolean ConnectStatus)

定义回调函数形式，当Gateway与服务器连接状态发生改变时，此回调函数会被调用，可参照如下定义：

<strong>int GwConnectCallback</strong>(gboolean ConnectStatus)

使用AxInit函数将此回调函数函数传递给XGM，当Gateway成功登陆服务器时，此函数会被调用。ConnectStatus是连接状态，成功：True，失败：False。
AxLogin传入的gpointer (*connectionChangedCb)(gboolean status, gpointer data)也是同样的用法。

## 2.  typedef int (*AxMsgCallbackFun)(t_deviceId* , const char* , const char* )

定义回调函数形式，当XGM接收消息属性时，此回调函数会被调用，可参照如下定义：

<strong>int AxMsgCallback</strong>(t_deviceId* devId, const char* strName, const char* strValue)

使用AxInit函数将此函数传递给XGM，当XGM接收到从服务器下发的属性，此函数会被调用。devId是本地区分子设备的唯一标识，在AxLogin时指定。如果devId是NULL，那么表示这是Gateway属性。strName是Attribute Name，strValue是Attribute Value。
注意：一般地，devId为NULL，表明是Gateway属性，devId不为NULL，表明是子设备属性。但是“deletedevice”属性比较特殊，当接收到deletedevice属性，不管devId为何值，这都是Gateway属性，devId的值用来表明要删除哪一个子设备，devId在AxLogin时传入。
当XGM接收到permitjoin属性，会通过回调函数将permitjoin这个attribute和attribute值传递出来，开发者然后进行本地添加设备的操作，将成功添加的设备使用AxLogin函数登录到服务器。如果XGM接收到permitjoin属性，并没有调用AxLogin函数，服务器上是不会添加这个新设备的。
当XGM接收到deletedevice属性，会通过回调函数将deletedevice这个attribute和attribute值传递出来，开发者然后进行本地删除设备的操作，将成功删除的设备使用AxDelete函数从服务器删除。如果XGM接收到deletedevice属性，并没有调用AxDelete函数，服务器是不会删除设备的。

## 3.  typedef gpointer (*UserSuccessCb)(gpointer data)

定义回调函数形式，AxLogin、AxLogout、AxDelete、AxSetProperty等函数都是异步的，函数返回值只是表示XGM是否接收此次操作。后续由XGM和服务器通信来完成操作，操作结果通过回调函数返回。成功回调函数可参照如下定义：
<strong>gpointer UserSuccessCb</strong>(gpointer data)

使用AxLogin、AxLogout、AxDelete、AxSetProperty函数将最后一个参数gpointer data用户定义数据传给XGM，当这些操作成功的时候，此函数会被调用。data是用户定义数据，在AxLogin、AxLogout、AxDelete、AxSetProperty传入，在回调函数中返回

## 4.  typedefgpointer(*UserServerTimeCb(axa_timestamp*serverTime,gpointer data)

定义AxGetTime成功回调函数形式，此回调函数可参照如下定义：
<strong>gpointer UserServerTimeSuccessCb</strong>(axa_timestamp serverTime, gpointer data)
使用AxGetTime函数会将最后一个参数gpointer data用户定义数据传给XGM，当函数执行成功的时候，此函数会被调用。serverTime是函数返回的时间结构体，可以从中得到当前服务器的格林威治时间。data是用户定义数据，在AxGetTime传入，在回调函数中返回。

## 5.  typedef gpointer (*UserFailedCb)(int errorCode, gpointer data)

定义回调函数形式，AxLogin、AxLogout、AxDelete、AxSetProperty等函数都是异步的，函数返回值只是表示XGM是否接收此次操作。后续由XGM和服务器通信来完成操作，操作结果通过回调函数返回。失败回调函数可参照如下定义：
<strong>gpointer UserFailedCb</strong>(int errorCode, gpointer data)
使用AxLogin、AxLogout、AxDelete、AxSetProperty、AxGetTime函数会将最后一个参数gpointer data用户定义数据传给XGM，当这些函数执行失败的时候，此函数会被调用。errorCode是返回的错误码，data是用户定义数据，在AxLogin、AxLogout、AxDelete、AxSetProperty、AxGetTime传入，在回调函数中返回。

## 6.  6axa_result_eAxInit(AxMsgCallbackFuncpMsgCb,GwConnectcallbackFun pGwConnectCb,const char *workDir)

@function
开始运行XGM，XGM会导入配置文件gw_config.xml中的配置信息，将Gateway登陆到Axalent Cloud，并且和Axalent Cloud保持连接。这个函数应该在程序最开始被调用。
@param  pMsgCb
回调函数，函数形式见上方4.2，当Gateway从Axalent Cloud接收到数据时，此函数会被调用。此函数开发者可以参考example.c代码，根据自己需要的功能去具体实现。
@param  pGwConnectCb
回调函数，函数形式见上方4.1，当Gateway成功登录到Axalent Cloud时，此函数会被调用。
@param  workDir
配置文件路径，此参数主要是用来指定gw_config.xml配置文件的路径。如果配置文件和可执行文件在同一目录，参数应该设置为“./”。若在指定的路径中找不到配置文件，程序运行会报错。
@return
成功返回0（AXA_SUCCESS），失败返回-1（AXA_FAILURE)。

## 7.  axa_result_eAxLogin(t_deviceId*devId,char*type,connectionChangedCb changedCb, gpointer data)

@function
通过devId和type登陆一个子设备。它会告诉Axalent Cloud一个新的子设备加入，并且将它的状态变为online。
@param  devId
传入子设备的唯一ID(本地ID)，这个ID最大支持64位，由开发者自己定义。一定要记得这个devId，后面的AxSetProperty和pMsgCallback会使用此ID作为判断哪一个子设备的唯一标识。
@param  type
子设备的类型，是一个字符串，目前服务器有gateway，SL，SSmoke等常用类型。
@param  changedCb
传入一个函数，当设备状态改变之后，此函数会执行。也可以传入NULL。
@param  data
传入数据，成功和失败的回调函数会将数据返回。也可以传入NULL。
@return
AXA_SUCCESS表示XGM成功接受子设备登录的操作，设备登录状态改变会通过回调函数传回(若状态回调函数返回设备异常掉线，XGM会自动重新登录设备，开发者无需再次AxLogin同一个子设备)。其它返回值则需要重新调用此函数进行登录。

##  8. axa_result_eAxLogout(t_deviceId*devId,UserSuccessCbsuccessCb,UserFailedCb failedCb, gpointer data)

@function
通过devId注销一个子设备，此devId是AxLogin时传入的。这个子设备还可以使用<strong>AxLogin</strong>再次登录。如果要删除一个子设备，请使用<strong>AxDelete</strong>函数。AxLogout和AxDelete的区别：<strong>AxLogout</strong>>只是将该设备的状态变为离线，并不会从服务器中删除此设备。而<strong>AxDelete</strong>不仅将该设备的状态变为离线，同时从服务器中删除此设备。
@param  devId
子设备的唯一ID，AxLogin函数传入。
@param  successCb
传入一个函数，当设备成功注销之后，此函数会执行。也可以传入NULL。
@param  failedCb
传入一个函数，当设备注销失败之后，此函数会执行。也可以传入NULL。
@param  data
传入数据，成功和失败的回调函数会将数据返回。也可以传入NULL。
@return
AXA_SUCCESS表示XGM接受请求，结果会通过回调函数返回。其它返回值表示失败。

## 9. axa_result_eAxDelete(t_deviceId*devId,UserSuccessCbsuccessCb,UserFailedCb failedCb, gpointer data)

@function
通过devId删除一个子设备。如果一个子设备从本地网络中删除，需调用这个函数告诉Axalent Cloud删除这个子设备。如果子设备只是想要offline，请使用<strong>AxLogout</strong>函数。此子设备想要再次加入网络，请使用<strong>AxLogin</strong>函数。
@param  devId
子设备的唯一ID，AxLogin函数传入。
@param  successCb
传入一个函数，当设备删除成功之后，此函数会执行。也可以传入NULL。
@param  failedCb
传入一个函数，当设备删除失败之后，此函数会执行。也可以传入NULL。
@param  data
传入数据，成功和失败的回调函数会将数据返回。也可以传入NULL。
@return
AXA_SUCCESS表示XGM接受请求，处理结果会在回调函数中返回。其他值表示失败。

## 10. axa_result_eAxSetProperty(t_deviceId*devId,char*property,UserSuccessCb successCb, UserFailedCb failedCb, gpointer data)

@function
发送一个子设备属性信息到Axalent Cloud。
@param  devId
子设备的唯一ID，AxLogin函数传入。
@param  property
字符串。字符串的形式为“属性 属性值”，中间使用空格隔开。例如：字符串“light 1”。
@param  successCb
传入一个函数，当属性成功发送之后，此函数会执行。也可以传入NULL。
@param  failedCb
传入一个函数，当属性发送失败之后，此函数会执行。也可以传入NULL。
@param  data
传入数据，成功和失败的回调函数会将数据返回。也可以传入NULL。
@return
AXA_SUCCESS表示XGM接受请求，结果会在回调函数中返回。其他返回值表示失败。

## 11. axa_result_eAxGetTime(axa_timestamp_t*,int16_ttimezone,userServerTimeCb successCb, UserFailedCb failedCb, gpointer data)

@function
从Axalent Cloud获得当前的时间。
@param  timestamp
时间结构体指针
@param  timezone
相对 GMT 的时区，单位：小时
@param  successCb
传入一个函数，当获取时间成功之后，此函数会执行。也可以传入NULL。
@param  failedCb
传入一个函数，当获取时间失败之后，此函数会执行。也可以传入NULL。
@param  data
传入数据，成功和失败的回调函数会将数据返回。也可以传入NULL。
@return
AXA_SUCCESS表示XGM接受请求，处理结果会在回调函数中返回。其它返回值表示失败。
