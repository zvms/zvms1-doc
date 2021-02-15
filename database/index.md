*[返回上一级](./../index)

# 数据库部分文档

这个文档是对数据库部分的解释

## 1. 总述

数据库采用了`MySQL`，使用`pymysql`进行连接

## 2. 数据库位置

服务器位置：（Waiting for complete）

用户名：`zvms`

密码：不需要知道，后端会提供完备的接口进行处理

## 3. 数据库架构

在用户`zvms`下有五个数据表，`user`,`student`,`volunteer`,`stu_vol`和`class_vol`

### 3.1. `user`表

这个数据表只储存可以登录的用户，每个班和每个老师分配一个账户，义管会、实践部另算

项名 | 解释 | 类型 | 简述 | 举例 | 备注信息
-|-|-|-|-|-
userId | user identity | INTEGER | 储存用户的id | 202001 |
userName | user name | CHAR(64) | 储存用户的名字 | 王彳亍 |
class | class | INTEGER | 储存用户拥有的班级id | 202001 |
permission | permission | SMALLINT | 储存用户的身份权限 | 0 | 1:团支书 2: 教师 3: 义管会 4: 系统
password | password | CHAR(255) | 储存密码 | aababababab | 加密方式为`md5`

### 3.2. `student`表

这个数据表存储每个学生的身份以及义工完成情况

项名 | 解释 | 类型 | 简述 | 举例 | 备注信息
-|-|-|-|-|-
stuId | student identity | INTEGER | 储存学生的学号 | 20200101 |
stuName | student name | CHAR(64) | 储存学生的名字 | 王彳亍 |
volTimeInside | volunteering time inside | INTEGER | 储存学生的累计校内义工时间 | 0 | 校内义工，为避免浮点运算，在数据库中以分钟为单位
volTimeOutside | volunteering time outside | INTEGER | 储存学生的累计校外义工时间 | 0 | 校外义工，以分钟为单位
volTimeLarge | volunteering time large | INTEGER | 储存学生的累计大型活动义工时间 | 0 | 大型活动义工，以分钟为单位

### 3.3. `volunteer`表

这个数据表存储每次义工活动的详细信息

项名 | 解释 | 类型 | 简述 | 举例 | 备注信息
-|-|-|-|-|-
volId | volunteering identity | INTEGER | 义工活动的唯一确定编号 | 1 |
volName | volunteering name | CHAR(255) | 义工活动的名称 | 喂孔子+拜锦鲤 |
volDate | volunteering date | CHAR(64) | 义工活动的日期 | 2020.9.24 |
volTime | volunteering time | CHAR(64) | 义工活动的时间 | 13:00 |
stuMax | maximum students | INTEGER | 义工活动的人数上限 | 10 |
nowStuCount | now student | INTEGER | 义工活动的现有人数 | 8 |
description | description | TEXT | 义工活动的描述 | blablablabla |
status |status | SMALLINT | 义工活动的状态 | 0 | Waiting for complete
volTimeInside | volunteering time inside | INTEGER | 每个人预计将获得的校内义工时间 | 0 | 以分钟为单位
volTimeOutside | volunteering time outside | INTEGER | 每个人预计将获得的校外义工时间 | 60 | 以分钟为单位
volTimeLarge | volunteering time large | INTEGER | 每个人预计将获得的大型活动义工时间 | 0 | 以分钟为单位
holderId | volunteer holder\'s id | INTEGER | 义工发布者的`userid` | 202001 |

### 3.4. `stu_vol`表

这个数据表存储学生和义工活动之间的关系

项名 | 解释 | 类型 | 简述 | 举例 | 备注信息
-|-|-|-|-|-
volId | volunteering identity | INTEGER | 义工活动的编号 | 1 | 表示`stuId`的学生参加了这个义工活动
stuId | student identity | INTEGER | 学生的学号 | 20200101 | 表示这个学生参加了`volId`的义工活动
status | status | SMALLINT | 审核状态 | 0 | Waiting for complete
volTimeInside | volunteering time inside | INTEGER | 实际获得的校内义工时间 | 0 | 以分钟为单位
volTimeOutside | volunteering time outside | INTEGER | 实际获得的校外义工时间 | 60 | 以分钟为单位
volTimeLarge | volunteering time large | INTEGER | 实际获得的大型活动义工时间 | 0 | 以分钟为单位
thought | volunteering thought | TEXT | 义工感想 | blablabla | 长度几十字到百余字

### 3.5. `class_vol`表

这个数据表存储班级和义工活动之间的关系

项名 | 解释 | 类型 | 简述 | 举例 | 备注信息
-|-|-|-|-|-
volId |volunteering identity | INTEGER | 义工活动的编号 | 1 |
class | class | INTEGER | 班级的编号 | 202001 | 表示`volId`的义工活动，这个班级被允许参加且有分配人数
stuMax | student max | INTEGER | 这个班级被分配给多少名额 | 10 |
nowStuCount | now student count | INTEGER | 当前这个班级报名该义工的人数 | 5 |
