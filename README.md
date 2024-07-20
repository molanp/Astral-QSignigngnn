# --- Astral-QSignigngnn ---

## Fork版本 Started at 9.0.56 

获取QQSign参数通过Unidbg，开放HTTP API。unidbg-fetch-sign最低从QQ8.9.33（不囊括）开始支持，TIM不支持。

从qsign删库跑路开始（9.0）不支持，俺懒得适配旧版本。

**从9.0.56开始支持。**

## 切记

 - 公共API具有高风险可能
 - 请使用与协议对应版本的libfekit.so文件
 - QSign基于Android平台，其它平台Sign计算的参数不同，不互通（例如：IPad）。
 - 不支持载入Tim.apk的so文件。
 - 为了保证作者的人身**氨醛**，作者绕过了一些致命性检测，但是`故意保留`了一部分，使用本签名将会被腾讯精准检测，具体表现是：4小时内出现冻结，每15天扫脸解封一次，请确认你的账号具有扫脸解封条件，否则请不要使用。

## 使用限制
本签名程序禁止由木落开发的SealDice（简称海豹骰）及其开发分支，通过gocq或包括但不限于在其他协议端进行直接调用或间接调用，整合和使用。
当您使用本程序默认你同意了上述限制。

## 部署方法
(编写中！)

## 你可能需要的项目

- [fix-protocol-version](https://github.com/cssxsh/fix-protocol-version)：基于**mirai**的qsign api对接。

## 使用API

### [初始化QSign&刷新token](https://github.com/fuqiuluo/unidbg-fetch-qsign/blob/master/refresh_token/README.md)

#### 原始energy

```kotlin
# http://host:port/custom_energy?uin=[QQ]&salt=[SALT HEX]&data=[DATA]
```
| 参数名 | 意义      | 例子     |
|-----|---------|--------|
| UIN | Bot的QQ号 | 114514 |

> 非专业人员勿用。

#### sign

```kotlin
# http://host:port/sign?uin=[UIN]&qua=[QUA]&cmd=[CMD]&seq=[SEQ]&buffer=[BUFFER]
```
| 参数名    | 意义                                                | 例子                          |
|--------|---------------------------------------------------|-----------------------------|
| UIN    | Bot的QQ号                                           | 114514                      |
| QUA    | QQ User-Agent，与QQ版本有关                             | V1_AND_SQ_8.9.68_4264_YYB_D |
| CMD    | 指令类型，CMD有很多种，目前登录、发信息均需要sign                      | wtlogin.login               |
| SEQ    | 数据包序列号，用于指示请求的序列或顺序。它是一个用于跟踪请求的顺序的数值，确保请求按正确的顺序处理 | 2333                        |
| BUFFER | 数据包包体，不需要长度，将byte数组转换为HEX发送                       | 020348010203040506          |

<details>
<summary>POST的支持</summary>

如果buffer过长，会超出get请求方式的长度上限，因此sign的请求也支持POST的方式。

请求头 `Content-Type: application/x-www-form-urlencoded`

POST的内容："uin=" + uin + "&qua=" + qua + "&cmd=" + cmd + "&seq=" + seq + "&buffer=" + buffer
</details>

#### 登录包energy(tlv544)

下面这个只是个例子

```kotlin
# http://host:port/energy?version=[VERSION]&uin=[UIN]&guid=[GUID]&data=[DATA]
```

| 参数名     | 意义                                                           | 例子                               |
|---------|--------------------------------------------------------------|----------------------------------|
| VERSION | **注意！**这里的VERSION指的**不是QQ的版本号，而是SDK Version**，可以在QQ安装包中找到此信息 | 6.0.0.2549                       |
| UIN     | Bot的QQ号                                                      | 114514                           |
| GUID    | 登录设备的GUID，将byte数组转换为HEX发送，必须是32长度的HEX字符串                     | ABCDABCDABCDABCDABCDABCDABCDABCD |
| DATA    | QQ发送登录包的CmdId和SubCmdId，例子中810是登陆CmdId，9是SubCmdId             | 810_9                            |

## 其他
- 由于项目的特殊性，我们可能~~随时删除本项目~~且不会做出任何声明