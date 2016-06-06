# IRC 聊天工具

> IRC (Intelnet Relay Chat) 因特网中继聊天，是一种通过网络即时聊天方式。  
> 主要是群体聊天，也可以个人对个人聊天。

#### irssi

> 用命令行聊天（太Geeks 了）。  
> 安装方式：  
> Debian:       sudo apt-get install irssi  
> Redhat:       sudo yum install irssi  
> arch linux:   sudo pacman -S irssi  
> gentoo:       sudo emerge irssi

#### 使用教程

- 连接服务器

> 运行irssi  
> 连接服务器 irc.freenode.net or ircnet 在线服务

<pre>
	$ irssi
		/connect irc.freenode.net
</pre>

- 注册一个名字

<pre>
	/nick Matcai
	/msg nickserv help        #会打开一个频道，Ctrl+N/P 切换看一下命令，参数错误会有提示
</pre>

#### 基本的用户接口用法

> Home 键向上滚动，End 键向下滚动  
> /SB HOME  和 /SB END 用于滚动屏幕  
> `Meta-1, Meta-2, .. Meta-10`    用于切换1-10 个windows  
> `Meta-q, .. Meta-o`             用于切换11-19 个windows  
> `/WINDOW <number>`              通过命令切换windows  
> `Ctrl-P, Ctrl-N`                上一个windows 和 下一个windows  

#### 自动化网络和服务器连接

> /NETWORK ADD -autosendcmd '^msg hello world' IRCnet  
> /NETWORK ADD -autosendcmd '/^msg nickserv ident pass;wait 2000' OFTC  

> /SERVER ADD -auto -network IRCnet irc.kpnqwest.fi 6667  
> 自动添加服务器  
> /SERVER ADD -auto -network worknet irc.mycompany.com 6667 password

> `-auto` 选项表示在服务器启动时自动连接到该服务器上。

> /CHANNEL ADD -auto -bots \*!\*bot@host.org -botcmd "/^msg $0 op pass" #irssi efnet  
> 自动添加频道进入  
> `-bots` 和 `-botcmd` 表示进入频道自动发送命令给机器人，$0 表示第一个机器人。

#### `/lastlog` 回溯历史消息

> /lastlog world     打印所有包含world 的行  
> /lastlog world 10  打印最后10个world  
> /lastlog -topics   打印所有改变的topics  
> /lastlog -file ~/irc.log  保存

#### 记录消息

> 当设置了`/AWAY` 离开模式，irssi 可以自动记录消息  
> 当取消了`/AWAY` 后，也就是UNAWAY ,日志可以自动打印到屏幕上  

> /SET awaylog_level MSGS HILIGHT   指定要记录那些消息  
> /SET awaylog_file ~/.irssi/away.log 指定存放消息的文件  
> /SET autolog ON 开启自动记录消息

> /SET autolog_level ALL -CRAP -CLIENTCRAP -CTCPS 默认记录的内容  
> /SET autolog_path ~/irclogs/$tag/$0.log 默认记录的日志文件  
> $0 为channel/nick


#### 按键绑定

> /BIND F1 /ECHO F1 pressed  绑定F1

#### 配置代理连接

> /SET use_proxy ON 开启用户代理  
> /SET proxy_address 127.0.0.1 配置代理ip地址  
> /SET proxy_port 1080         配置代理端口  
> /SET proxy_password PROXY_PASSWORD  配置密码 

> BNC 代理的启用  
> /SET -clear proxy_string   
> /SET proxy_string_after conn %s %d

> HTTP 代理的启用  
> /SET -clear PROXY_PASSWORD  
> /EVAL SET proxy_string CONNECT %s；%d HTTP/1.0\n\n

> SOCKS 代理的启用  
> 添加选项--with-socks 在 /SET 配置。


#### 一些配置

> /SET timestamps ON  
> /示每条消息的时间戳  
> /SET hide_text_style OFF  
> /藏所有加粗，下划线，MIRC颜色等。  
> /SET show_nickmode ON  
> /示nick的状态模式，<@nick> 表示管理人，<+nick> 表示voices等  
> /SET show_nickmode_empty ON  
> /果nick没有模式，则使用<nick>  
> /set lag_min_show 100  
> /状态栏显示服务器的延迟，单位是1/100 second  
> /SET indent 10  
> /本过长则换行显示  

### 保存配置 `/save`


### irssi 命令参考

> /ban，/B              #设置或禁止列出channel  
> /clear，/C            清除channel 缓存  
> /join，/J             加入channel  
> /kick，/K             踢用户  
> /kickban，/kb         kickban用户  
> /msg，/M              私聊用户  
> /names，/N            列出用户当前所有加入的channel  
> /query，/q            打开与用户相关的查询窗口，或者关闭  
> /topic，/t            显示或编辑当前话题。  
> /windows close，/wc   关闭窗口  
> /whois，/wi           显示用户信息



#### irc 用户注册以及申请频道

> /nick ArchWave  先设置名字  
> /msg NickServ REGISTER <password> <email>  
> 查看邮箱，根据邮箱信息操作

> 申请频道  
> /join test-cn 如果该频道op 是你，说明没有被注册  
> /msg ChanServ REGISTGER <#channel> <password>  

> 一般频道都通过ChanServ 机器人管理，将机器人永久加入聊天室  
> /msg ChanServ SET <#channel> GUARD ON
