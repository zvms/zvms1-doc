# 数据库规范
## user

这个表只储存可以登录的用户，预计每个班和每个老师分配一个账户，义管会、实践部另算

项名 |备注 | 类型 | 简述 | 举例 | 其他 
-|-|-|-|-|-
userId |user identity | INTEGER | 储存用户的id | 202001 | 不需要自动递增， 唯一， 理论上六位即可
userName |user name | CHAR(64) | 储存用户的名字 | 王彳亍 | 长度不知道要多少，凭感觉来:-D
class |class | INTEGER | 储存用户拥有的班级id | 202001 | 理论长度其实也是六位就够
permission |permission | SMALLINT | 储存用户的身份权限 | 0 | 1:团支书 2: 教师 3: 义管会 4: 系统 权限等级待定
password |password | CHAR(255) | 储存密码 | aababababab | 记得加密，方式待定

## student

这个表存储每个学生的身份以及部分义工信息

项名 |备注 | 类型 | 简述 | 举例 | 其他
-|-|-|-|-|-
stuId |student identity | INTEGER | 储存学生的学号 | 20200101 | 不需要自动递增， 唯一， 理论上八位即可
stuName |student name | CHAR(64) | 储存学生的名字 | 王彳亍 | 长度不知道要多少，凭感觉来:-D
volTimeInside |volunteering time inside | INTEGER | 储存学生的义工时间 | 0 | 避免浮点运算，在数据库中以分钟为单位
volTimeOutside |volunteering time outside | INTEGER | 储存学生的义工时间 | 0 | 以分钟为单位
volTimeLarge |volunteering time large | INTEGER | 储存学生的义工时间 | 0 | 以分钟为单位

## volunteer

这个表存储每次义工活动

项名 |备注 | 类型 | 简述 | 举例 | 其他
-|-|-|-|-|-
volId |volunteering identity | INTEGER | 义工活动的唯一确定编号 | 1 | 其实这个自动递增倒也无所谓
volName |volunteering name | CHAR(255) | 义工活动的名称 | 喂孔子+拜锦鲤 | 长度不知道要多少，凭感觉来:-D
volDate |volunteering date | DATE | 义工活动的日期 | 2020.9.24 | 长度不知道要多少，凭感觉来:-D
volTime |volunteering time | TIME | 义工活动的时间 | 13:00 | 长度不知道要多少，凭感觉来:-D
stuMax |maximum students | INTEGER | 义工活动的人数上限 | 10 | 
nowStuCount |now student | INTEGER | 义工活动的现有人数 | 8 |
description |description | TEXT | 义工活动的描述 | blablablabla | 长度不知道要多少，凭感觉来:-D
status |status | SMALLINT | 义工活动的状态 | 0 | `0`表示已经结束，`1`表示还没开始，`2`表示正在进行
volTimeInside |volunteering time inside | INTEGER | 每个人预计将获得的义工时间 | 0 | 以分钟为单位
volTimeOutside |volunteering time outside | INTEGER | 每个人预计将获得的义工时间 | 0 | 以分钟为单位
volTimeLarge |volunteering time large | INTEGER | 每个人预计将获得的义工时间 | 0 | 以分钟为单位
holderId |volunteer holder\'s id | INTEGER | 义工发布者的`id` | 20200101 | 

## stu_vol

这个表存储学生和义工之间的关系

项名 |备注 | 类型 | 简述 | 举例 | 其他
-|-|-|-|-|-
volId |volunteering identity | INTEGER | 义工活动的编号 | 1 | 表示`stuId`的学生参加了这个义工活动 
stuId |student identity | INTEGER | 学生的学号 | 20200101 | 表示这个学生参加了`volId`的义工活动 
status |status | SMALLINT | 审核状态 | 0 | `0`表示未通过，`1`表示通过，`2`表示审核中
volTimeInside |volunteering time inside | INTEGER | 实际获得的义工时间 | 0 | 以分钟为单位
volTimeOutside |volunteering time outside | INTEGER | 实际获得的义工时间 | 0 | 以分钟为单位
volTimeLarge |volunteering time large | INTEGER | 实际获得的义工时间 | 0 | 以分钟为单位
thought |volunteering thought | TEXT | 义工感想 | 吃鸡！ | 长度几十字到百字

## class_vol

这个表存储义工活动分配给哪些班级

项名 |备注 | 类型 | 简述 | 举例 | 其他
-|-|-|-|-|-
volId |volunteering identity | INTEGER | 义工活动的编号 | 1 | 
class |class | INTEGER | 班级的编号 | 202001 | 表示`volId`的义工活动，这个班级被允许参加 
stuMax |student max | INTEGER | 这个班级被分配给多少名额 | 10 |  
