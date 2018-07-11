# ShadowsockR
此为转存,感谢原作者
源链接：

https://raw.githubusercontent.com/teddysun/across/master/bbr.sh
https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocksR.sh
https://github.com/teddysun/shadowsocksr/archive/manyuser.zip
https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocksR
https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocksR-debian

**********************************

## 安装过程

### 部署 SSR

#### 执行：

```Shell
wget https://raw.githubusercontent.com/sklun/ShadowsockR/master/shadowsocksR.sh    //获取脚本 
```    
>>若提示 "wget:command not found";

>>执行
```Shell
	yum install wget -y    //安装wget
```
```Shell
chmod +x shadowsocksR.sh    //赋予执行权限
```
```Shell
./shadowsocksR.sh 2>&1 | tee shadowsocksR.log    //执行安装脚本
```
{

	Please enter password for ShadowsocksR:
	(Default password: teddysun.com):zheshimima   //输入密码

	---------------------------
	password = zheshimima
	---------------------------

	Please enter a port for ShadowsocksR [1-65535]:
	(Default port: 8989):2333    //输入端口

	---------------------------
	port = 2333
	---------------------------
    
	Please select stream cipher for ShadowsocksR:
	1) none
	2) aes-256-cfb
	3) aes-192-cfb
	4) aes-128-cfb
	5) aes-256-cfb8
	6) aes-192-cfb8
	7) aes-128-cfb8
	8) aes-256-ctr
	9) aes-192-ctr
	10) aes-128-ctr
	11) chacha20-ietf
	12) chacha20
	13) rc4-md5
	14) rc4-md5-6
	Which cipher you'd select(Default: aes-256-cfb):12   //选择加密方式

	---------------------------
	cipher = chacha20
	---------------------------

	Please select protocol for ShadowsocksR:
	1) origin
	2) verify_deflate
	3) auth_sha1_v4
	4) auth_sha1_v4_compatible
	5) auth_aes128_md5
	6) auth_aes128_sha1
	7) auth_chain_a
	8) auth_chain_b
	Which protocol you'd select(Default: origin):3  //选择协议

	---------------------------
	protocol = auth_sha1_v4
	---------------------------

	Please select obfs for ShadowsocksR:
	1) plain
	2) http_simple
	3) http_simple_compatible
	4) http_post
	5) http_post_compatible
	6) tls1.2_ticket_auth
	7) tls1.2_ticket_auth_compatible
	8) tls1.2_ticket_fastauth
	9) tls1.2_ticket_fastauth_compatible
	Which obfs you'd select(Default: plain):6   //选择混淆

	---------------------------
	obfs = tls1.2_ticket_auth
	---------------------------


	Press any key to start...or Press Ctrl+C to cancel   //开始部署
}

#### 部署完毕后

{

	Congratulations, ShadowsocksR server install completed!
	Your Server IP        :  106.46.46.150 
	Your Server Port      :  2333 
	Your Password         :  zheshimima 
	Your Protocol         :  auth_sha1_v4 
	Your obfs             :  tls1.2_ticket_auth 
	Your Encryption Method:  chacha20 
}

**********************************
### 安装Google BBR加速

#### 执行:

```Shell
wget https://raw.githubusercontent.com/sklun/ShadowsockR/master/bbr.sh
```
```Shell
chmod +x bbr.sh
```
```Shell
./bbr.sh
```
#### 检验是否安装完成
```Shell
uname -r
```
		查看内核版本，含有 4.9.0 就表示 OK 了
```Shell
sysctl net.ipv4.tcp_available_congestion_control
```
		返回值一般为：net.ipv4.tcp_available_congestion_control = bbr cubic reno
```Shell
sysctl net.ipv4.tcp_congestion_control
```
		返回值一般为：net.ipv4.tcp_congestion_control = bbr
```Shell
sysctl net.core.default_qdisc
```
		返回值一般为：net.core.default_qdisc = fq
```Shell
lsmod | grep bbr
```
		返回值有 tcp_bbr 模块即说明bbr已启动

### 附录

#### VPS

* 显示所有进出链接
```Shell
netstat -anp |grep 'ESTABLISHED' |grep 'python'
```
* 仅显示链接服务器的用户连接 
```Shell
netstat -anp |grep 'ESTABLISHED' |grep 'python' |grep 'tcp6'
```
* 仅显示链接服务器的用户连接数量
```Shell
netstat -anp |grep 'ESTABLISHED' |grep 'python' |grep 'tcp6' |wc -l
```

#### SSR 
* 启动
```Shell
/etc/init.d/shadowsocks start
```
* 停止
```Shell	
/etc/init.d/shadowsocks stop
```
* 重启
```Shell	
/etc/init.d/shadowsocks restart
```
* 查看状态
```Shell
/etc/init.d/shadowsocks status
```
* 配置文件路径：
	
	/etc/shadowsocks.json

* 日志文件路径：
	
	/var/log/shadowsocks.log

* 代码安装目录：

	/usr/local/shadowsocks	
	
* 重定义命令	//设不设都行
```Shell
echo -e "alias startssr='/etc/init.d/shadowsocks start'\nalias stopssr='/etc/init.d/shadowsocks stop'\nalias restartssr='/etc/init.d/shadowsocks restart'\nalias statusssr='/etc/init.d/shadowsocks status'" >> ~/.bashrc && source ~/.bashrc
```
* 设置后：
 >启动
```Shell
	startssr
```
 >停止
```Shell
	stopssr
```
 >重启
```Shell
	restartssr
```
 >查看状态
```Shell
	statusssr
```
* 显示当前所有链接SS的用户IP
```Shell
netstat -anp |grep 'ESTABLISHED' |grep 'python' |grep 'tcp6' |awk '{print $5}' |awk -F ":" '{print $1}' |sort -u
```
* 显示当前所有链接SS的用户IP数量
```Shell
netstat -anp |grep 'ESTABLISHED' |grep 'python' |grep 'tcp6' |awk '{print $5}' |awk -F ":" '{print $1}' |sort -u |wc -l
```
* 多端口
```Shell
netstat -anp |grep 'ESTABLISHED' |grep 'python' |grep 'tcp6' |grep 222.233.22.22:2222  /*yourIp:yourPort*/
```
```Shell
netstat -anp |grep 'ESTABLISHED' |grep 'python' |grep 'tcp6' |grep 222.233.22.22:3333  /*yourIp:yourPort*/
```
##### SSR修改密码、配置多端口
```Shell
vi /etc/shadowsocks.json
```
{
    
	"server": "0.0.0.0",    
	"server_ipv6": "::",
	"local_address": "127.0.0.1",
	"local_port": 1081,
	"port_password":{
		"port1":"passwd1",
		"port2":"passwd2",
		"port3":"passwd3",
		"port4":"passwd4"
	},
	
}    
```Shell
/etc/init.d/shadowsocks restart    //重启ssr
```
###### 如不能联网，则关闭防火墙    //一般不要关
```Shell
service iptables stop
```    
```Shell
chkconfig iptables off 
``` 
