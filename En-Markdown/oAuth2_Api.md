#  Axalent Cloud OAuth2 API Manual

##  1.  OAuth2 Services API

In this section, you will know how to use the OAuth2 API for user authentication and how to get an OAuth2 access token.

###   1.1 authorize

Verify user information and third-part application information that requires authorization。
 
#### Usage

Before getting the OAuth2 access token, the user information and authorized 3rd-part application information are required. The user information includes: Name, password, appId (Axalent Cloud App ID). The 3rd-part application information includes: client_id, redirect_uri, state (optional). After the authentication is successful, Axalent Cloud will return to the address of 302 redirects, the redirect address will contain the parameter uuid. To debug the authorizeCode API, the uuid is required. 

<strong>PS：</strong>Authorized third-party applications need to be registered in Axalent Cloud beforehand, Redirect_uri is provided by a 3rd-part at the time of registration after registration, the client_id and client_secret of app will be returned. 

#### Parameters
|Parameter name|Description|
|----|----|
name|User Name|
password|User Password|
appId|Axalent Cloud App ID|
client_id|3rd-part App ID assigned by Axalent Cloud|
response_type|Must be“code”|
redirect_uri|3rd-part App callback address, must be “https”|
state|Optional, 3rd-part App verification|

#### Response

302 redirect, including parameter uuid.

#### Example

https://example.axalent.com/oauth2/authorize?name=allen&password=123456&appId=1001&client_id=1d974df93b8845fb9cb162e5ba937fcb&response_type=code&redirect_uri=https://thirdparty.com

302 &nbsp; &nbsp; https://example.axalent.com/authorize.html?uuid=03d011f1-4f31-4987-993e-24cfde39a125

###  1.2 authorizeCode

Get authorize code.

#### Usage

Getting the authorize code of OAuth2 according to uuid from authorization.

#### Parameters

|Parameter name|Description|
|----|----|
uuid|Get the uuid from authorization|

#### Response

302 redirect, including the parameter authorize code. If state is specified during authentication, the parameter state will also be included.

#### Example

https://example.axalent.com/oauth2/authorizeCode?uuid=03d011f1-4f31-4987-993e-24cfde39a125

302 &nbsp; &nbsp;https://thirdparty.com?code=DkoF


###  1.3	accessToken

Get the access token of OAuth2.

#### Usage

To get the access token of OAuth2, Authorization code obtained according to authorizeCode. 

#### 获取令牌Parameters

|Parameter name|Description|
|----|----|
grant_type|Must be “authorization_code”|
client_id|3rd-part APP ID assigned by Axalent Cloud|
client_secret|3rd-part secret assigned by Axalent Cloud|
code|The authorized code gets from AuthorizeCode |
redirect_uri|The redirect address of 3rd-part APP |

#### Update the token parameters

|Parameter name|Description|
|----|----|
grant_type|Must be “refresh_token”|
client_id|3rd-part APP ID assigned by Axalent Cloud|
client_secret|3rd-part secret assigned by Axalent Cloud|
refresh_token|The returned refresh_token getting the OAuth2 token|

#### Response

|Key|Value|
|----|----|
access_token|OAuth2 access token|
token_type|For “bearer”|
expires_in|30days, unit: s|
refresh_token|To refresh the OAuth2 access token|

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

In this section, you will know how to use OAuth2 access token to transfer the Web API. The currently open APIs are as follows, and new can be opened as needed.

###	2.1	getDeviceTypeList

To get all device lists under the Axalent Cloud.

#### Usage

Axalent Cloud has many variety types of device, like: Light, Smoke and so on. This interface provides a list of all device types that Axalent Cloud now has.

#### Parameters

|Parameter name|Description|
|----|----|
accessToken|OAuth2 access token|

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

To get all device lists under the current account of Axalent Cloud

#### Usage

Every Axalent Cloud account has abound of devices, like: Light, Smoke and so on. This interface provides a list of all device that Axalent Cloud now has.

#### Parameters

|Parameter name|Description|
|----|----|
accessToken|OAuth2 access token|
#### Response

|Response name|Description|
|----|----|
devList|Device ID<br>Device Name<br>Type ID<br>Time of last modification<br>Device Authorization(1 Creator 2 Full control, including sharing, reading, writing 3 Can only read and write to the device 4 Can only read device information，5 Extend，6 Expansion)<br>presenceInfo Device status（1 means online、0 means offline）|

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

To get the device attributes list.
#### Usage

To geta list of multiple attribute values for devices under the Axalent Cloud account

#### Parameters

|Parameter name|Description|
|----|----|
accessToken|OAuth2 access token|
devId|Device ID|
deviceTypeId|Device Type ID|

#### Response

|Response name|Description|
|----|----|
typeId|Device Type ID|
typeName|Device Type Name|
presenceInfo|Device Status|
attrList|Attribute Name<br>Display Name<br>Device attribute: Indicates whether it is a device attribute. If yes, it will send to the hardware device and saved to the database, otherwise it will only be saved to database. (1 means yes，0 means no)<br>Ts: Whether it has history record or not.(1 means yes，0 means no).<br>tsValueType: The data type of the attribute.<br>Value: Attribute value<br>updTime: The update time of attributes(UnixTimestamp)|

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

To get device attribute

#### Usage

To get the a single attribute value of a single device under the account of Axalent Cloud

#### Parameters

|Parameter name|Description|
|----|----|
accessToken|OAuth2 access token|
devId|Device ID|
name|Attribute Name|

#### Response

|Response name|Description|
|----|----|
value|Attribute value|
updTime|Last update time|

#### Examples

https://example.axalent.com:8081/oauth2/services/zamapi/getDeviceAttribute?accessToken=2f91ece2-cb66-4b48-b72e-4db4c845c178&devId=2132124&name=open

```
<ns1:getDeviceAttributeResponse xmlns:ns1="http://axalent.com/zamapi/">
	<value>1</value>
	<updTime>1</updTime>
</ns1: getDeviceAttributeResponse>
```

###	2.5	setDeviceAttribute

To set the device attribute
#### Usage

To set the device attribute under the account of Axalent Cloud

#### Parameters


|Parameter name|Description|
|----|----|
accessToken|OAuth2 access token|
devId|Device ID|
typeId|Type ID|
name|Attribute name|
value|Attribute value|

#### Response


|Response name|Description|
|----|----|
retCode|Redirect code, 0 means ok|

#### Examples

https://example.axalent.com:8081/oauth2/services/zamapi/setDeviceAttribute?secToken=2002-23214343223&devId=234924&deviceTypeId=15&name=light&value=1

```
<ns1:setDeviceAttributeResponse xmlns:ns1="http://axalent.com/zamapi/">
	<retCode>0</retCode>
</ns1: setDeviceAttributeResponse>
```

