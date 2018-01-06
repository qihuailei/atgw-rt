sudo yum install python-setuptools && easy_install pip
sudo pip install shadowsocks

systemctl start firewalld.service
firewall-cmd --zone=public --add-port=443/tcp --permanent
firewall-cmd --reload

sudo ssserver -p 443 -k ssspgz -m aes-256-cfb --user nobody -d start
