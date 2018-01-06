# 华硕路由器
## 1 准备

	硬件：Ac86u，固件版本： 382.1.2（官固，梅林均可），开启SSH（必备）
	内存：官宣512M，实际430M左右，开机即占用70%左右，剩余140M左右，坑。
	
	<1>JFFS：

	#nvram set jffs2_on=1
	#nvram set jffs2_enable=1
	#nvram set jffs2_format=1
	#nvram set jffs2_scripts=1
	#nvram commint
	#reboot

	<2>udisk：
	
	#df -h
	#umount /tmp/mnt/sda1
	#mkfs.ext3 /dev/sda1		
	#mkdir -p /tmp/mnt/sda1
	#mount /dev/sda1 /tmp/mnt/sda1
	
	#optional
	#dd if=/dev/zero of=/tmp/mnt/sda1/swapfile bs=1024 count=524292
	#mkswap /tmp/mnt/sda1/swapfile
	#chmod 600 /tmp/mnt/sda1/swapfile
	#swapon /tmp/mnt/sda1/swapfile

	
## 2 entware

	#mkdir -p /tmp/mnt/sda1/opt
	#mkdir -p /tmp/opt
	#mount -o bind /tmp/mnt/sda1/opt /tmp/opt
	#cd /opt
	#wget -O - http://pkg.entware.net/binaries/armv7/installer/entware_install.sh | sh
	
	#opkg install shadowsocks-libev-ss-redir
	#opkg install dnsmasq-full
	#opkg install rng-tools


## 3 dns server

### 3.1 dnsmasq
	192.168.0.1,192.168.1.1 etc.

	#genpac --format=dnsmasq --dnsmasq-dns="192.168.*.1#53" --dnsmasq-ipset "gfwlist" -o gfwlist.conf

	#scp gfwlist.conf routeadmin@192.168.*.1:/opt/etc/dnsmasq.d

	/opt/etc/dnsmasq.d/gfwlist.conf

	/opt/etc/dnsmasq.conf
	/opt/etc/resolv.conf
	/opt/etc/resolv.dnsmasq.conf

	ipset -N gfwlist iphash
	iptables -t nat -A PREROUTING -p tcp -m set --match-set gfwlist dst -j REDIRECT --to-port 1080

	start dnsmasq：/opt/etc/init.d/S**dnsmasq restart|start|stop

	/opt/sbin/rngd -f -r /dev/urandom

### 3.2 unbound+dnscrypt-proxy
	https://wido.me/sunteya/use-openwrt-resolve-gfw-dns-spoofing

	https://blog.phoenixlzx.com/2016/04/27/better-dns-with-unbound/
	https://cokebar.info/archives/246

## 4 ss
	server: vultr,Los Angeles.(tokyo,singapore dead now),python or libev
	2018-01-6:switch to sydney
	
	

	route client: ss-redir -c /opt/etc/shadowsocks.json -b 0.0.0.0
	

	

	
	

