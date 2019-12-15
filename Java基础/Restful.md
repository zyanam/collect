### Rest 风格

URI: /资源名称/资源标识

|      | 普通CRUD                 | Restful         |
| ---- | ------------------------ | --------------- |
| 查询 | getEmp                   | emp-GET         |
| 添加 | addEmp?xxx               | emp-POST        |
| 修改 | updateEmp?id=xxx&xxx=xxx | emp/{id}-PUT    |
| 删除 | delEmp?id=1              | emp/{id}-DELETE |

