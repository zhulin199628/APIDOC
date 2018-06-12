# Axalent云平台接口定义
## 1.UserApi
本节描述用户可用的API 

### 1.1 userLogin
用户登录界面
#### Usage
您的应用程序要想接入到Axalent cloud需调用此接口，成功后会返回userId、securityToken两个参数，userId为当前用户的编号，securityToken为安全令牌，Axalent cloud通过安全令牌来验证每个请求是否为该用户发出的，一个安全令牌有效期为15分钟，如果失效，你需要重新登录来获取另一个安全令牌

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
### 1.2 registerUser
注册用户接口
 
#### Usage
注册一个Axalent cloud账号，如果您的应用程序没有账户可调用此接口来注册一个Axalent cloud账户，传入手机号参数后调用getVerificationCode接口向手机发送验证码短信，手机凭此短信的验证码进行注册

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
### 1.3 getVerificationCode
获取手机短信验证码

#### Usage
获取手机短信验证码有两种，1是获取注册用户短信验证码，2是获取忘记密码短信验证码，Axalent cloud会通过这两种类型发送短信验证给用户，用户将短信验证码填入相应的操作接口即可
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

### 1.4 recoverUserPassword  
恢复用户密码
#### Usage
如果您忘记了Axalent cloud账号的密码可通过此接口来恢复密码，传入手机号参数后调用getVerificationCode接口向手机发送验证码短信，手机凭此短信的验证码进行恢复

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
### 1.5 getUserValueList
获取用户属性列表

#### Usage
每个Axalent cloud账户都有一个属性列表，此属性列表用来描述当前账户的各种信息，例如：昵称、生日、性别、城市等
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
### 1.6 setUserAttribute
设置用户属性
#### Usage
用来设置Axalent cloud账户的属性信息，例如：昵称、性别、城市等等
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

### 1.7 getUserInfoByName
根据用户名获取Axalent cloud账户信息
#### Usage
通过用户名来获取某个Axalent cloud的账户信息
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
### 1.8 getTouchList
获取好友申请消息列表
#### Usage
好友申请消息是一个Axalent cloud账户向另一个Axalent cloud账户发送添加好友申请，调用这个接口将获取当前用户所有的好友申请消息
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
### 1.9 touchUser
添加好友申请
#### Usage
向另一个Axalent cloud账户发送添加好友申请，调用这个接口即可
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

### 1.10 dealTouch
处理好友申请
#### Usage
处理好友申请接口，收到一个用户的好友申请可以调用此接口去处理好友请求
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
### 1.11 getContactsList
获取好友列表
#### Usage
获取当前Axalent cloud账户下的所有好友列表
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
### 1.12 removeContact
删除联系人
#### Usage
删除当前Axalent cloud账户下的联系人
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
### 1.13 disableUser 
管理用户是否可以登录。需要有系统账户权限使用。
#### Usage
由system account 调用，通过此接口可以启动或者禁用某一个普通用户的账户
#### Parameters
|Parameter name|Description|
|----|----|
secToken|Security Token|
userId|User ID|
disable|是否禁用，0表示禁用账户，1表示启动账户Enable or disable, 0 means disable, 1 means enable.|

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
### 1.14 updatePassword 
更新密码接口
#### Usage
管理员和普通用户均可调用，用来更新密码
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
<ns1:updatePasswordResponse xmlns:ns1="http://axalent.com/zamapi/">
    <retCode>0</retCode>
</ns1:updatePasswordResponse>
```
### 1.15 getDeviceTypeList
获取当前Axalent cloud下的所有设备类型列表
#### Usage
Axalent cloud拥有众多设备类型，比如Light、Smoke等类型，通过此接口就能获取Axalent cloud现拥有所有设备类型列表
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
### 1.16 getDeviceList
获取当前Axalent cloud账户下的所有设备列表
#### Usage
每个Axalent cloud账户拥有众多设备，比如Light、Smoke等设备，通过此接口就能获取Axalent cloud账户下有所有设备列表
#### Parameters

|Parameter name|Description|
|----|----|
secToken|Security Token|
userId|User ID|

#### Response
|Response  name|Description|
|----|----|
devList|Device ID<br> Device Name<br>Type ID<br>Time of last modification<br> 设备权限（1创建者，2完全控制权，包括分享、读、写设备），3 只能对设备进行读和写，4 只能读取设备信息，5 扩展，6 扩展）presenceInfo 设备状态（1为在线、0为掉线）<br>Ownership(1 Creator, 2 Owner，3 read，write，4 read)<br>presenceInfo (1 means online 0 means offline)

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
### 1.17 createVirtualDevice
创建虚拟设备

#### Usage
在Axalent cloud账户下创建一个虚拟设备，此设备不同于实体设备，它的作用在于存储、标识数据，而非直接与实体设备绑定。

#### Parameters
|Parameter name|Description|
|----|----|
secToken|Security Token|
userId|User ID|
typeId|Type ID|
typeName|Type Name|
parentId|标识将设备加入到某个设备节点下（可选），例如若parentId=”44783”，创建的设备就会加到44783下面，结果如下：<br>```<devList>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<devId>44783</devId>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<devName>gateway1</devName>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<typeId>120</typeId>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<ownership>1</ownership>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<presenceInfo>1</presenceInfo>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<lastModified>149..</lastModified>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<devList>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;``` <devId>59048</devId>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<devName>light1</devName>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<typeId>122</typeId>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;``` <ownership>1</ownership>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<presenceInfo>1</presenceInfo>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<lastModified>149..</lastModified>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```</devList>```<br>```</devList> ```|
attributes|设备属性列表（可选），添加成功后将自动设置设备的属性，例<br>{“name”:”设备1”, “location”:”厨房”}|

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
### 1.18 getDeviceAttribute
获取设备属性
#### Usage
获取Axalent cloud账户下的单个设备属性值
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
### 1.19 getDeviceAttributesWithValues
获取设备属性列表
#### Usage
获取Axalent cloud账户下的设备多个属性值列表
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
attrList|Attribute Name<br>Display Name<br>device&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;属性是否是设备属性，是则发送到硬件设备并保存到数据库，否则只保存到数据库(1 是，0否)。<br>ts&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;是否拥有历史记录(1 是，0否)。<br>tsValueType&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;属性的数据类型。<br>value&nbsp;&nbsp;&nbsp;属性值。<br>updTime&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;属性的更新时间(Unix时间戳)。<br>|

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
### 1.20 setDeviceAttribute
设置设备属性
#### Usage
设置Axalent cloud账户下的设备属性
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
### 1.21 getDeviceTS2
获取设备历史记录
#### Usage
获取Axalent cloud账户下的设备属性历史记录
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
### 1.22 getDeviceTS3
获取多个设备属性的历史记录
#### Usage
获取Axalent cloud账户下的多个设备属性的历史记录，一次性最多返回500条
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
### 1.23 removeDeviceFromUser
删除Axalent cloud账户下的设备
#### Usage
传入想要删除设备的编号，在通过此接口删除Axalent cloud账户下的设备
#### Parameters
|Parameter name|Description|
|----|----|
secToken|Security Token|
userId|User ID|
deviceId|Device ID|
parentId|标识从某个父节点中移除该设备中（可选）|

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
### 1.24 deviceAuth
设备验证
#### Usage
验证此设备，如果通过则返回设备的编号

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

### 1.25 addDeviceToUser
添加设备到用户
#### Usage
添加一个设备到Axalent cloud账户下

#### Parameters
|Parameter name|Description|
|----|----|
secToken|Security Token|
userId|User ID|
deviceId|Device ID|
deviceName|Device Name|
typeName|Device Type|
parentId|标识将设备加入到某个设备节点下（可选），例如若parentId=”44783”，创建的设备就会加到44783下面，结果如下：<br>```<devList>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<devId>44783</devId>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<devName>gateway1</devName>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<typeId>120</typeId>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<ownership>1</ownership>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<presenceInfo>1</presenceInfo>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<lastModified>149..</lastModified>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<devList>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;``` <devId>59048</devId>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<devName>light1</devName>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<typeId>122</typeId>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;``` <ownership>1</ownership>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<presenceInfo>1</presenceInfo>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```<lastModified>149..</lastModified>```<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```</devList>```<br>```</devList> ```|

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
### 1.26 setHistoryAttached
设置历史记录附加描述
#### Usage
有时候设备的历史过多，但是我们想标识其中的某个历史记录就可以调用此接口
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
### 1.27 shareDevice
分享设备
#### Usage
分享一个设备给另外一个Axalent cloud账户

#### Parameters
|Parameter name|Description|
|----|----|
secToken|Security Token|
userId|User ID|
deviceId|Device ID|
targetUserId|Target User ID|
ownership|Ownership（2.完全控制权，包括分享、读、写，3.只能对设备读、写，4.只能读取设备信息

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
### 1.28 cancelShareDevice
取消设备分享
#### Usage
取消分享一个设备给另外一个Axalent cloud账户
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
### 1.29 getDeviceUserList
获取当前设备被分享的所有用户列表 
#### Usage
通过此接口可以获得Axalent cloud的某个设备被分享给了多少用户

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
设置多个设备的属性
#### Usage
每个设备拥有多个属性，想要一次性设置多个属性可以使用此接口
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
### 1.31 getDeviceTS3Count
获取设备日志的条数
#### Usage
通过此接口可以获得日志的总条数
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

## 2. Admin APi
本节用来描述管理员可用的API
### 2.1 systemLogin
管理员登录
#### Usage
登录管理员需要调用此接口，成功后会返回appId、userId、securityToken两个参数，userId为用户编号，securityToken为安全令牌，appId为应用程序编号
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
### 2.2 createDeviceType
创建设备类型
#### Usage
通过此接口可以创建设备的类型
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
### 2.3 createDeviceAttribute
创建设备属性
#### Usage
通过此接口可以设置设备属性
#### Parameters
|Parameter name|Description|
|----|----|
sysSecToken|Security Token|
type|Device Type|
name|Attribute Name|
device|是否为硬件属性，硬件属性需要发送到硬件并保存到数据库，否则只保存到数据库|
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
### 2.4 createCustomAttribute
创建用户属性
#### Usage
创建用户属性（属性名称，属性显示名称）
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
### 2.5 getCustomAttributes
获取用户属性
#### Usage
查看用户属性（属性名称，属性显示名称）
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
### 2.6 registerDevice
注册设备
#### Usage
通过此接口可以注册一个设备
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

### 2.7 createNewUser
创建用户

#### Usage

通过此接口可以创建普通用户

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

### 2.8 disableUser

禁用或启用账户

#### Usage

由system account 调用,通过此接口可以禁用有启用普通用户的账户

#### Parameters
|Parameter name|Description|
|----|----|
sysSecToken|Security Token|
userId|User ID|
disable|Disable(0 表示启用，用户可以登录。1 表示禁用，用户不能登录)|

#### Response

|Response  name|Description|
|----|----|
retCode|Ret Code (0 means OK)|

#### Examples

https://alpha-api.axalent.com:8081/zdk/services/zamapi/disableUser?
sysSecToken=1001-366739636-1001&userId=1344&disable=0
```
<ns1:disableUserResponse xmlns:ns1="http://axalent.com/zamapi/">
    <retCode>0</retCode>
</ns1:disableUserResponse>
```
