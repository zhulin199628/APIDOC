# Axalent Cloud OAuth2 API手册

##  1.  OAuth2 Services API

本节主要介绍如何使用OAuth2 API来进行用户验证，并且取得OAuth2安全令牌。

###   1.1 authorize

 
#### Usage

获取OAuth2安全令牌前，需要验证用户信息、需要授权的第三方应用信息。用户信息包括name、password、appId(Axalent Cloud应用ID)。第三方应用信息包括client_id、redirect_uri、state(可选)。认证成功后，Axalent Cloud会返回302重定向，重定向地址中会包含参数uuid，需取得此uuid来调用authorizeCode API。

<strong>PS：</strong>被授权的第三方应用需要事先在Axalent Cloud进行注册，redirect_uri是在注册时由第三方提供，注册成功后会返回该应用的client_id和client_secret。

#### Parameters
|Parameter name|Description|
|----|----|
name|用户名|
password|用户密码|
appId|Axalent Cloud应用ID|
client_id|Axalent Cloud分配的第三方应用ID|
response_type|必须为“code”|
redirect_uri|第三方应用回调地址，须https|
state|可选，第三方应用验证|

#### Response

302重定向，包含参数uuid

#### Example

https://example.axalent.com/oauth2/authorize?name=allen&password=123456&appId=1001&client_id=1d974df93b8845fb9cb162e5ba937fcb&response_type=code&redirect_uri=https://thirdparty.com

302 &nbsp; &nbsp; https://example.axalent.com/authorize.html?uuid=03d011f1-4f31-4987-993e-24cfde39a125

###  1.2 authorizeCode

获取授权码

#### Usage

根据authorize获得的uuid来获取OAuth2授权码。

#### Parameters

|Parameter name|Description|
|----|----|
uuid|authorize获取到的uuid|

#### Response

302重定向，包含参数授权码code，如果认证时指定了state，也会包含参数state

#### Example

https://example.axalent.com/oauth2/authorizeCode?uuid=03d011f1-4f31-4987-993e-24cfde39a125

302 &nbsp; &nbsp;https://thirdparty.com?code=DkoF


###  1.3	accessToken

获取OAuth2安全令牌。

#### Usage

根据authorizeCode获取的授权码code获取OAuth2安全令牌access_token，或者使用refresh_token来刷新安全令牌。

#### 获取令牌Parameters

|Parameter name|Description|
|----|----|
grant_type|必须是“authorization_code”|
client_id|Axalent Cloud分配的第三方应用ID|
client_secret|Axalent Cloud分配的第三方应用secre|
code|AuthorizeCode获取到的授权码|
redirect_uri|第三方应用的回调地址|

#### 刷新令牌Parameters

|Parameter name|Description|
|----|----|
grant_type|必须是“refresh_token”|
client_id|Axalent Cloud分配的第三方应用ID|
client_secret|Axalent Cloud分配的第三方应用secret|
refresh_token|获取OAuth2令牌时返回的refresh_token|

#### Response

|Key|Value|
|----|----|
access_token|OAuth2安全令牌|
token_type|为“bearer”|
expires_in|30天，单位s|
refresh_token|用于刷新OAuth2安全令牌|

#### Example

https://example.axalent.com/oauth2/accessToken?grant_type=authorization_code&client_id=1d974df93b8845fb9cb162e5ba937fcb&client_secret=bad8336aefa64169be438c5e445c03d2&code=DkoF&redirect_uri=https://thirdparty.com

https://example.axalent.com/oauth2/accessToken?grant_type=authorization_code&client_id=1d974df93b8845fb9cb162e5ba937fcb&client_secret=bad8336aefa64169be438c5e445c03d2&refresh_token=54ebdfac-c260-42a8-9fdd-dbeb65bffd44

{
“access_token”: “2f91ece2-cb66-4b48-b72e-4db4c845c178”,
“token_type”: “bearer”,
“expires_in”: 2592000,
“refresh_token”: “54ebdfac-c260-42a8-9fdd-dbeb65bffd44”
}

##	2.	OAuth2 Web API

本节主要介绍使用OAuth2安全令牌调用Web API，目前开放的API如下，可按需开放新的API。

###	2.1	getDeviceTypeList

获取Axalent Cloud下所有的设备类型列表。

#### Usage

Axalent Cloud拥有众多设备类型，比如Light、Smoke等类型，通过此接口就能获取Axalent Cloud现拥有所有设备类型列表。

#### Parameters

|Parameter name|Description|
|----|----|
accessToken|OAuth2安全令牌|

#### Response

|Response name|Description|
|----|----|
typeNameList|Device ID<br>Device Name<br>Device Display Name|

#### Examples

https://example.axalent.com:8081/oauth2/services/zamapi/getDeviceTypeList?accessToken=2f91ece2-cb66-4b48-b72e-4db4c845c178

```
<ns1:getDeviceTypeListResponse xmlns:ns1="http://axalent.com/zamapi/">
	<typeNameList>
	    <id>123</id>
	    <name>Light</name>
		<displayName>Simple Light</displayName>
	</typeNameList>
</ns1:getDeviceTypeListResponse >

```

###	2.2	getDeviceList

获取当前Axalent Cloud账户下的所有设备列表

#### Usage

每个Axalent Cloud账户拥有众多设备，比如Light、Smoke等设备，通过此接口就能获取Axalent Cloud账户下有所有设备列表

#### Parameters

|Parameter name|Description|
|----|----|
accessToken|OAuth2安全令牌|
#### Response

|Response name|Description|
|----|----|
devList|Device ID<br>Device Name<br>Type ID<br>Time of last modification<br>设备权限(1 创建者，2 完全控制，包括分享、读、写设备，3 只能对设备进行读和写，4 只能读取设备信息，5 扩展，6 扩展)<br>presenceInfo 设备状态（1为在线、0为掉线）|

#### Examples

https://example.axalent.com:8081/oauth2/services/zamapi/getDeviceList?accessToken=2f91ece2-cb66-4b48-b72e-4db4c845c178

```
<ns1:getDeviceListResponse xmlns:ns1="http://axalent.com/zamapi/">
<devList>
	<devId>123</ devId>
	<devName>AXA00001</devName>
	<typeId>7</typeId>
	<ownership>1</ownership>
	<presenceInfo>Simple Light</presenceInfo>
		<lastModified>1499248614341</lastModified>
	</devList>
</ns1: getDeviceListResponse>
```


###	2.3	getDeviceAttributesWithValues

获取设备属性列表

#### Usage

获取Axalent Cloud账户下的设备多个属性值列表

#### Parameters

|Parameter name|Description|
|----|----|
accessToken|OAuth2安全令牌|
devId|Device ID|
deviceTypeId|Device Type ID|

#### Response

|Response name|Description|
|----|----|
typeId|Device Type ID|
typeName|Device Type Name|
presenceInfo|Device Status|
attrList|Attribute Name<br>Display Name<br>Device属性表示是否是设备属性，是则发送到硬件设备并保存到数据库，否则只保存到数据库(1 是，0 否)<br>ts 是否拥有历史记录(1 是，0否)。<br>tsValueType 属性的数据类型<br>value 属性值<br>updTime 属性的更新时间(Unix时间戳)|

#### Example

https://example.axalent.com:8081/oauth2/services/zamapi/getDeviceAttributesWithValues?accessToken=2f91ece2-cb66-4b48-b72e-4db4c845c178&devId=2132124&deviceTypeId=12

```
<ns1:getDeviceAttributesWithValuesResponse xmlns:ns1="http://axalent.com/zamapi/">
	<typeId>12</typeId>
	<typeName>1245</typeName>
	<presenceInfo>1</presceneInfo>
	<attrList>
		<name>phoneId</name>
		<diaplayName>phoneId</displayName>
		<device>0</device>
		<ts>0</ts>
		<tsValueType>1</tsValueType>
		<value>21312-343432-4324324-324324-654656/value>
		<updTime>1498191160542</updTime>
	</attrList>
</ns1:getDeviceAttributesWithValuesResponse>
```

###	2.4	getDeviceAttribute

获取设备属性

#### Usage

获取Axalent Cloud账户下的单个设备的单个属性的值

#### Parameters

|Parameter name|Description|
|----|----|
accessToken|OAuth2安全令牌|
devId|Device ID|
name|属性名称|

#### Response

|Response name|Description|
|----|----|
value|属性值|
updTime|最后更新时间|

#### Examples

https://example.axalent.com:8081/oauth2/services/zamapi/getDeviceAttribute?accessToken=2f91ece2-cb66-4b48-b72e-4db4c845c178&devId=2132124&name=open

```
<ns1:getDeviceAttributeResponse xmlns:ns1="http://axalent.com/zamapi/">
	<value>1</value>
	<updTime>1</updTime>
</ns1: getDeviceAttributeResponse>
```

###	2.5	setDeviceAttribute

设置设备属性

#### Usage

设置Axalent Cloud账户下的设备属性

#### Parameters


|Parameter name|Description|
|----|----|
accessToken|OAuth2安全令牌|
devId|设备ID|
typeId|Type ID|
name|属性名称|
value|属性值|

#### Response


|Response name|Description|
|----|----|
retCode|返回值，0表示成功|

#### Examples

https://example.axalent.com:8081/oauth2/services/zamapi/setDeviceAttribute?secToken=2002-23214343223&devId=234924&deviceTypeId=15&name=light&value=1

```
<ns1:setDeviceAttributeResponse xmlns:ns1="http://axalent.com/zamapi/">
	<retCode>0</retCode>
</ns1: setDeviceAttributeResponse>
```

