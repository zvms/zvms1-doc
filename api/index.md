# 接口部分文档

这个文档是对前后端接口部分的解释

## 1. 总述

所有的`api`接口的数据都是以`JSON`格式传输的。

在标注`Token携带`的API中，需要将Token在`Authorization`中一并发送。

## 2. 错误发生时

通常发生在一个错误的请求之后，后端会对错误的发生原因进行检查。

发生错误时传回前端的`response`：

``` json
{
    "type": "ERROR",
    "message": "错误信息"
}
```

### 2.1. 常见的错误

#### 2.1.1. 未知错误

通常由后端的运行时环境错误引发，可以尝试重新启动后端服务。

#### 2.1.2. 数据库信息错误

该错误主要会有两种，下面列出了每种错误的处理方法：

* 未查询到相关信息：通常由非法的请求导致，后端会记录下导致错误的请求。如果请求的数据确实存在，请联系软件开发者。
* 要求一个但查询到多个：数据库内信息错误，可以尝试删除数据库中错误或重复的那一条数据。

#### 2.1.3. 请求接口错误

由前端发来的非法请求或新加入的后端无法识别的接口导致，也可以考虑某些用户手动连接服务器发送请求的情况。

#### 2.1.4. 用户ID或密码错误

请让用户检查是否输入了正确的ID和密码，如果忘记密码请找软件开发者联系。

#### 2.1.5. 学生列表错误

通常发生在团支书报名义工活动时填入了他班学生的学号。

#### 2.1.6. 人数超限

发生在义工活动报名时上报的人数大于现需人数时，团支书可视情况删除超出部分。

#### 2.1.7. 权限不足

由跨越权限的操作导致，可能发生在用户手动连接服务器调用高权限的接口时。

### 2.2. 错误处理机制

通常表现为后端拒绝了该操作，向前端返回错误状态和提示。

同时，后端服务器会记录下导致错误的请求，便于系统维护者复现问题。

（保存在文件`logs/debug.log`中）

## 3. 接口调用

### 3.1. 用户相关 `/user/`

#### 3.1.1. 登陆 `/user/login`

Token：不携带

请求方法：`POST`

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

#### 3.1.2. 登出 `/user/logout`

Token：不携带

请求方法：`GET`

> 输出示例

``` json
{
    "type": "SUCCESS",
    "message": "登出成功"
}
```

#### 3.1.3. 查看当前登陆账号的信息 `/user/info`

Token：携带

请求方法：`GET`

> 输出示例

```json
{
    "type": "SUCCESS",
    "message": "获取成功",
    "userName": "王彳亍",
    "class": 202001,
    "permission": 0
}
```

#### 3.1.4. 查看账号信息 `/user/getInfo/\<userId>`

Token：携带

请求方法：`GET`

> 输出示例

```json
{
    "type": "SUCCESS",
    "message": "获取成功",
    "userName": "王彳亍",
    "class": 202001,
    "permission": 0
}
```

### 3.2. 学生相关 `/student`

#### 3.2.1. 查询某个学生的义工本 `/volbook/\<stuId>`

Token：携带

请求方法：`GET`

> 输出示例

``` json
{
    "type": "SUCCESS",
    "message": "获取成功",
    "rec": [
        {"volId": 1, "name": "Event1", "inside": 0.5, "outside": 0, "large": 0, "status": 1},
        {"volId": 3, "name": "Event2", "inside": 1.5, "outside": 0, "large": 0, "status": 1},
        {"volId": 5, "name": "Event3", "inside": 0, "outside": 0, "large": 2, "status": 1},
        {"volId": 6, "name": "Event4", "inside": 0, "outside": 2, "large": 0, "status": 1},
    ]
}
```

### 3.3. 班级相关 `/class`

#### 3.3.1. 获取班级列表 `/class/list`

Token：携带

请求方法：`GET`

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
    ]
}
```

Postscript: `name`随年份自动计算。

#### 3.3.2. 获取某个班级的学生列表 `/class/stulist/\<classId>`

Token：携带

请求方法：`GET`

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
    ]
}
```

#### 3.3.3. 查询某个班级能参加的义工活动列表 `/class/volunteer/\<classId>`

Token：携带

请求方法：`GET`

> 输出示例

``` json
{
    "type": "SUCCESS",
    "message": "获取成功",
    "volunteer": [
        {"id": 1, "name": "义工活动1", "date": "2020.10.1", "time": "13:00", "description": "...", "status": 1, "stuMax": 20},
        {"id": 2, "name": "义工活动2", "date": "2020.10.2", "time": "13:00", "description": "...", "status": 1, "stuMax": 2},
        {"id": 3, "name": "义工活动3", "date": "2020.10.3", "time": "13:00", "description": "...", "status": 0, "stuMax": 5},
        {"id": 4, "name": "义工活动4", "date": "2020.10.4", "time": "13:00", "description": "...", "status": 2, "stuMax": 10}
    ]
}
```

### 3.4. 义工活动相关 `/volunteer`

#### 3.4.1. 义工活动总表 `/volunteer/list`

Token：携带

请求方法：`GET`

> 输出示例

``` json
{
    "type": "SUCCESS",
    "message": "获取成功",
    "volunteer": [
        {"id": 1, "name": "义工活动1", "description": "...", "date": "2020.10.1", "time": "13:00", "status": 1, "stuMax": 20},
        {"id": 2, "name": "义工活动2", "description": "...", "date": "2020.10.2", "time": "13:00", "status": 1, "stuMax": 2},
        {"id": 3, "name": "义工活动3", "description": "...", "date": "2020.10.3", "time": "13:00", "status": 0, "stuMax": 5},
        {"id": 4, "name": "义工活动4", "description": "...", "date": "2020.10.4", "time": "13:00", "status": 2, "stuMax": 10}
    ]
}
```

#### 3.4.2. 查询单次义工详细信息 `/volunteer/fetch/\<volId>`

Token：携带

请求方法：`GET`

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
    "description": "...",
    "status": 1,
    "inside": 0,
    "outside": 3,
    "large": 0
}
```

#### 3.4.3. 报名义工活动 `/volunteer/signup/\<volId>`

Token：携带

请求方法：`POST`

> 输入示例

``` json
{
    "stulst": [
        20200101,
        20200102,
        20200103
    ]
}
```

> 输出示例

``` json
{
    "type": "SUCCESS",
    "message": "报名成功"
}
```

#### 3.4.4. 创建义工活动 `/volunteer/create`

Token：携带

请求方法：`POST`

> 输入示例

``` json
{
    "name": "义工活动1",
    "date": "2020.10.1",
    "time": "13:00",
    "stuMax": 20,
    "description": "...",
    "inside": 0,
    "outside": 3,
    "large": 0,
    "class": [
        {"id": 202001, "stuMax": 10, "visible": true},
        {"id": 202002, "stuMax": 5, "visible": true},
        {"id": 202003, "stuMax": 10, "visible": true}
        {"id": 202004, "stuMax": 0, "visible": false},
    ]
}
```

> 输出示例

``` json
{
    "type": "SUCCESS",
    "message": "创建成功"
}
```

Postscript: `class`是分配给每个班级的人数，`visible`表示该义工活动对该班级是否可见，默认为`false`。

#### 3.4.5. 获取义工活动报名列表 `/volunteer/signerList/\<volId>`

Token：携带

请求方法：`GET`

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

#### 3.4.6. 选择义工活动参加的人 `/volunteer/choose/\<volId>`

Token：携带

请求方法：`POST`

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
}
```

#### 3.4.7. 义工活动参与者列表 `/volunteer/joinerList/\<volId>`

Token：携带

请求方法：`GET`

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

#### 3.4.8. 义工活动感想提交 `/volunteer/thought/\<volId>`

Token：携带

请求方法：`POST`

> 输入示例

```json
{
    "thought":[
        {"userId": 20200101, "content": "没有感想"},
        {"userId": 20200102, "content": "感想没有"}
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

#### 3.4.9. 随机获取一条感想 `/volunteer/randomThought/`

Token：不携带

请求方法：`GET`

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

Postscript: 实际使用过程中这样的感想一定不能让过。

#### 3.4.10. 感想审核 `/volunteer/audit/\<volId>`

Token：携带

请求方法：`POST`

> 输入示例

```json
{
    "thought": [
        {"stuId": 20200101, "status": 1, "inside": 70, "outside": 0, "large": 0},
        {"stuId": 20200102, "status": 2, "inside": 0, "outside": 0, "large": 0},
        {"stuId": 20200103, "status": 3, "inside": 0, "outside": 0, "large": 0}
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

Postscript: `status = 1` 表示审核通过，义工时间会立刻到账；`status = 2` 表示感想被打回，可重新提交；`status = 3`表示写的是什么垃圾感想，义工时间不给了，不允许重新提交。
