# 一些函数
## extract()
数从数组中将变量导入到当前的符号表。

```php
<?php
$a = "Original";
$my_array = array("a" => "Cat","b" => "Dog", "c" => "Horse");
extract($my_array);
echo "\$a = $a; \$b = $b; \$c = $c";
?>
```
## parse_str()
将查询到的字符串解析到变量中

```php
<?php
parse_str("name=Bill&age=60");
echo $name."<br>";
echo $age;
?>
```

##i mport_request_variables()
将 GET／POST／Cookie 变量导入到全局作用域中
