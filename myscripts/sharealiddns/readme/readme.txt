﻿======================================================================================
sharealiddns脚本说明文档：
======================================================================================
sharealiddns脚本是共享的，你可自由下载和使用，但不得用于商业目的，引用时请注明来源。
本脚本当初是放在国内的K&S论坛上方便大家下载和使用，但经常习惯性地被K&S屏蔽，这就不符合
共享精神了，所以决定放在全球知名的开源github上共大家下载和使用。个人看来，K&S后面的S
可以删除了，你觉得呢？
======================================================================================
脚本使用条件：
1、你必须到阿里云进行域名实名认证。
2、域名信息必须在阿里云中成功通过审核。
3、阿里云给你的key和cret，必须是正确而有效的。
4、你要有一个公网IPV4 IP并独占(有IPV6 IP环境除外)。
======================================================================================
脚本功能说明：
1、本脚本有增加，更新，删除，验证解析记录等功能。
2、本脚本支持N个域名解析。
3、主机别名支持直通符@, 通配符*或具体的别名。 
4、不支持中文域名。
======================================================================================
私有概念定义：  
1、光猫桥接，与之相联的路由用来拨号，则是一级路由，接在一级路由之后的为二级路由。
2、光猫拨号，则光猫为一级路由，联在光猫之后的路由是二级路由。
3、你可直接理解成：拨号的东东就是一级路由，其它是二级路由，三级路由...
======================================================================================
配置文件aliddns.conf中的pppoe_ifname参数设置：
1、对于二级路由(是路由)，pppoe_ifname必须设置为any。
2、对于一级路由(是光猫非路由)，pppoe_ifname必须也只能设置为any。
3、对于一级路由(非光猫是路由)，接入的是单线，则pppoe_ifname设置为wan0或auto，最好设置为auto。
4、对于一级路由(非光猫是路由)，接入的是双线，取wan0公网IP，则pppoe_ifname设置为wan0，取wan1公网IP，则pppoe_ifname设置为wan1。
5、对于一级路由(非光猫是路由)，采用多拨模式，我没有环境，所以没办法测试。
6、对于有多台路由环境，选择其中一台路由运行此脚本就可以，不可多台都运行此脚本，否则域名解析有可能发生冲突。
======================================================================================
配置文件aliddns.conf中的islog参数设置：
islog设置为1时开启脚本运行日志，设置为0时关闭脚本运行日志，日志文件在log目录内。
======================================================================================
配置文件aliddns.conf中定时更新域名时间参数设置：
    cron_Time_type:时间类型，可以设置为min或hour。
    cron_Time:当cron_Time_type设置为min时，可设置值为5~59，cron_Time_type设置为hour时，可设置为1~23。
======================================================================================
配置文件aliddns.conf中的isipv4_domain和isipv6_domain参数设置：
    isipv4_domain设置为1，执行IPV4域名解析，设置为0，不执行IPV4域名解析。
	isipv6_domain设置为1，执行IPV6域名解析，设置为0，不执行IPV6域名解析。
======================================================================================
配置文件aliddns.conf域名参数设置：
具体设置说明：
    1、aliddns_AccessKeyId和aliddns_AccessKeySecret，阿里给你的key和cret。
    2、routerddns_no，在有多个域名的情况下，用哪一个域名远程访问路由器。
    3、aliddns_name为主机别名，支持直通符@, 通配符*或具体的别名(如www, sss, yyy, m等)，不能设置为空值。
	4、aliddns_domain，为主机域名，如abc.com。
    5、aliddns_ttl，为解析有效生存时间，通常设置为600，企业用户可以设置更小值。
    6、aliddns_type，为记录类型，IPV4，必须设置为A，IPV6，必须设置为AAAA。
	7、aliddns_lan_mac，终端设备的MAC地址，为接在路由器后端的终端设备(NAS、PC等),提供域名解析，这个功能是专门为IPV6设置的，
	   对IPV4无效。
	   
只有一个域名IPV4网络的例子：
    aliddns_name="www"
    aliddns_domain="abc.com"
    aliddns_ttl="600"
    aliddns_type="A"
    aliddns_lan_mac=":"
	
有5个域名IPV4网络的例子(注意每行总列数必须都是5列，每列之间以空格隔开，下面的例子要求相同)：
    aliddns_name="www home office router my"
    aliddns_domain="abc.com abc.com abc.com abc.com abc.com"
    aliddns_ttl="600 600 600 600 600"
    aliddns_type="A A A A A"
    aliddns_lan_mac="none none none none none"

有多个域名IPV4网络，支持直通符@, 通配符*的例子：	
    aliddns_name="@ * www home office router my"
    aliddns_domain="abc.com abc.com abc.com abc.com abc.com abc.com abc.com"
    aliddns_ttl="600 600 600 600 600 600 600"
    aliddns_type="A A A A A A A"
    aliddns_lan_mac="none none none none none none none"

有多个域名IPV6网络，支持直通符@, 通配符*的例子：	
    aliddns_name="@ * www home office router my"
    aliddns_domain="abc.com abc.com abc.com abc.com abc.com abc.com abc.com"
    aliddns_ttl="600 600 600 600 600 600 600"
    aliddns_type="AAAA AAAA AAAA AAAA AAAA AAAA AAAA"
    aliddns_lan_mac="none none none none none none none"	
 
为终端设备提供IPV6域名解析的例子(aliddns_name必须有一个具体的值，如nas，home，两个终端设备网卡MAC假设是
    33:D7:7A:2B:DE:6B和65:D0:7B:2A:DE:4A)：
    aliddns_name="@ * www home office router my nas"
    aliddns_domain="abc.com abc.com abc.com abc.com abc.com abc.com abc.com abc.com"
    aliddns_ttl="600 600 600 600 600 600 600 600"
    aliddns_type="AAAA AAAA AAAA AAAA AAAA AAAA AAAA AAAA"
    aliddns_lan_mac="none none none 33:D7:7A:2B:DE:6B none none none 65:D0:7B:2A:DE:4A"	
	
    远程访问：
	    终端1: http://home.abc.com(有证书：https://home.abc.com)
		终端2: http://nas.abc.com(有证书：https://nas.abc.com)
	如果域名解析已经成功，但终端设备还是无法访问，这是因为win操作系统对IPV6支持还不完善，你需要在windows command下运行：
        ipconfig/flushdns

路由器同时支持IPV4和IPV6双网络的例子：	
    aliddns_name="www home office router my"
    aliddns_domain="abc.com abc.com abc.com abc.com abc.com"
    aliddns_ttl="600 600 600 600 600"
    aliddns_type="A AAAA A AAAA A"
    aliddns_lan_mac="none none none none none"

路由器有不同域名IPV4网络的例子：	
    aliddns_name="www home office router my"
    aliddns_domain="aaa.com bbb.com ccc.com ddd.com eee.com"
    aliddns_ttl="600 600 600 600 600"
    aliddns_type="A A A A A"
    aliddns_lan_mac="none none none none none"

路由器有不同域名IPV6网络的例子：	
    aliddns_name="www home office router my"
    aliddns_domain="aaa.com bbb.com ccc.com ddd.com eee.com"
    aliddns_ttl="600 600 600 600 600"
    aliddns_type="AAAA AAAA AAAA AAAA AAAA"
    aliddns_lan_mac="none none none none none"   
======================================================================================
======================================================================================
安装方法：
    准备好winscp和xshell工具软件，或国产FinalShell免费版：
	在xshell或FinalShell中粘贴下面指令：
	cd /tmp/;wget --no-check-certificate -O /tmp/install.sh https://raw.githubusercontent.com/zwmscorm/sharealiddns/master/myscripts/install.sh;sh /tmp/install.sh
    然后按提示进行操作：
	1、将脚本安装到jffs，则输入jffs，将脚本安装到usb，则输入usb，删除脚本，则输入uninstall。	
	2、接下来是设置aliddns.conf参数，请按上面的要求认真填写。
	3、接下来脚本会自动运行，请注意观察脚本运行情况。如出现错误，会有提示。
	   如果aliddns.conf参数设置错误，请运行下面的脚本(假设脚本安装到jffs)，重新输入：
	        sh /jffs/myscripts/sharealiddns/etc/init.d/sharealiddns.sh setconf
		    或者直接进入/jffs/myscripts/sharealiddns/conf目录对aliddns.conf进行修改。
            大部分情况都是参数设置不正确，使得脚本运行出现错误。
    4、至此，脚本已经安装完毕。
======================================================================================
    脚本一些功能调用(假设脚本安装到jffs)：
	修改或设置aliddns.conf参数：
	    sh /jffs/myscripts/sharealiddns/etc/init.d/sharealiddns.sh setconf
    检测aliddns.conf所有域名是否成功解析：
        sh /jffs/myscripts/sharealiddns/etc/init.d/sharealiddns.sh check
		或
		sh /jffs/myscripts/sharealiddns/etc/init.d/sharealiddns.sh check 0
    检测aliddns.conf第3个域名是否成功解析：
        sh /jffs/myscripts/sharealiddns/etc/init.d/sharealiddns.sh check 3
    增加或更新aliddns.conf所有域名:
        sh /jffs/myscripts/sharealiddns/etc/init.d/sharealiddns.sh restart	
		或 
		sh /jffs/myscripts/sharealiddns/etc/init.d/sharealiddns.sh restart 0
    增加或更新aliddns.conf第3个域名:
        sh /jffs/myscripts/sharealiddns/etc/init.d/sharealiddns.sh restart 3
    删除aliddns.conf所有域名:
        sh /jffs/myscripts/sharealiddns/etc/init.d/sharealiddns.sh remove 0
		或
		sh /jffs/myscripts/sharealiddns/etc/init.d/sharealiddns.sh removeall
    删除aliddns.conf第3域名:
        sh /jffs/myscripts/sharealiddns/etc/init.d/sharealiddns.sh remove 3
    设置aliddns.conf第3域名解析记录为禁用
        sh /jffs/myscripts/sharealiddns/etc/init.d/sharealiddns.sh status 3 0
    设置aliddns.conf第3域名解析记录为启用
        sh /jffs/myscripts/sharealiddns/etc/init.d/sharealiddns.sh status 3 1
	设置aliddns.conf所有域名解析记录为启用
        sh /jffs/myscripts/sharealiddns/etc/init.d/sharealiddns.sh status 0 1
	设置aliddns.conf所有域名解析记录为禁用
        sh /jffs/myscripts/sharealiddns/etc/init.d/sharealiddns.sh status 0 0
    对aliddns.conf所有域名进行兼容性和压力测试(开发模式专用)：
        sh /jffs/myscripts/sharealiddns/etc/init.d/sharealiddns.sh monitor
    对aliddns.conf第3域名进行兼容性和压力测试(开发模式专用)：
        sh /jffs/myscripts/sharealiddns/etc/init.d/sharealiddns.sh monitor 3
	对aliddns.conf所有域名进行兼容性和压力测试(开发模式专用)：
        sh /jffs/myscripts/sharealiddns/etc/init.d/sharealiddns.sh monitor
	检测客户端状态：
	    sh /jffs/myscripts/sharealiddns/etc/init.d/sharealiddns.sh client ipv4
		或
		sh /jffs/myscripts/sharealiddns/etc/init.d/sharealiddns.sh client ipv6
	检测公网IP：
	    sh /jffs/myscripts/sharealiddns/etc/init.d/sharealiddns.sh checkwanip
	显示日志：
	    sh /jffs/myscripts/sharealiddns/etc/init.d/sharealiddns.sh showlog
======================================================================================
======================================================================================



