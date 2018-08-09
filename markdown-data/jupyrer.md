#DataAPI User Manual

## 1、简介

DataAPI是python3的SDK，通过DataAPI可以从Axalent Cloud获取一些数据统计信息。

## 2、 DataAPI

###  2.1 获取总的设备消息统计

```DataAPI.GetDeviceMsgApp```

参数：无

返回值：pandas data frame

Index|类型|描述
|----|----|----|
0|Int|返回一条数据||
Columns|类型|描述|
up|Int|设备端发向服务器端的消息条数|
down|Int|服务器端发向设备端的消息条数|
upbytes|Int|设备端发向服务器端的消息字节数|
downbytes|Int|服务器端发向设备端的消息字节数|

示例：
![设备消息总数图片]("")

###  2.2 获取总的web API消息统计

DataAPI.GetWebApiMsgApp

参数：

参数|类型|描述
|----|----|----|
name|list|API名称列表，指定获取哪些API的消息统计。可不传，默认获取所有API。|

返回值：pandas data frame


Index|类型|描述
|----|----|----|
0~N|Int||
Columns|类型|描述|
apiname|Str|API的名称|
number|Int|API被调用的次数|

示例：

![webApi_message](图片)

###  2.3 获取用户登录历史记录统计

DataAPI.GetUserLoginHistory

参数：

参数|类型|描述
|----|----|----|
uuid|List|用户ID列表，指定获取哪些用户的登录历史记录。可不传，默认获取所有用户|
start|Str|查询的起始时间，可不传，默认获取终止时间前30天记录。|
end|Str|查询的终止时间，可不传，默认终止时间为现在。|
limit|Int|返回的记录最大条数，可不传，默认最大返回500条记录。|

返回值：pandas data frame


Index|类型|描述
|----|----|----|
0 ~ N|Int|
Columns|类型|描述|
uuid|Int|用户ID|
appid|Int|应用ID|
addr|Str|登录的IP地址|
port|Int|登录的端口号|
permission|Str|登录方式，user：普通登录，admin：管理员登录，system：系统登录|
country|Str|登陆地所在国家|
area|Str|登陆地所在区域|
region|Str|登陆地所在省|
city|Str|登陆地所在城市|
isp|Str|登陆地所属运营商|
timestamp|Int|登录时间戳|

示例：

![login_history](图片)

###  2.4 按天获取活跃用户数量统计

DataAPI.GetActiveUserDay


参数|类型|描述
|----|----|----|
start|Str|查询的起始日期，可不传，默认获取终止日期前30天记录。|
end|Str|查询的终止日期，可不传，默认终止日期为现在。|

返回值：pandas data frame

Index|类型|描述
|----|----|----|
0~N|Int||
Columns|类型|描述|
number|Int|当天用户登录的数量，同一个用户只计算一次。|
totalnumber|Int|当天用户登录的次数，同一个用户可计算多次。|
data|Str|日期，例如：2018-2-15|

示例：
![user_number](图片)


### 2.5 按天获取用户活跃时间段统计

DataAPI.GetUserActiveTimeDay

参数：

参数|类型|描述
|----|----|----|
start|Str|查询的起始日期，可不传，默认获取终止日期前30天记录。|
end|Str|查询的终止日期，可不传，默认终止日期为现在。|

返回值：pandas data frame(multi index)

Index|类型|描述
|----|----|----|
0 ~ N|Int||
Columns|类型|描述|
hour|Str|0 ~ 23，0表示0点到1点之间用户登录情况，以此类推。|
number|Int|当天该时间段用户登录的数量，同一个用户只计算一次。|
totalnumber|Int|当天该时间段用户登录的次数，同一个用户可计算多次。|
data|Str|日期，例如：2018-2-15|


示例：

![user_active](图片)

### 2.6 按天获取用户登录地点统计

DataAPI.GetUserLoginLocationDay

参数：


参数|类型|描述
|----|----|----|
start|Str|查询的起始日期，可不传，默认获取终止日期前30天记录。|
end|Str|查询的终止日期，可不传，默认终止日期为现在。|

返回值：pandas data frame1，pandas data frame2

Pandas data frame1：


Index|类型|描述
|----|----|----|
0 ~ N|Int||
Columns|类型|描述|
region|Str|省份或直辖市，例如：四川省，上海市，甘肃省|
number|Int|当天在该省份登录的用户数量，同一个用户只计算一次。|
totalnumber|Int|当天在该省份登录的用户数量，同一个用户可计算多次。|
date|Str|日期，例如：2018-2-15|

Pandas data frame2：

Index|类型|描述
|----|----|----|
0 ~ N|Int||
Columns|类型|描述|
city|Str|城市，例如：成都市，上海市，兰州市|
number|Int|当天在该城市登录的用户数量，同一个用户只计算一次。|
totalnumber|Int|当天在该城市登录的用户数量，同一个用户可计算多次。|
date|Int|日期，例如：2018-2-15|

示例：

![login_adress](图片)

###  2.7 按天获取设备连接统计

DataAPI.GetDeviceConnectionDay

参数：


参数|类型|描述
|----|----|----|
start|Str|查询的起始日期，可不传，默认获取终止日期前30天记录。|
end|Str|查询的终止日期，可不传，默认终止日期为现在。|

返回值：pandas data frame


Index|类型|描述
|----|----|----|
0 ~ N|Int||
Columns|类型|描述|
connectionsmax|Int|当天TCP/UDP连接的最大值|
connectionsmin|Int|当天TCP/UDP连接的最小值|
endpointsmax|Int|当天连接设备数的最大值|
endpointsmin|Int|当天连接设备数的最小值|
date|Str|日期，例如：2018-2-15|

示例：

![device_connect](图片)



### 2.8 按天获取设备上下线统计

DataAPI.GetDeviceIsonlineDay

参数：

参数|类型|描述
|----|----|----|
start|Str|查询的起始日期，可不传，默认获取终止日期前30天记录。|
end|Str|查询的终止日期，可不传，默认终止日期为现在。|

返回值：pandas data frame

Index|类型|描述
|----|----|----|
0 ~ N|Int||
Columns|类型|描述|
online|Int|当天上线的设备数量，同一个设备只计算一次。|
totaonline|Int|当天上线的设备数量，同一个设备可计算多次。|
offline|Int|当天下线的设备数量，同一个设备只计算一次。|
totaloffline|Int|当天下线的设备数量，同一个设备可计算多次。|
date|Str|日期，例如：2018-2-15|


![device-off-on-line](图片)


### 2.9 按天获取设备联动统计

DataAPI.GetTriggerDay

参数：


参数|类型|描述
|----|----|----|
start|Str|查询的起始日期，可不传，默认获取终止日期前30天记录。|
end|Str|查询的终止日期，可不传，默认终止日期为现在。|

返回值：pandas data frame

Index|类型|描述
|----|----|----|
0 ~ N|Int||
Columns|类型|描述|
srcnumber|Int|当天设备联动源设备数量，同一个设备只计算一次。|
targetnumber|Int|当天设备联动目标设备数量，同一个设备只计算一次。|
totaloffline|Int|当天设备联动次数，同一个设备可计算多次。|
date|Str|日期，例如：2018-2-15|

示例：

![device_trigger](图片)

### 2.10 按天获取设备消息统计
DataAPI.GetDeviceMsgDay


参数|类型|描述
|----|----|----|
start|Str|查询的起始日期，可不传，默认获取终止日期前30天记录。|
end|Str|查询的终止日期，可不传，默认终止日期为现在。|

返回值：pandas data frame

Index|类型|描述
|----|----|----|
0 ~ N|Int||
Columns|类型|描述|
up|Int|当天设备端发向服务器端的消息数量。|
down|Int|当天服务器端发向设备端的消息数量。|
upbytes|Int|当天设备端发向服务器端的消息字节数。|
downbytes|Int|当天服务器端发向设备端的消息字节数。|
date|Str|日期，例如：2018-2-15|

示例：

![device_message](图片)

### 2.11 按天获取Web API消息统计

DataAPI.GetWebApiMsgDay

参数|类型|描述
|----|----|----|
start|Str|查询的起始日期，可不传，默认获取终止日期前30天记录。|
end|Str|查询的终止日期，可不传，默认终止日期为现在。|

返回值：pandas data frame

Index|类型|描述
|----|----|----|
0 ~ N|Int||
Columns|类型|描述|
number|Int|当天Web API调用的次数。|
date|Str|日期，例如：2018-2-15|



示例：

![](图片)
