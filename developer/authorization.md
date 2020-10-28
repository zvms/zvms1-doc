# zvms的用户权限说明

| 操作 | 权限`0` | 权限`1` | 权限`2` | 权限`3` |
| --- | --- | --- | --- | --- |
| `user/login` | True | True | True | True |
| `user/logout` | True | True | True | True |
| `user/info` | True | True | True | True |
| `user/getInfo/\<userId> | False | ? | ? | True |
| `student/volbook/\<stuId>` | 仅限本班的同学 | True | True | True |
