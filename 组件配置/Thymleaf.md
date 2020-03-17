### 格式化日期，时间

```html
<td th:text="${#calendars.format(employee.gmtCreate,'yyyy-MM-dd HH:mm:ss')}">创建时间</td>
```

### Each迭代循环

```html
<tr th:each="employee : ${employees}">
    <td th:text="${employeeStat.count}">1</td>
    <td><input type="checkbox"></td>
    <td th:text="${employee.username}">姓名</td>
    <td th:text="${employee.gender == 1 ? '男' : '女'}">性别</td>
    <td th:text="${employee.phoneNumber}">电话号码</td>
    <td th:text="${employee.enable == 1 ? '启用' : '禁用'}">账号状态</td>
    <td>角色&权限</td>
    <td th:text="${#calendars.format(employee.gmtCreate,'yyyy-MM-dd HH:mm:ss')}">创建时间</td>
    <td>
        <button type="button" class="btn btn-primary btn-xs"><i
                                                                class=" glyphicon glyphicon-pencil"></i></button>
        <button type="button" class="btn btn-danger btn-xs"><i
                                                               class=" glyphicon glyphicon-remove"></i></button>
    </td>
</tr>
```

### 三元运算

```html
<td th:text="${employee.gender == 1 ? '男' : '女'}">性别</td>
```

### 自带序号

```html
<td th:text="${employeeStat.count}">1</td>
```

