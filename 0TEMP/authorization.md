# zvms的用户权限说明

| 操作 | 权限`1` | 权限`2` | 权限`3` | 权限`4` |
| --- | --- | --- | --- | --- |
| `user/login` | True | True | True | True |
| `user/logout` | True | True | True | True |
| `user/info` | True | True | True | True |
| `user/getInfo/\<userId>` | False | False | False | True |
| `student/volbook/\<stuId>` | 仅限本班的同学 | True | True | True |
| `class/list` | True | True | True | True |
| `class/stulist/\<classId>` | 仅限本班 | True | True | True |
| `class/volunteer/\<classId>` | 仅限本班 | True | True | True |
| `volunteer/list` | False | False | True | True |
| `volunteer/fetch/\<volId>` | 仅限与本班有关 | True | True | True |
| `volunteer/signup/\<volId>` | 仅限与本班有关 | False | False | False 接口待完善 |
| `volunteer/create` | False | True | False | True |
| `volunteer/audit/\<volId>` | False | False | True | True |
