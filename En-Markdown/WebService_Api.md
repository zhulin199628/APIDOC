# Axalent Cloud API 

## 1. Introduction
This document lists Axalent cloud Application Programming Interfaces (APIs) and provides detailed explanation of each function and required parameters.
There are 3 types of account with different privileges in the Axalent cloud:
A)Axalentsuper administrator: currently reserved for Axalent personnel only.  Axalentsuper adminiatrator has the privilege to create and remove appIDs and appID administrators.
B)AppID system administrator: the highest-privileged account type in an operational cloud system deployed for Axalent’s customer.  This level has full access, control and user management privileges within an appID.  The Axalent cloud is a multi-tenant cloud infrastructure.  An appID identifies a tenant within this multi-tenant infrastructure.
C)User: users of devices and services provided by the Axalent cloud.  A user has full access, configuration, control, monitor and management privilege of devices and services claimed by his/her user account.


## 2. General APIs

This chapter describes the available APIs to all account types.


### 2.1 userLogin

Account login.

#### Usage

To access the Axalent cloud, your app will need this API, 2 parameters will be returned when the call succeeds: userid and securityToken.  Userid is the ID code of current user.  securityToken is the secure access token, Axalent cloud uses this token to verify every request to see if it comes from the user.  The validity period of each securityToken is 15 minutes, so you will need to re-login to obtain a new token every 15 minutes, and the older token would become invalid.

#### Parameters

|Parameter name|Description|
|----|----|
name|User Name|
password|Password|
appId|APP ID|

#### Response
|Response  name|Description|
|----|----|
userId|User ID|
securityToken|Security Token|

#### Examples
https://example.axalent.com:8081/zdk/services/zamapi/userLogin?name=jason&password=123456&appId=2002
```
<ns1:userLoginResponse xmlns:ns1="http://axalent.com/zamapi/">
	<userId>42</userId>
	<securityToken>897345hjaf234ggagdse25345</securityToken>
</ns1:userLoginResponse>
```
### 2.2 registerUser

Regsiter new user.
 
#### Usage

To register an account for Axalent Cloud, you can use this API.  It uses a user-provided phone number and use getVerificationCode API to send the verification code for registration via SMS to user’s phone.  User then uses the verification code to complete the registration.

#### Parameters
|Parameter name|Description|
|----|----|
appId|APP ID|
phone|Phone Numbe|
code|Verification Code|
username|User Name|
password|Password|


#### Response
|Response  name|Description|
|----|----|
userId|User ID|

#### Examples
https://example.axalent.com:8081/zdk/services/zamapi/registerUser?appId=2002&phone=15690239434&code=200223&username=jason1&password=123456
```
<ns1:registerUserResponse xmlns:ns1="http://axalent.com/zamapi/">
	<userId>42</userId>
</ns1: registerUserResponse>
```
### 2.3 getVerificationCode
Get the verification code and send to via SMS to user-specified phone number.

#### Usage

There are 2 different verification codes returned by this API: 1. The SMS verification code when registering a new user, 2. The SMS verification code for recovering a forgotten user password.  Axalent cloud sends the corresponding verification code to a user-specified phone number.  User can then enter this verification code into the corresponding app interface to finish the operation.

#### Parameters

|Parameter name|Description|
|----|----|
appId|APP ID|
phone|Phone Numbe|
type|Registration,Recover password|

#### Response
|Response  name|Description|
----| ----|
retCode|Ret Code(0 means ok)|

#### Examples
https://example.axalent.com:8081/zdk/services/zamapi/getVerificationCode?appId=2002&phone=15690239434&type=registration

```
<ns1:getVerificationResponse xmlns:ns1="http://axalent.com/zamapi/">
	<retCode>0</ retCode>
</ns1: getVerificationResponse>
```

### 2.4 recoverUserPassword  
Recover user password.
#### Usage
When the user has forgotten his/her Axalent cloud account password, first send a verification code to user’s phone number using getVerificationCode.  Then use this verification code and user’s phone number in recoverUserPassword API to allow the user to set a new password for his/her account.

#### Parameters
|Parameter name|Description|
|----|----|
appId|APP ID|
code|SMS Verification Code|
phone|Phone Number|
password|New Password|

#### Response
|Response  name|Description|
|----|----|
retCode|Ret Code(0 means ok)|

#### Examples
https://example.axalent.com:8081/zdk/services/zamapi/recoverUserPassword?appId=2002&code=2i3ji2&phone=15690239434&password=123457
```	
<ns1:recoverUserPasswordResponsexmlns:ns1="http://axalent.com/zamapi/">
	<retCode>0</retCode>
</ns1: recoverUserPasswordResponse>
```
### 2.5 getUserValueList
Get user value list

#### Usage
Every Axalent cloud account has its own Value List (attribute-value list) for describing the different kinds of information of the current user, for example: name, birthday, gender, address, etc.  User this API to get all these user information.
#### Parameters
|Parameter name|Description|
|----|----|
secToken|Security Token|
userId|User ID|
userName|User Name|

#### Response
|Response  name|Description|
|----|----|
userId|User ID|
valist|Attribute Name Attribute Value|


#### Examples
https://example.axalent.com:8081/zdk/services/zamapi/getUserValueList?secToken=2002-23214343223&userId=24&userName=jason1
```
<ns1:getUserValueListResponse xmlns:ns1="http://axalent.com/zamapi/">
	<userId>24</ userId>
	<valist>
	<name>nickname</name>
	<value>super man</value>
	</valist>
</ns1: getUserValueListResponse>
```
### 2.6 setUserAttribute
Set user attribute
#### Usage
For setting the user’s attribute value of Axalent cloud account, like: Name, gender, location, etc.
#### Parameters
|Parameter name|Description|
|----|----|
secToken|Security Token|
userId|User ID|
name|Attribute Name|
value|Attribute Value|

#### Response
|Response  name|Description|
|----|----|
retCode|Ret Code (0 meas ok)|

#### Examples
https://example.axalent.com:8081/zdk/services/zamapi/setUserAttribute?secToken=2002-23214343223&userId=24&name=test&value=test
```
<ns1:setUserAttributeResponse xmlns:ns1="http://axalent.com/zamapi/">
	<retCode>0</retCode>
</ns1: setUserAttributeResponse>
```

### 2.7 getUserInfoByName
Get Axalent cloud userID by user name.
#### Usage
Get Axalent cloud userID by user name.
#### Parameters
|Parameter name|Description|
|----|----|
secToken|Security Token|
userName|User Name|

#### Response
|Response  name|Description|
|----|----|
userId|User ID|
name|User Name|

#### Examples
https://example.axalent.com:8081/zdk/services/zamapi/getUserInfoByName?secToken=2002-23214343223&userName=jason1
```
<ns1:getUserInfoByNameResponse xmlns:ns1="http://axalent.com/zamapi/">
	<userId>24</userId>
	<name>jason1</name>
</ns1: getUserInfoByNameResponse>
```
### 2.8 getTouchList
Get list of friend requests.
#### Usage
Friend application is when one Axalent cloud account requests to another Axalent cloud account for becoming “friends” in order to share information between the accounts.  Use this API to get a list of all such requests to the current user.
#### Parameters
|Parameter name|Description|
|----|----|
secToken|Security Token|
userId|User ID|

#### Response
|Response  name|Description|
|----|----|
usertouch|User ID,Name,Message|

#### Examples
https://example.axalent.com:8081/zdk/services/zamapi/getTouchList?secToken=2002-23214343223&userId=24
```
<ns1:getTouchListResponse xmlns:ns1="http://axalent.com/zamapi/">
	<userTouch>
	<userId>24</userId>
	<name>jason1</name>
	<msg> Hello，I am Jason!</msg>
	</userTouch>
</ns1: getTouchListResponse>
```
### 2.9 touchUser
Make a friend request.
#### Usage
User this API to send a friend request to another account.
#### Parameters
|Parameter name|Description|
|----|----|
secToken|Security Token|
userId|User ID|
targetUserId|Target User Id|
msg|Message|

#### Response
|Response  name|Description|
|----|----|
retCode|Ret Code (0 means ok)|

#### Examples
https://example.axalent.com:8081/zdk/services/zamapi/touchUser?secToken=2002-23214343223&userId=24&targetUserId=25&msg=你好，我是jason！

```
<ns1:touchUserResponse xmlns:ns1="http://axalent.com/zamapi/">
	<retCode>0</retCode>
</ns1: touchUserResponse>
```

### 2.10 dealTouch
Respond to friend request.

#### Usage
Use this API to accept or deny a friend request.
#### Parameters
|Parameter name|Description|
|----|----|
secToken|Security Token|
userId|User ID|
targetUserId|Target User Id|
accept|1（Accept），0（Refuse）|

#### Response
|Response  name|Description|
|----|----|
retCode|Ret Code (0 means ok)|

#### Examples
https://example.axalent.com:8081/zdk/services/zamapi/dealTouch?secToken=2002-23214343223&userId=24&targetUserId=25&accept=1
```
<ns1:dealTouchResponse xmlns:ns1="http://axalent.com/zamapi/">
	<retCode>0</retCode>
</ns1: dealTouchResponse>
```
### 2.11 getContactsList
Get friend list
#### Usage
Get all friends of the current Axalent cloud account.
#### Parameters
|Parameter name|Description|
|----|----|
secToken|Security Token|
userId|User ID|

#### Response
|Response  name|Description|
|----|----|
contact|User ID, User Name|

#### Examples
https://example.axalent.com:8081/zdk/services/zamapi/getContactsList?secToken=2002-23214343223&userId=24
```
<ns1:getContactsListResponse xmlns:ns1="http://axalent.com/zamapi/">
	<contact>
	<userId>24</userId>
	<name>jason1</name>
	</contact>
	<contact>
	<userId>25</userId>
	<name>jason2</name>
	</contact>
</ns1: getContactsListResponse>
```
### 2.12 removeContact
Remove a friend
#### Usage
Remove a friend of the current Axalent cloud account.
#### Parameters
|Parameter name|Description|
|----|----|
secToken|Security Token|
userId|User ID|
targetUserId|Target User ID|

#### Response
|Response  name|Description|
|----|----|
retCode|Ret Code (0 means ok)|

#### Examples
https://example.axalent.com:8081/zdk/services/zamapi/removeContact?secToken=2002-23214343223&userId=24&targetUserId=25
```
<ns1:removeContactResponse xmlns:ns1="http://axalent.com/zamapi/">
	<retCode>0</retCode>
</ns1: removeContactResponse>
```
### 2.13 updatePassword
Change password
#### Usage
Both administrator and normal user can use this function to change the account password.
#### Parameters
|Parameter name|Description|
|----|----|
secToken|Security Token|
userId|User ID|
password|Administrator Password|
newPassword|New Password|

#### Response
|Response  name|Description|
|----|----|
retCode|Ret Code (0 means ok)|

#### Examples
https://alpha-api.axalent.com:8081/zdk/services/zamapi/updatePassword ?secToken=1001-680132313-1001&userId=1344&password=axalent1&newPassword=123456
```
<ns1:disableUser :ns1="http://axalent.com/zamapi/">
	<retCode>0</retCode>
</ns1: disableUser >
```
### 2.14 getDeviceTypeList
Get the device type list of the current Axalent cloud appID.
#### Usage
Each Axalent cloud appID is a system.  Each appID may have many types of devices, like Light, Smoke, etc.  Through this API you can get the list of all device types associated with the particular appID represented by the secToken.
#### Parameters
|Parameter name|Description|
|----|----|
secToken|Security Token|

#### Response
|Response  name|Description|
|----|----|
typeNameList|Device ID,Device Name,Device Display Name|

#### Examples
https://example.axalent.com:8081/zdk/services/zamapi/getDeviceTypeList?secToken=2002-2321434322
```
<ns1:getDeviceTypeListResponse xmlns:ns1="http://axalent.com/zamapi/">
	<typeNameList>
	    <id>123</id>
	    <name>Light</name>
		<displayName>Simple Light</displayName>
	</typeNameList>
</ns1: getDeviceTypeListResponse>
```

### 2.15 getDeviceList
Get device list of the current user account.
#### Usage
Every Axalent cloud account can have many devices, like Light, Smoke, etc.  Through this API you can get the list of all devices in the user account.
#### Parameters

|Parameter name|Description|
|----|----|
secToken|Security Token|
userId|User ID|

#### Response
|Response  name|Description|
|----|----|
devList|Device ID<br> Device Name<br>Type ID<br>Time of last modification<br> Ownership(1 Creator of the device, 2 Owner,has complete privilege including share, read, write, 3 read, write, 4 read only, 5 (reserved), 6 (reserved))
presenceInfo (1 means online 0 means offline)<br>Ownership(1 Creator, 2 Owner，3 read，write，4 read)<br>presenceInfo (1 means online 0 means offline)

#### Examples
https://example.axalent.com:8081/zdk/services/zamapi/getDeviceList?secToken=2002-2321434322&userId=24
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
### 2.16 createVirtualDevice
Create Virtual Device.

#### Usage
Creates a virtual device under the cloud account.  A virtual device is not related to any physical device.  It’s purpose is to store and identify data, not to be bounded with a real physical device.

#### Parameters
|Parameter name|Description|
|----|----|
secToken|Security Token|
userId|User ID|
typeId|Type ID|
typeName|Type Name|
parentId|Add virtual device under this “parent device”(optional).  For example, suppose parentId=”44783”, a virtual device created can be added under deviceId 44783 as follows：<br>```<devList>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<devId>44783</devId>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<devName>gateway1</devName>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<typeId>120</typeId>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<ownership>1</ownership>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<presenceInfo>1</presenceInfo>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<lastModified>149..</lastModified>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<devList>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;``` <devId>59048</devId>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<devName>light1</devName>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<typeId>122</typeId>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;``` <ownership>1</ownership>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<presenceInfo>1</presenceInfo>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<lastModified>149..</lastModified>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```</devList>```<br>```</devList> ```|
attributes|(Optional) Based on typeId and typeName, set corresponding device attributes’ values, after virtual devices is created successfully, it will set these attributes automatically.
{“name”:”Device1”, “location”: ”kitchen”}|

#### Response
|Response  name|Description|
|----|----|
deviceId|Device ID|

#### Examples
https://example.axalent.com:8081/zdk/services/zamapi/createVirualDevice?secToken=20022321434322&userId=24&typeId=7&typeName=trigger&attributes={“name”:”light”,”location”:”scene“}
```
<ns1:createVirualDeviceResponse xmlns:ns1="http://axalent.com/zamapi/">
	<deviceId>12232</deviceId>
</ns1: createVirualDeviceResponse>
```
### 2.17 getDeviceAttribute
Get device attribute
#### Usage
Get one attribute’s value for one device under a user account.
#### Parameters
|Parameter name|Description|
|----|----|
secToken|Security Token|
devId|Device ID|
name|Attribute Name|

#### Response
|Response  name|Description|
|----|----|
value|Attribute Value|
updTime|Last update Time|

#### Examples
https://example.axalent.com:8081/zdk/services/zamapi/getDeviceAttribute?secToken=2002-2321434322&devId=2132124&name=open
```
<ns1:getDeviceAttributeResponse xmlns:ns1="http://axalent.com/zamapi/">
	<value>1</value>
	<updTime>1</updTime>
</ns1: getDeviceAttributeResponse>
```
### 2.18 getDeviceAttributesWithValues
Get list of device attribute value
#### Usage
Get a list of attribute values of user account.
#### Parameters
|Parameter name|Description|
|----|----|
secToken|Security Token|
devId|Device ID|
deviceTypeId|Device Type ID|

#### Response
|Response  name|Description|
|----|----|
typeId|Device Type ID|
typeName|Device Type Name|
presenceInfo|Device Status|
attrList|Attribute Name<br>Display Name<br>device：&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;whether attribute value is updated to device or just stored in cloud (1 yes, 0 no)<br>ts：&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br>tsValueType：&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;the type of attribute value<br>value：&nbsp;&nbsp;&nbsp;attribute value<br>updTime：&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;last update time (Unix time stamp)<br>|
#### Examples
https://example.axalent.com:8081/zdk/services/zamapi/getDeviceAttributesWithValues?secToken=2002-2321434322&devId=2132124&deviceTypeId=12
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
</ns1: getDeviceAttributesWithValuesResponse
```
### 2..19 setDeviceAttribute
Set the device attribute
#### Usage
Set the device attribute value under the user account.
#### Parameters
|Parameter name|Description|
|----|----|
secToken|Security Token|
devId|Device ID|
typeId|Type ID|
name|Device Attribute Name|  
value|Device Attribute Value|

#### Response
|Parameter name|Description|
|----|----|
retCode|Ret Code (0 means ok)|

#### Examples
https://example.axalent.com:8081/zdk/services/zamapi/setDeviceAttribute?secToken=2002-23214343223&devId=234924&deviceTypeId=15&name=light&value=1
```
<ns1:setDeviceAttributeResponse xmlns:ns1="http://axalent.com/zamapi/">
	<retCode>0</retCode>
</ns1: setDeviceAttributeResponse>
```
### 2.20 getDeviceTS2
Get the historical values of an attribute of a device
#### Usage
Get the history record of device attribute under the Axalent cloud account.
#### Parameters
|Parameter name|Description|
|----|----|
secToken|Security Token|
devId|Device ID|
typeId|Type ID|
propName|Attribute Name |
start|Start Time(Unix Time stamp)|
end|End Time(Unix Time stamp)|

#### Response
|Response  name|Description|
|----|----|
devId|Device ID|
propName|Device Attribute|
tsData|History Record ID<br>Attribute update time(Unix Time stamp)<br>The value in history record<br>Attachment|

#### Examples
https://example.axalent.com:8081/zdk/services/zamapi/getDeviceTS2?secToken=2002-23214343223&devId=234924&propName=light&start=1494981000000&end=1495006700000
```
<ns1:getDeviceTS2Response xmlns:ns1="http://axalent.com/zamapi/">
	<devId>234924</devId>
	<propName>light</propName>
	<tsData>
		<id>321iji34u543po6i43oi545</id>
		<strValue>1</strValue>
		<time>1494981120000</time>
		<attached>灯被打开</attached>
	</tsData>
	<tsData>
		<id>321iji34u543po6i43oi545</id>
		<strValue>0/strValue>
		<time>1494981060000</time>
		<attached>灯被关闭</attached>
	</tsData>
</ns1: getDeviceTS2Response>
```
### 2.21 getDeviceTS3
Get historical values of multiple devices’ attributes.
#### Usage
Get the historical attributes’ records of multiple devices under the user account, up to the latest 500 records in one response.
#### Parameters
|Parameter name|Description|
|----|----|
secToken|Security Token|
idArray|Device ID.<br>Format：[16126,16127,16128]|
nameArray|Attribute Name Array.<br>Format：[“light”,”location”,” description”]|
start|Start Time(Unix Time stamp)|
end|End Time(Unix Time stamp)|
skip|Skip the earlier record|

#### Response
|Response  name|Description|
|----|----|
devId|History Record ID<br>Attribute update time(Unix Time stamp)<br>The value in history record<br>Attachment|

#### Examples
https://example.axalent.com:8081/zdk/services/zamapi/getDeviceTS3?secToken=2002-23214343223&IdArray=[16126,16127,16128]&nameArray=[“light”,”location”,”description”]&start=1499156347155&end=1499674747155&skip=0
```
<ns1:getDeviceTS3Response xmlns:ns1="http://axalent.com/zamapi/">
	<tsData>
		<id>321iji34u543po6i43oi545</id>
		<devId>16126</devId>
		<name>light</name>
		<strValue>1</strValue>
		<time>1499653793470</time>
		<attached>lights on</attached>
	</tsData>
	<tsData>
		<id>321iji34u543po6i43oi545</id>
		<devId>16126</devId>
		<name>location</name>
		<strValue>room 1</strValue>
		<time>1499653783364</time>
		<attached>room 1</attached>
	</tsData>
	<tsData>
		<id>321iji34u543po6i43oi545</id>
		<devId>16126</devId>
		<name>description</name>
		<strValue>devices in room 1</strValue>
		<time>1499653773365</time>
		<attached> devices in room 1</attached>
	</tsData>
	<tsData>
		<id>321iji34u543po6i43oi545</id>
		<devId>16127</devId>
		<name>light</name>
		<strValue>0</strValue>
		<time>1499653763346</time>
		<attached>lights off</attached>
	</tsData>
	<tsData>
		<id>321iji34u543po6i43oi545</id>
		<devId>16127</devId>
		<name>location</name>
		<strValue>room 2</strValue>
		<time>1499653753339</time>
		<attached>room 2</attached>
	</tsData>
	<tsData>
		<id>321iji34u543po6i43oi545</id>
		<devId>16127</devId>
		<name>description</name>
		<strValue> devices in room 2</strValue>
		<time>1499653743334</time>
		<attached> devices in room 2</attached>
	</tsData>
	<tsData>
		<id>321iji34u543po6i43oi545</id>
		<devId>16128</devId>
		<name>light</name>
		<strValue>1</strValue>
		<time>1499653739291</time>
		<attached>lights on</attached>
	</tsData>
	<tsData>
		<id>321iji34u543po6i43oi545</id>
		<devId>16126</devId>
		<name>location</name>
		<strValue>room 3</strValue>
		<time>1499653737793</time>
		<attached>room 3</attached>
	</tsData>
	<tsData>
		<id>321iji34u543po6i43oi545</id>
		<devId>16126</devId>
		<name>description</name>
		<strValue> devices in room 3</strValue>
		<time>1499653737684</time>
		<attached> devices in room 3</attached>
	</tsData>
</ns1: getDeviceTS3Response>
```
### 2.22 removeDeviceFromUser
Remove a device under the user account
#### Usage
Enter the Device ID which you want to remove, and use this API to remove the device under the user account.
#### Parameters
|Parameter name|Description|
|----|----|
secToken|Security Token|
userId|User ID|
deviceId|Device ID|
parentId|(optional) Specify from which parent node to remove the device|

#### Response
|Response  name|Description|
|----|----|
retCode|Ret Code (0 means ok)|

#### Examples
https://example.axalent.com:8081/zdk/services/zamapi/removeDeviceFromUser?secToken=2002-23214343223&userId=13&deviceId=234924
```
<ns1:removeDeviceFromUserResponse xmlns:ns1="http://axalent.com/zamapi/">
	<retCode>0</retCode>
</ns1: removeDeviceFromUserResponse>
```
### 2.23 deviceAuth
Device Authentication
#### Usage
Check the device’s authenticity, the device ID of the device will be returned if the check passes.

#### Parameters
|Parameter name|Description|
|----|----|
secToken|Security Token|
name|Device Name|
password|Device Password|

#### Response
|Response  name|Description|
|----|----|
devId|Device ID|

#### Examples
https://example.axalent.com:8081/zdk/services/zamapi/deviceAuth?secToken=2002-23214343223&name=AXA0000501&password=8A
```
<ns1:deviceAuthResponse xmlns:ns1="http://axalent.com/zamapi/">
	<devId>123574</devId>
</ns1: deviceAuthResponse>
```

### 2.24 addDeviceToUser
Add device to user
#### Usage
Add a device to user account.

#### Parameters
|Parameter name|Description|
|----|----|
secToken|Security Token|
userId|User ID|
deviceId|Device ID|
deviceName|Device Name|
typeName|Device Type|
parentId|（optional）Add device under this “parent device”.  For example, suppose parentId=”44783”, a device can be added under deviceId 44783 as follows：<br>```<devList>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<devId>44783</devId>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<devName>gateway1</devName>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<typeId>120</typeId>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<ownership>1</ownership>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<presenceInfo>1</presenceInfo>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<lastModified>149..</lastModified>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<devList>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;``` <devId>59048</devId>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<devName>light1</devName>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<typeId>122</typeId>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;``` <ownership>1</ownership>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<presenceInfo>1</presenceInfo>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<lastModified>149..</lastModified>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```</devList>```<br>```</devList> ```|

#### Response
|Response   name|Description|
|----|----|
retCode|Ret Code (0 means ok)|

#### Examples
https://example.axalent.com:8081/zdk/services/zamapi/addDeviceToUser?secToken=2002-23214343223&userId=13&deviceId=234924&deviceName=AXA0000909&typeName=light
```
<ns1:addDeviceToUserResponse xmlns:ns1="http://axalent.com/zamapi/">
	<retCode>0</retCode>
</ns1: addDeviceToUserResponse>
```
### 2.25 setHistoryAttached
Add a description or note to a record in attribute’s history log.
#### Usage
Sometimes, there are too many records, to specially mark one with a short note you can use this API.
#### Parameters
|Parameter name|Description|
|----|----|
secToken|Security Token|
id|History Record ID|
attached|Attachment（Within 255 bytes）|

#### Response
|Response  name|Description|
|----|----|
retCode|Ret Code (0 means ok)|

#### Examples
https://example.axalent.com:8081/zdk/services/zamapi/setHistoryAttached?secToken=2002-23214343223&id=13321312423432423&attached=我的描述
```
<ns1:addDeviceToUserResponse xmlns:ns1="http://axalent.com/zamapi/">
	<retCode>0</retCode>
</ns1: addDeviceToUserResponse>
```
### 2.26 shareDevice
Share device
#### Usage
Share a device to another account of Axalent cloud

#### Parameters
|Parameter name|Description|
|----|----|
secToken|Security Token|
userId|User ID|
deviceId|Device ID|
targetUserId|Target User ID|
ownership|ownership（share privilege）: 2=full, including share device, device status read, write; 3=device status read, write; 4=device status read only|

#### Response
|Response  name|Description|
|----|----|
retCode|Ret Code (0 means ok)|）|

#### Examples
https://example.axalent.com:8081/zdk/services/zamapi/shareDevice?secToken=2002-23214343223&userId=13321312423432423&deviceId=25564&targetUserId=24&ownership=2 
```
<ns1:addDeviceToUserResponse xmlns:ns1="http://axalent.com/zamapi/">
	<retCode>0</retCode>
</ns1: addDeviceToUserResponse>
```
### 2.27 cancelShareDevice
Cancel the device sharing
#### Usage
Revoke the device sharing to another account of Axalent cloud.
#### Parameters
|Parameter name|Description|
|----|----|
secToken|Security Token|
userId|User ID|
deviceId|Device ID|
targetUserId|Target User ID|

#### Response
|Response  name|Description|
|----|----|
retCode|Ret Code (0 means ok)|）|

#### Examples
https://example.axalent.com:8081/zdk/services/zamapi/cancelShareDevice?secToken=2002-23214343223&userId=13321312423432423&deviceId=25564&targetUserId=24 
```
<ns1:addDeviceToUserResponse xmlns:ns1="http://axalent.com/zamapi/">
	<retCode>0</retCode>
</ns1: addDeviceToUserResponse>
```
### 2.28 getDeviceUserList
Get the list of users permitted to share the device
#### Usage
This API gives a way to know how many users are sharing a device in a user account in the Axalent cloud.

#### Parameters
|Parameter name|Description|
|----|----|
secToken|Security Token|
userId|User ID|
deviceId|Device ID|

#### Response
|Response  name|Description|
|----|----|
user|User ID <br> User Name <br> ownership (2 authority，3 read、write，4 read)|

#### Examples
https://example.axalent.com:8081/zdk/services/zamapi/getDeviceUserList?secToken=2002-23214343223&userId=13321312423432423&devId=25564 
```
<ns1:getDeviceUserListResponse xmlns:ns1="http://axalent.com/zamapi/">
	<user>
		<userId>1234</userId>
		<name>jason1</name>
		<ownership>2</ownership>
	</user>
</ns1: getDeviceUserListResponse
```
### 1.30 setMultiDeviceAttributes
Set multiple attributes’ values of a device.
#### Usage
Every device has many attributes, to set multiple attributes at one time you can use this API.
#### Parameters
|Parameter name|Description|
|----|----|
secToken|Security Token|
devId|User ID|
typeId|Device ID|
attributes|Device Attribute List, like：{“name”:”light”:”,location”:”kitchen”}|

#### Response
|Response  name|Description|
|----|----|
retCode|Ret Code (0 means ok)|）|

#### Examples
https://example.axalent.com:8081/zdk/services/zamapi/setMultiDeviceAttributes?secToken=2002-23214343223&devId=232423&typeId=2&attributes={“name”:”light”,”location”:”厨房”}
```
<ns1: setMultiDeviceAttributesResponse xmlns:ns1="http://axalent.com/zamapi/">
	<retCode>0</retCode>
</ns1: setMultiDeviceAttributesResponse>
```
### 2.30 getDeviceTS3Count
Get the count of device logs for the convenience of displaying this info on different pages of an app or webpage.
#### Usage
To get a count of the number of device logs, we can use this API.
#### Parameters
|Parameter name|Description|
|----|----|
secToken|Security Token|
idArray|Device ID(Array)|
nameArray|Custom Name(Array)|
start|Starting Time|
end|Ending Time|
skip|Skip (optional value)|

#### Response
|Response  name|Description|
|----|----|
Count|Count of Return|

#### Examples
https://alpha-api.axalent.com:8081/zdk/services/zamapi/getDeviceTS3Count?secToken=125916222997831001&idArray[63310,47397,1467716,1467717]&nameArray=[“_isonline”,”smoke”]&start=1503446400000&end=1504076624186&skip=0

```
<ns1:getDeviceTS3CountResponse xmlns:ns1="http://axalent.com/zamapi/">
	<count>93</count>
</ns1:getDeviceTS3CountResponse>
```

## 3. Admin APi
This chapter describes APIs that only appID system administrator can use.
### 3.1 systemLogin

Aministrator login

#### Usage
Login administrator needs to call this interface,parameters will be returned when the call succeeds: appId、userId、securityToken,Userid is the ID code of current user.  securityToken is the secure access token,appID  is the Application number.
#### Parameters
|Parameter name|Description|
|----|----|
name|Admin Name|
password|Admin Password|
appid|App ID|

#### Response
|Response  name|Description|
|----|----|
appId|App ID|
userId|User ID|
securityToken|Security Token|

#### Examples
https://alpha-api.axalent.com:8081/zdk/services/zamapi/systemLogin?name=axalentadmin&password=axalent1&appid=1001
```
<ns1:systemLoginResponse xmlns:ns1="http://axalent.com/zamapi/">
    <appId>1001</appId>
    <userId>1001</userId>
	<securityToken>1001-1429804176-1001</securityToken>
</ns1:systemLoginResponse>
```

### 3.2 disableUser 

Allow or disallow a user from logging into the cloud.

#### Usage
This API enables or disables a user account.

#### Parameters

|Response  name|Description|
|----|----|
SysSecToken|Security Token|
userId|User ID|
disable|Enable or disable, 0 means disable, 1 means enable.|

#### Response

|Response  name|Description|
|----|----|
retCode|Ret Code (0 means ok)|

#### Examples

https://alpha-api.axalent.com:8081/zdk/services/zamapi/disableUser ?sysSecToken=1001-680132313-1001&userId=1344&disable=0

```
<ns1:disableUser :ns1="http://axalent.com/zamapi/">
	<retCode>0</retCode>
</ns1: disableUser >
```

### 3.3 createDeviceType
Get the type of device.

#### Usage
To create a new type of device, we can use this API
#### Parameters
|Parameter name|Description|
|----|----|
sysSecToken|Security Token|
typeName|Type Name|
displayName|Device Display Name|

#### Response
|Response  name|Description|
|----|----|
typeId| Type ID|

#### Examples
https://alpha-api.axalent.com:8081/zdk/services/zamapi/createDeviceType?sysSecToken=1001-2139626120-1001&typeName=test1&displayName=test1
```
<ns1:creatDeviceTypeResponse xmlns:ns1="http://axalent.com/zamapi/">
    <typeId>187</typeId>
</ns1:creatDeviceTypeResponse>
```
### 3.4 createDeviceAttribute
Create a device attribute
#### Usage
To create a device attribute, we can use this API
#### Parameters
|Parameter name|Description|
|----|----|
sysSecToken|Security Token|
type|Device Type|
name|Attribute Name|
device|Specify if the attribute is to be stored in hardware or not (just stored in the cloud database).  0=no, 1=yes|
ts|Is there any history record|
tsValueType|Value Type|
displayName|The display name of device attribute|

#### Response
|Response  name|Description|
|----|----|
retCode| Ret Code (0 means ok)|

#### Examples
https://alpha-api.axalent.com:8081/zdk/services/zamapi/createDeviceAttribute?sysSecToken=100121396261201001&type=test6&name=test6&device=1&ts=0&tsValueType=1&displayName=mvesion

```
<ns1:createDeviceAttributeResponse xmlns:ns1="http://axalent.com/zamapi/">
    <retCode>0</retCode>
</ns1:createDeviceAttributeResponse>
```
### 3.5 createCustomAttribute
Create user attribute
#### Usage
Create user attribute (Attribute Name, Attribute Display Name)
#### Parameters
|Parameter name|Description|
|----|----|
sysSecToken|Security Token|
name|Attribute Name|
displayName|Attribute Display Name|

appId|APP ID|
#### Response
|Response  name|Description|
|----|----|
retCode| Ret Code (0 means ok)|

#### Examples
https://alpha-api.axalent.com:8081/zdk/services/zamapi/createCustomAttribute?sysSecToken=1001-6801323131001&name=testApi&displayName=testApi&appId=1001

```
<ns1:createCustomAttributexmlns:ns1="http://axalent.com/zamapi/">
	<retCode>0</retCode>
</ns1:createCustomAttribute>
```
### 3.6 getCustomAttributes
Get user attribute
#### Usage
Check the user attributes (Attribute Name, Attribute Display Name)
#### Parameters
|Parameter name|Description|
|----|----|
sysSecToken|Security Token|
appId|APP ID|

#### Response
|Response  name|Description|
|----|----|
name| User Attribute Name|
displayName|displayName|

#### Examples
https://alpha-api.axalent.com:8081/zdk/services/zamapi/getCustomAttributes?sysSecToken=1001-680132313-1001&appId=1001
```
<ns1:getCustomAttributesResponse xmlns:ns1="http://axalent.com/zamapi/">
<attr>
        <name>cardInfo</name>
        <displayName>Card Information</displayName>
    </attr>
    <attr>
        <name>machines</name>
        <displayName>Wash machine is using</displayName>
    </attr>
    <attr>
        <name>app_ios_ver</name>
        <displayName>iOS APP version</displayName>
    </attr>
    <attr>
        <name>app_android_ver</name>
        <displayName>Android APP version</displayName>
    </attr>
    <attr>
        <name>whitelist</name>
        <displayName>white list</displayName>
    </attr>
    <attr>
        <name>ownership</name>
        <displayName>ownership</displayName>
    </attr>
    <attr>
        <name>grouplist</name>
        <displayName>group list</displayName>
    </attr>
    <attr>
        <name>1</name>
        <displayName>1</displayName>
    </attr>
    <attr>
        <name>2</name>
        <displayName>2</displayName>
    </attr>
    <attr>
        <name>43</name>
        <displayName>43</displayName>
    </attr>
</ns1:getCustomAttributesResponse>
```
### 3.7 registerDevice
Device register

#### Usage
To register a new device, we can use this API
#### Parameters
|Parameter name|Description|
|----|----|
sysSecToken|Security Token|
pwd|Password|
deviceKey|Device Key|
name|Device Name|

#### Response
|Response  name|Description|
|----|----|
devId|Device ID|

#### Examples
https://alpha-api.axalent.com:8081/zdk/services/zamapi/registerDevice?sysSecToken=100121396261201001&pwd=X8&deviceKey=dddddddddddddddddddddddddddddddd&name=T000
```
<ns1:registerDeviceResponse xmlns:ns1="http://axalent.com/zamapi/">
    <devId>1472865</devId>
</ns1:registerDeviceResponse>
```

### 3.8 createNewUser
Create a new user
#### Usage

Create common users through this interface
#### Parameters
|Parameter name|Description|
|----|----|
sysSecToken|Security Token|
name|User Name|
password|User Password|

#### Response
|Response  name|Description|
|----|----|
userId|User ID|

#### Examples

https://alpha-api.axalent.com:8081/zdk/services/zamapi/createNewUser?
sysSecToken=1001-366739636-1001?name=gold?password=123456

```
<ns1:createNewUserResponse xmlns:ns1="http://axalent.com/zamapi/">
    <userId>1589</userId>
</ns1:createNewUserResponse>
```