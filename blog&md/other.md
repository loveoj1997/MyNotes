# NTFS ADS流
这是一种``隐藏文件``的方法
## 创建数据流
    c:\test>echo Hello, freebuf! > test.txt:ThisIsAnADS
## 列出数据流
c:\test>dir /r test.txt
## 查看ADS内容
PS C:\test> Get-Content test.txt -stream ThisIsAnADS

# png

## png图片格式http://www.cnblogs.com/fengyv/archive/2006/04/30/2423964.html

## 查看IDAT数据块
pngcheck.exe -v xxx.png 正常一个快是65524

#zlib
78 9C是zlib压缩标志
zlib扩展阅读http://zlib.net/

test