# API列表

以下请求皆为POST发送。输入输出皆为JSON格式。

在没有标注`NoToken`的API中，需要将Token在`Authorization`中一并发送。

## 发生错误时的默认返回值

``` json
{
    "type": "ERROR",
    "message": "错误信息"
}
```

## 用户 /user

### 用户登录（NoToken） /login
**/user/login**

> 输入示例

``` json
{
    "userid": "202001",
    "password": "123456"
}
```

> 输出示例

``` json
{
    "type": "SUCCESS",
    "message": "登陆成功",
    "username": "王彳亍",
    "class": 202001,
    "permission": 0
    "token": "xxxx"
}
```

### 用户登出（NoToken） /logout
**/user/logout**

> 输出示例

``` json
{
    "type": "SUCCESS",
    "message": "登出成功"
}
```

### 用户信息 /info
**/user/info**

> 输出示例

```json
{
    "type": "SUCCESS",
    "message": "获取成功",
    "userName": "王彳亍",
    "class": 202001,
    "permission": 0
    // 返回数据库中除了`userId`之外的所有内容
}
```

### 用户信息 /getInfo/\<userId>
**/user/getInfo/\<userId>**

> 输出示例

```json
{
    "type": "SUCCESS",
    "message": "获取成功",
    "userName": "王彳亍",
    "class": 202001,
    "permission": 0
    // 只有权限4能使用
}
```

## 学生 /student

### 学生完成的义工列表 /volbook/\<stuId>
**/student/volbook/\<stuId>**

> 输出示例

``` json
{
    "type": "SUCCESS",
    "message": "获取成功",
    "rec": [
        {"volId": 1, "name": "打扫新华书店", "inside": 0.5, "outside": 0, "large": 0, "status": 1},
        {"volId": 3, "name": "打扫新华树店", "inside": 1.5, "outside": 0, "large": 0, "status": 1},
        {"volId": 5, "name": "打扫新华数店", "inside": 0, "outside": 0, "large": 2, "status": 1},
        {"volId": 6, "name": "打扫新华鼠店", "inside": 0, "outside": 2, "large": 0, "status": 1},
    ]
}
```

## 班级 /class

### 班级列表 /list
**/class/list**

> 输出示例

``` json
{
    "type": "SUCCESS",
    "message": "获取成功",
    "class": [
        {"id": 202001, "name": "高一1班"},
        {"id": 202011, "name": "蛟一1班"},
        {"id": 202002, "name": "高一2班"},
        {"id": 201901, "name": "高二1班"},
        {"id": 201801, "name": "高三1班"}
        //name随年份自动计算
    ]
}
```

### 班级学生列表 /stulist/\<classId>
**/class/stulist/\<classId>**

> 输出示例

``` json
{
    "type": "SUCCESS",
    "message": "获取成功",
    "student": [
        {"id": 20200101, "name": "王可", "inside": 1.5, "outside": 2, "large": 8},
        {"id": 20200102, "name": "王不可", "inside": 2.5, "outside": 2, "large": 8},
        {"id": 20200103, "name": "王可以", "inside": 5, "outside": 8, "large": 0},
        {"id": 20200104, "name": "王不行", "inside": 1, "outside": 4, "large": 16},
        {"id": 20200105, "name": "王彳亍", "inside": 5, "outside": 0, "large": 8}
        // inside表示校内义工，outside表示校外义工，large表示大型活动义工
    ]
}
```

### 班级义工活动列表 /volunteer/\<classId>
**/class/volunteer/\<classId>**

> 输出示例

``` json
{
    "type": "SUCCESS",
    "message": "获取成功",
    "volunteer": [
        {"id": 1, "name": "义工活动1", "date": "2020.10.1", "time": "13:00", "description": "打扫新华书店", "status": 1, "stuMax": 20},
        {"id": 2, "name": "义工活动2", "date": "2020.10.2", "time": "13:00", "description": "新华书店打扫", "status": 1, "stuMax": 2},
        {"id": 3, "name": "义工活动3", "date": "2020.10.3", "time": "13:00", "description": "华新书店打扫", "status": 0, "stuMax": 5},
        {"id": 4, "name": "义工活动4", "date": "2020.10.4", "time": "13:00", "description": "打扫华新书店", "status": 2, "stuMax": 10}
    ]
}
```

## 义工 /volunteer

### 义工活动列表 /list
**/volunteer/list**

> 输出示例

``` json
{
    "type": "SUCCESS",
    "message": "获取成功",
    "volunteer": [
        {"id": 1, "name": "义工活动1", "description": "新华打扫书店", "date": "2020.10.1", "time": "13:00", "status": 1, "stuMax": 20},
        {"id": 2, "name": "义工活动2", "description": "打扫新华书店", "date": "2020.10.2", "time": "13:00", "status": 1, "stuMax": 2},
        {"id": 3, "name": "义工活动3", "description": "打新华打新华", "date": "2020.10.3", "time": "13:00", "status": 0, "stuMax": 5},
        {"id": 4, "name": "义工活动4", "description": "扫书店扫书店", "date": "2020.10.4", "time": "13:00", "status": 2, "stuMax": 10}
    ]
}
```

### 义工详细信息 /fetch/\<volId>
**/volunteer/fetch/\<volId>**

> 输出示例

``` json
{
    "type": "SUCCESS",
    "message": "获取成功",
    "name": "义工活动1",
    "date": "2020.10.1",
    "time": "13:00",
    "stuMax": 20,
    "stuNow": 18,
    "description": "新华书店打扫",
    "status": 1,
    "inside": 0,
    "outside": 3,
    "large": 0
}
```

### 报名义工 /signup/\<volId>
**/volunteer/signup/\<volId>**

> 输入示例

``` json
{
    "stulst": [
        20200101,
        20200102,
        20200103
    ]
    // 注意 这里别忘记后台的session验证，要检验stulst中的人是否都是本班的
}
```

> 输出示例

``` json
{
    "type": "SUCCESS",
    "message": "报名成功"
}
```

### 创建义工活动 /create
**/volunteer/create**
> 输入示例

``` json
{
    "name": "义工活动1",
    "date": "2020.10.1",
    "time": "13:00",
    "stuMax": 20,
    "description": "新华书店打扫",
    "inside": 0,
    "outside": 3,
    "large": 0,
    "class": [
        {"id": 202001, "stuMax": 10},
        {"id": 202002, "stuMax": 5},
        {"id": 202003, "stuMax": 10}
    ]
    // hid 是自动从session获取的
}
```
> 输出示例

``` json
{
    "type": "SUCCESS",
    "message": "创建成功"
}
```

### 义工活动报名者列表 /signerList/\<volId>
**/volunteer/signerList/\<volId>**
> 输出示例

```json
{
    "type": "SUCCESS",
    "message": "获取成功",
    "result": [
        {"stuId": 20200101, "stuName": "王彳亍"},
        {"stuId": 20200102, "stuName": "王不可"},
        {"stuId": 20200103, "stuName": "王可"}
    ]
}
```

### 义工活动选人 /choose/\<volId>
**/volunteer/choose/\<volId>**
> 输入示例

```json
{
    "result": [
        {"stuId": 20200101, "res": true},
        {"stuId": 20200102, "res": false}
    ]
}
```
> 输出示例

```json
{
    "type": "SUCCESS",
    "message": "审核成功"
    // 要判断权限
}
```

### 义工活动参与者列表 /joinerList/\<volId>
**/volunteer/joinerList/\<volId>**
> 输出示例

```json
{
    "type": "SUCCESS",
    "message": "获取成功",
    "result": [
        {"stuId": 20200101, "stuName": "王彳亍"},
        {"stuId": 20200103, "stuName": "王可"}
    ]
}
```

### 义工活动感想 /thought/\<volId>
**/volunteer/thought/\<volId>**
> 输入示例

```json
{
    "thought":[
        {"userId": 20200101, "content": "没有感想"},
        {"userId": 20200102, "content": "感想没有"}
        // 如果一个人交了两次感想就报错
    ]
}
```

> 输出示例

```json
{
    "type": "SUCCESS",
    "message": "提交成功"
    // 要判断权限
}
```

### 随机义工活动感想 （NoToken）/randomThought/
**/volunteer/randomThought/**

> 输出示例

```json
{
    "type": "SUCCESS",
    "message": "获取成功",
    "userName": "王彳亍",
    "userId": 20200101,
    "content": "没有感想"
}
```

### 义工活动最终审核/audit/\<volId>
**/volunteer/audit/\<volId>**

> 输入示例
```json
{
    // "status": 1, // 审核通过
    "status": 2, // 感想不通过，感想重新提交后可以再次审核
    // "status": 3, // 义工活动审核不通过，不能再次审核
    "thought":[
        {"stuId": 20200101, "thought": true},
        {"stuId": 20200101, "thought": false},
    ],
    "time": [ // 如果感想未通过，time应该不出现
        {"stuId": 20200101, "inside": 1.2, "outside": 0, "large": 0},
        {"stuId": 20200101, "inside": 1.2, "outside": 0, "large": 0},
    ]
}
```

> 输出示例

```json
{
    "type": "SUCCESS",
    "message": "提交成功"
}
```
