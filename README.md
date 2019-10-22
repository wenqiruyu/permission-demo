# permission-demo

1.下载
在线安装
yum -y install python-setuptools
easy_install supervisor
echo_supervisord_conf > /etc/supervisord.conf
mkdir -p /var/log/supervisor
mkdir -p /etc/supervisor/conf.d/
echo -e "[include]\nfiles = /etc/supervisor/conf.d/*.conf" >> /etc/supervisord.conf
supervisord -c /etc/supervisord.conf

supervisor为python编写，可以选择pip安装
pip install supervisor
echo_supervisord_conf > /etc/supervisord.conf

离线安装
wget https://pypi.python.org/packages/44/80/d28047d120bfcc8158b4e41127706731ee6a3419c661e0a858fb0e7c4b2d/supervisor-3.3.0.tar.gz
2.解压
tar zxf supervisor-3.3.0.tar.gz
3.进入目录
cd supervisor-3.3.0
4.安装
python setup.py install
5.检查是否安装成功
登陆python控制台输入import supervisor 查看是否能成功加载
6.生成配置文件(supervisord.conf)：
我的是安在了/usr/local/python27/bin/下，进入该目录，可以执行以下命令
echo_supervisord_conf > /etc/supervisord.conf
启动supervisor
supervisord -c /etc/supervisord.conf
查看状态
supervisorctl status
启动全部
supervisorctl start all
停止全部
supervisorctl stop all
以下是样例配置文件
supervisord.conf
; supervisor config file

[unix_http_server]
file=/var/run/supervisor.sock ; (the path to the socket file)
chmod=0700 ; sockef file mode (default 0700)

[supervisord]
logfile=/var/log/supervisord.log ; (main log file;default $CWD/supervisord.log)
pidfile=/var/run/supervisord.pid ; (supervisord pidfile;default supervisord.pid)
childlogdir=/var/log/ ; ('AUTO' child log dir, default $TEMP)

; the below section must remain in the config file for RPC
; (supervisorctl/web interface) to work, additional interfaces may be
; added by defining them in separate rpcinterface: sections
[rpcinterface:supervisor]
supervisor.rpcinterface_factory = supervisor.rpcinterface:make_main_rpcinterface

[supervisorctl]
serverurl=unix:///var/run/supervisor.sock ; use a unix:// URL for a unix socket

; The [include] section can just contain the "files" setting. This
; setting can list multiple files (separated by whitespace or
; newlines). It can also contain wildcards. The filenames are
; interpreted as relative to this file. Included files *cannot*
; include files themselves.

[include]
files = /etc/supervisor/conf.d/*.conf
server.conf
[program:server] command=python /opt/archive_server/server.py autorstart=true redirect_stderr=true stdout_logfile=/mnt/log/supervisord_server.log directory=/tmp user=root priority=1
