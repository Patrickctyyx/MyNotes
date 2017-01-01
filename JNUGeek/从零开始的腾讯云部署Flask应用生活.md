### 2016-09-08 12:00:34开通腾讯云服务器
说实话，这个时候我对云服务器的概念还是一脸懵逼的，我所有的认识，就是只要把应用放在上面，从外面就可以访问了，抱着这个简单的想法我就开始做准备活动了，没想到这项工作居然会如此浩大，如此艰难。

刚开始我安装的系统是CentOS7.2，因为都是Linux，这个比较靠前，腾讯云上面相应的文档又比较多于是乎就这样了。然而我没想到的是，同样是Linux的发行版，Ubuntu和CentOS居然有如此大的差别。CentOS上好像没有
Python3.X的版本，于是第一个任务就是安装Python3.5。其实Linux下安装还是比较简单的，wget下载压缩包，tar解压，再就是安装。

然而，由于系统自带的是Python2.7，在用3.5替换的时候遇到了一些问题，由于yum是用Python2.7运行的，替换之后有些问题，而且3.5也有些问题，解决起来不容易，于是我菜月Patrick就用出了死亡回归——重装系统。

### 2016-09-08 15:21:48重装了CentOS7.2
这次还是一样先下载了Pytho3.5，然而在操作的过程中电脑突然出问题，键盘不能用，云服务器的工作不得不停止了。停了三天之后问题终于逐渐解决，windows彻底不能用，Ubuntu上的网络问题也已经解决，于是又开始了。然而，由于折腾Linux比较累了，于是我就准备把腾讯云的工作交给郑洋来做了。

但和我一样，郑洋也同样遇到了相当大的阻力，于是我还是自己装了Python3.5。但是yum还是有问题，尽管我已经把yum文件头部的路径该成了python3，。这时候我已经不再管这个了，Python基本搞定，接下来就是装MySQL了，装的时候又有各种问题。这个时候在网上搜教程的时候突然发现腾讯云主机中有那种已经集成了环境的服务主机，于是狠下新来又再次主动死亡回归了——第二次重装系统。

### 2016-09-12 10:14:02重装了CentOS6.5
重装了之后，我就安心的把东西交给了郑洋。这次选择的系统是已经有MySQL5.6的，于是要做的就是装Python3.5了，然而似乎是系统的问题，之前装3.5的教程在这个系统上就各种报错，下载的.tar.xz文件都无法解析，试了很久无奈之下又准备找另一个集成了MySQL的更新的系统。就这样，我又开始了新一次的死亡回归——第三次重装系统。

### 2016-09-12 15:27:08重装了CentOS7.0
有了前面几次的经验，这次操作起来就是飞快了，3.5，数据库，所需模块，全都安装完毕,用python3 admin.py run也可以顺利的跑起来，然后就没有然后了。本以为这样跑起来就可以了，但是后来才发现并没有这么容易，这时候在网上搜了一些，发现了要想部署要经过很长一些步骤。再加上本地运行又有一些莫名其妙的错误于是就暂时放下来了。直到晚上试了N种方法不成功后抱着碰运气的想法运行了一个命令后终于成功运行了！简直开心爆了，一下子把这几天的不爽都冲消了大半。

> 附：本地MySQL语法错误问题解决方法
> 
> 原因：MySQLdb没有装好
> - 由于MySQLdb官方只支持到2.7，于是在3.5的环境下就会因为兼容性而有问题
> 
> 解决方法：
> 1. sudo apt-get install libmysqlclient-dev(ubuntu)
> 2. sudo pip3 install mysqlclient
> 
> 以上后MySQldb就可以完美的运行了

第二天又开始搞云主机了，根据志平的建议，还有就是因为教程的步骤太多太麻烦不想去试验于是就按照腾讯云的文档安装了Nginx，但当我根据他的教程来修改数据的时候，显示的却并不是他出现的那样，一顿修改之后发现各种报错而且还不能用，而且卸载再安装又有很多莫名其妙的错误，对我来说错误已经不可修复，然后就是另一次的死亡回归了——第四次重装系统。

### 2016-09-13 13:18:06重装了Ubuntu 14.04 LTS
当我准备再次重装系统时，我向下翻，希望能找到一个更适合Python的环境，于是我找到了Ubuntu 14.04。用起来感觉确实比CentOS省心多了。集成了Python3.4，以及MySQL，再加上强大的apt，用起来感觉就是不一样。

这次我是下定决心跟着一个看起来比较靠谱的教程来做的，安装虚拟环境，安装所需的模块，安装配置uwsgi,安装配置Nginx，安装配置Supervisor。一切的一切，都是按照教程来的，但又有各种莫名奇妙的错误。uwsgi无法启动admin.py，使用一样的端口会显示端口已占用，使用其他的端口会显示找不到callable，于是又再次卡在这里了。这时候我觉得可能是因为环境安装的太杂，所以有一些冲突。因为安装虚拟环境的时候还不是很会，出了一些问题。这时候我又不愿意再次重装系统，因为胜利距离我就几步之遥了，这个时候我就决定先借用吕方的云主机了。也就是说，另一种方式的死亡回归——第五次重装系统。

> 附：对virtualenv+uwsgu+Nginx+Supervisor理解以及配置
> 
> #### virtualenv
> - 安装虚拟环境sudo pip3 install
> - 新建名为jnugeek的虚拟环境virtualenv jnugeek(注意，这里不要用sudo，不然后面涉及到权限问题很崩溃的，默认使用系统模块，如果不想用系统模块就在jnugeek前面加上--no-site-packages,另一种说法：默认情况下虚拟环境不会依赖系统环境的global site-packages。比如系统环境里安装了MySQLdb模块，在虚拟环境里import MySQLdb会提示ImportError。如果想依赖系统环境的第三方软件包，可以使用参数--system-site-packages。)
> - 启动虚拟环境source jnugeek/bin/activate/
> - 在虚拟环境里面安装模块用pip并且不加sudo
> - 退出虚拟环境deactivate
> - 删除虚拟环境rm -r jnugeek
> 
> 虚拟环境作用:为应用营造一个相对独立的环境，可以在同一台主机上运行多个不同版本的python程序而不会因为模块之类的相互干扰。
> 
> #### uwsgi
> - 安装pip install uwsgi
> - 配置好config.ini
> ```
> [uwsgi]
> # uwsgi 启动时所使用的地址与端口
> socket = 127.0.0.1:5000
> 
> # 指向网站目录
> chdir = /home/uftp/blog/
> 
> # python 启动程序文件
> wsgi-file = blogapp.py
> 
> # python 程序内用以启动的 application 变量名
> callable = app
> 
> # 进程数
> processes = 4
> 
> # 线程数
> threads = 2
> 
> #状态检测地址
> stats = 127.0.0.1:9191
> ```
> - 运行应用uwsgi config.ini
>  
> uwsgi是用来跑python应用的，比起直接运行，其优势在于指定了进程和线程数，而直接运行遇到高访问量的时候处理可能就没那么快了。然而这样极其容易出问题，端口可能被占用，callable可能找不到，而我也是因为这些才放弃使用uwsgi。
> 
> #### Nginx
> - 安装sudo apt-get install nginx
> - 配置，在/etc/nginx/sites-available目录中添加配置文件，建议直接改目录中的default，因为如果两个文件都建立了软链接，那么很可能引起端口冲突，事实上我遇到有一个问题就是因为这个
> 
> ```
> server {
>     listen      80; # 监听80端口
> 
>     root       /srv/awesome/www;
>     access_log /srv/awesome/log/access_log;
>     error_log  /srv/awesome/log/error_log;
> 
>     server_name awesome.liaoxuefeng.com; # 配置域名
> 
>     # 处理静态文件/favicon.ico:
>     location /favicon.ico {
>         root /srv/awesome/www;
>     }
> 
>     # 处理静态资源:
>     location ~ ^\/static\/.*$ {
>         root /srv/awesome/www;
>     }
> 
>     # 动态请求转发到9000端口:
>     location / {
>         proxy_pass       http://127.0.0.1:9000;
>         proxy_set_header X-Real-IP $remote_addr;
>         proxy_set_header Host $host;
>         proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
>     }
> }
> ```
> - 重启nginx　sudo /etc/init.d/nginx reload(或者sudo service nginx restart)
> - 如果遇到了问题
> 1. killall nginx
> 2. sudo service nginx start
> - 如果配置文件无误，那就可以了。
>  
> Nginx它可以处理静态资源，同时作为反向代理把动态请求交给Python代码处理。也就是说它连接起了服务器与网络，并加载了静态文件。它是整个服务器系统中最重要的部分。
> 
> #### Supervisor
> - 安装sudo apt-get install supervisor
> - 编写一个Supervisor的配置文件awesome.conf，存放到/etc/supervisor/conf.d/目录下
> ```
> [program:awesome]
> 
> command     = /srv/awesome/www/app.py
> directory   = /srv/awesome/www
> user        = www-data
> startsecs   = 3
> 
> redirect_stderr         = true
> stdout_logfile_maxbytes = 50MB
> stdout_logfile_backups  = 10
> stdout_logfile          = /srv/awesome/log/app.log
> ```
> - 启动/停止sudo supervisorctl start/stop awesome
> - 重新加载sudo supervisorctl reload
> - 查看运行状态sudo supervisorctl status
>  
> Supervisor+uwsgi是用来更好的运行app的，uwsgi可以单独启动，但二者一起就更方便监控，崩溃的时候也可以自动重启了
> 
### 2016-0X-XX XX:XX:XX重装了Ubuntu 14.04 LTS

这个是在吕方的电脑上装的，所以时间什么的就不具体了。

吸取了上次的教训后，这次装环境就更加谨慎了，也没什么差错，但是virtualenv+uwsgu+Nginx+Supervisor一套下来后还是在uwsgi上卡住了，死活识别不出来callable。无奈之下，又因为看到了廖雪峰的教程，由于对廖雪峰无脑的信任，就决定再次重装系统重新试验了。于是又在我的电脑上进行了新一次的死亡回归——第六次重装系统。

### 2016-09-17 13:34:12重装了Ubuntu 14.04 LTS

最后一次死亡回归了，当真正成功的时候，我的内心在狂喜之中，更多的是不要再出现任何差错的想法。

按照廖雪峰的教程，一步一步走了下来，然而还是加载不出来界面，这次意外的没有报任何错误。但这样反而更令人崩溃。

后来又尝试了uwsgi，还是同样的错误。

之后又改了admin.py中的host为0.0.0.0，改端口，感觉尝试的命令就打了一百多行，然而一直不见效。

第二天志平调试了一番后突然能用了，我简直是欣喜若狂！然而命途多舛，后来我再看的时候网站突然就不能用了，再后来就进入Nginx的欢迎界面了。

最后还是靠陈毅钊，解决了问题，并且找到了问题的症结所在。至此，Flask应用才正式稳定的部署了上去。

> 附：问题分析
> - 按照廖雪峰的教程不成功的原因之一就是因为Nginx的配置文件没读取出来，尽管建立了新的软链接，但是旧的软链接也同时存在，所以就有了sudo nginx中出现的80端口已被占用的错误。另外，用sudo nginx -t来查看配置文件是否有错误。
> - 另一个原因就是因为Flask自身的特殊性，要想被外界访问，需要指定host为0.0.0.0，但事实上和127.0.0.1是相同的，因为二者绑定在了一起。然后就是Flask的端口不必指定为80(Nginx所监听的端口)，而是和下面那个指定的保持一致就可以了。
> - 还有一个原因，也就是最初按照廖雪峰的却没有加载出任何界面的原因就是因为腾讯云主机设置了安全组只开放21端口，也就是说无论是5000端口还是80端口从外界都无法访问，因此也连Nginx的欢迎界面都加载不进去了。
> - 到此，问题全部解决。至于uwsgi，由于我本身对端口之类的了解有限，无法给出一个说的通的解释，但我现在的问题是：uwsgi启动的端口应该和python app相同么？为什么我再设置这二者相同的时候就会出现端口冲突？在admin.py文件中到底要怎样才能让callable能被识别出来？这些就是另外的问题了。或许现在我有些谨慎也就不愿意再冒险去试探这些问题了。先让官网就这样比较稳定的运行吧。后续等用的人少了再进行升级更新。那个时候就要再次请尘封已久的git出山了。这段时间真的要感谢和我一起奋斗官网的胡妙和东麟了，还有积极帮忙的郑洋，志平，陈毅钊了。

异世界在今天迎来了第一季的完结，同时我的Flask第一季也迎来了完结。菜月昴的精神，也应该继续留存着，伴随着我以后继续的开发。

ps:MySQL中还有一个坑，Ubuntu中的MySQL默认是不支持中文的，所以在建数据库的时候要指明用utf8

```
create database jnugeek default character set utf8
```
