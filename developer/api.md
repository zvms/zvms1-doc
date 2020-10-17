# API列表

以下请求皆为POST发送。输入输出皆为JSON格式。

## 发生错误时的默认返回值

``` json
{
    "type": "ERROR",
    "message": "错误信息"
}
```

## 用户 /user

### 用户登录 /login
**/user/login**
> 输入示例

``` json
{
    "userid": 202001,
    "password": 123456
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
}
```

### 用户登出 /logout
**/user/logout**

> 输出示例

``` json
{
    "type": "SUCCESS",
    "message": "登出成功"
}
```

## 学生 /student

### 学生完成的义工列表 /volbook/\<stuid>
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

### 班级学生列表 /stulist/\<classid>
**/class/stulist/\<classid>**

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

### 班级义工活动列表 /volunteer/\<classid>
**/class/volunteer/\<classid>**

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
    "nowStu": 18,
    "description": "新华书店打扫",
    "status": 1,
    "inside": 0,
    "outside": 3,
    "large": 0,
    "class": [
        {"id": 202001, "name": "高一1班"},
        {"id": 202011, "name": "蛟一1班"},
        {"id": 202002, "name": "高一2班"},
        {"id": 201901, "name": "高二1班"},
        {"id": 201801, "name": "高三1班"}
    ]
}
```
> 输入示例2

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

