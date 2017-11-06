# 代码审计
## phpmyadmin 密码修改
1. 更改用户中的密码
2. `config.inc.php`中password修改

## 变量覆盖漏洞
　　变量覆盖指的是用我们自定义的参数值替换程序原有的变量值，一般变量覆盖漏洞需要结合程序的其它功能来实现完整的攻击 变量覆盖漏洞大多数由函数使用不当导致，经常引发变量覆盖漏洞的函数有：extract(),parse_str()和import_request_variables()

## RCE(远程代码执行)
命令注入攻击中WEB服务器没有过滤类似system(),eval()，exec()等函数是该漏洞攻击成功的最主要原因。
