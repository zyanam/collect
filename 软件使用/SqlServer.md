### 批量导出csv

> 使用游标按小区导出数据，每个小区一个csv文件。
>
> 导出的csv文件中的身份证号以科学计数法显示，解决这个问题需要让csv的字段分隔符逗号＋Tab。
>
> 需要开启 xp_cmdsehll 的执行权限（见下面）

```mssql
declare @village_id int,@village_name varchar(20),@sql  varchar(3000)
declare cur_village cursor scroll
for
	select village_id,village_name from village where village_parent = 0
open cur_village

fetch first from cur_village into @village_id,@village_name
while @@FETCH_STATUS=0
begin 

	if @village_id = 5
	begin 
		set @sql = 'select v.village_name,v1.village_name,c = floor_name + ''-'' + cast(h.unit as varchar(10)) + ''-'' + h.door,h.owner,h.mobile,h.card_no  from village v left join house h on v.village_id = h.village_id left join (select village_name,village_id from village where village_parent = '+cast(@village_id as varchar(10))+') v1 on  h.village_son_id = v1.village_id left join floor f on h.village_id = f.village_id and h.floor_id = f.floor_id where h.state = 3 and h.village_id = '+cast(@village_id as varchar(10))+' order by v1.village_id, f.floor_name'
	end
	else if @village_id = 67
	begin 
		set @sql = 'select v.village_name,v1.village_name,c = floor_name + ''-'' + cast(h.unit as varchar(10)) + ''-'' + h.door,h.owner,h.mobile,h.card_no  from village v left join house h on v.village_id = h.village_id left join (select village_name,village_id from village where village_parent = '+cast(@village_id as varchar(10))+') v1 on  h.village_son_id = v1.village_id left join floor f on h.village_id = f.village_id and h.floor_id = f.floor_id where h.state = 3 and h.village_id = '+cast(@village_id as varchar(10))+' order by v1.village_id, f.floor_name'
	end
	else if @village_id = 82
	begin 
		set @sql = 'select v.village_name,v1.village_name,c = floor_name + ''-'' + cast(h.unit as varchar(10)) + ''-'' + h.door,h.owner,h.mobile,h.card_no  from village v left join house h on v.village_id = h.village_id left join (select village_name,village_id from village where village_parent = '+cast(@village_id as varchar(10))+') v1 on  h.village_son_id = v1.village_id left join floor f on h.village_id = f.village_id and h.floor_id = f.floor_id where h.state = 3 and h.village_id = '+cast(@village_id as varchar(10))+' order by v1.village_id, f.floor_name'
	end
	else
	begin
		set @sql = 'select v.village_name as 小区,房号=(floor_name + ''-'' + cast(h.unit as varchar(10))+''-''+h.door),h.owner as 业主,h.mobile,h.card_no from sxwy.dbo.village v left join sxwy.dbo.house h on v.village_id = h.village_id left join sxwy.dbo.floor f on h.village_id = f.village_id and h.floor_id = f.floor_id where h.state = 3 and h.village_id = '+cast(@village_id as varchar(10))+' order by f.floor_name'
	end

	set @sql = 'bcp "' + @sql + '" queryout "D:\datafile\'+@village_name+'.csv" -c -q -S"localhost" -U"sxwy" -P"dH#$%h@!(_123" -d"sxwy" -t",\t" '
	
	--exec(@sql)
	--print @sql
	exec master..xp_cmdshell @sql 

	fetch next from cur_village into @village_id,@village_name
end
close cur_village
deallocate cur_village
```



### 开启 xp_cmdsehll 的执行权限

```mssql
--查询状态
SELECT * FROM sys.configurations WHERE name='xp_cmdshell' OR name='show advanced options'
GO
```

![](..\imgs\TIM截图20200220195638.png)

```mssql
--打开设置
USE master
GO
RECONFIGURE --先执行一次刷新，处理上次的配置
GO
EXEC sp_configure 'show advanced options',1 --启用xp_cmdshell的高级配置
GO
RECONFIGURE --刷新配置
GO
EXEC sp_configure 'xp_cmdshell',1  --打开xp_cmdshell,可以调用SQL系统之外的命令
GO
RECONFIGURE
GO
--使用xp_cmdshell在D盘创建一个myfile 文件夹
EXEC xp_cmdshell 'mkdir d:\myfile',no_output --[no_output]表示是否输出信息
GO
```

```mssql
--关闭设置
EXEC sp_configure 'show advanced options','1' --确保show advances options 的值为1,这样才可以执行xp_cmdshell为0的操作
GO
RECONFIGURE
GO
EXEC sp_configure 'xp_cmdshell',0 --关闭xp_cmdshell
GO
RECONFIGURE
GO
EXEC sp_configure 'show advanced options','0' --关闭show advanced options
GO
RECONFIGURE
GO
```

