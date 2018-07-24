# 注册账号
## 访问https://aws.amazon.com/cn/注册账号

- [x] 要准备的材料visa信用卡 or master信用卡
> 注册过程中需要绑定信用卡，会扣$1(预授权)。但需要注意的是，在没有用超的情况下，是免费的，作为SS服务器，只能是流量超额会导致（15GB/月）
- [x] 开通国际长途业务的手机号
注册过程中，需要填写手机号，会有国际长途（手机号默认开通）打进来，告诉你验证码。

# 创建AWS实例
> 用刚才注册好的账号登录AWS控制台，点击EC2（云中的虚拟服务器）

3、定制服务器类型

4、勾选仅免费套餐

5、选择唯一的免费项目
底下有绿框描述
6、直接选蓝色按钮【审核和启动】
去创建密钥对

这时候会提示生成密钥对，这个很重要，一定要保存好，没有这个密钥对是无法远程登录管理你的服务器的，所以一定要保存好。
7、连接到服务器
定制完成后等几分钟，一般是在给你的服务器进行开发，等初始化完成后，就可以进行远程连接了
记住这里显示的公有IP地址，就是你的shadowsocks的服务器IP
8、点击连接会弹出对话框
9、以下过程以mac os系统为例
找到你在【第6步】创建的pem文件地址
9.1、使用chmod命令确保私有密钥不是公开可见的
chmod 400 /Users/Celery/.ssh/celerysoft.pem
9.2、通过ssh命令连接到服务器
sh -i /Users/Celery/.ssh/celerysoft.pem ec2-user@ec2-11-111-11-111.ap-northeast-1.compute.amazonaws.com
注意9.1和9.2都是在【第8步】弹出的对话框里会提示你如何连接的
登录成功后提示你保存密钥，你就都按yes就可以了
10、安装shadowsocks服务
10.1、切换为root用户
sudo su -
10.2、安装python
yum install -y python-setuptools
10.3、安装pip
easy_install pip
10.4、安装shadowsocks
pip install shadowsocks
10.5、配置shadowsocks.json
vi /etc/shadowsocks.json
{
 "server":"0.0.0.0",
 "server_port":端口,
 "local_address":"127.0.0.1",
 "local_port":1080,
 "password":"密码",
 "timeout":300,
 "method":"aes-256-cfb",
 "fast_open":false,
 "workers": 1
}
修改上面的中文：“端口” 和 “密码”，这两个将在之后使用手机连接ss时要使用到，请务必记下来
10.6、开启ss服务
ssserver -c /etc/shadowsocks.json -d start
如果找不到ssserver
使用which ssserver
找到ssserver绝对路径，类似下面：
/usr/local/bin/ssserver -c /etc/shadowsocks.json -d start
10.7、加到开机启动项

vi /etc/rc.local
sudo ssserver -c /etc/shadowsocks/ss.json -d start
11、控制台里面给服务添加安全组

12、下载客户端
当然你也可以到GitHub下载最新的客户端：
Windows客户端下载地址
macOS客户端下载地址
Linux客户端下载地址
12.1、配置shadowsocks客户端

12.2、配置Chrome的SwitchySharp扩展
选择私人VPN，开启网络之旅。如果要上国内网站，可选择Direct Connection，这样不走代理会更快。
12.3、踩坑提示
AWS 免费的服务器只有一台 12个月，切勿开启多台，因为那都是要钱的，不要问我怎么知道的！学费教的有点多
一台服务器每个月的流量是15G
在选择服务器地区的时候最好选择加州的，因为我有一次选择俄岗啥的，第二天就死活登不上去服务（我使用俄亥俄州没问题）
