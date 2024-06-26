



远程管控内网设备和服务在某些场景下很有必要。

以前用过FRP, 但其服务端需要建在服务器上，自己买的VPS服务器停用了。另一个是用一些提供该类服务的网站比如花生壳或Zerotier等等。

最近看到了Cloudflare tunnel的介绍，可以免费永久的搭建通道来管控内网（家里）设备和服务，还是比较有吸引力的。

搭建该Tunnel需要Cloudflare的账号，这个可以免费注册。  
还需要一个域名，这个有免费的，比如[Cloudns](https://www.cloudns.net/). 也有收费的，收费有高有低，低的一年也就不到10元人民币,如[Namecheap](https://www.namecheap.com),[Namesoli](https://www.namesilo.com/), [Spaceship](https://www.spaceship.com/)等网站中的某些域名。

搭建的方法网上很多就不多说了，按教程很快就能搭建好，也能顺利的实现穿透内网的目的。
顺便也折腾了其它功能，包括远程管理内网软路由器上的下载功能，Transmission和Qbt到相应的页面进去就可以正常使用，但Aria2就不行，一直连不上成功不了。  
Aria2支持的协议多，不想放弃。  
在网上包括google找了教程，但很难找到，唯一的一个讨论的帖子是在[reddit](https://www.reddit.com/r/selfhosted/comments/10bmp74/is_it_possible_to_access_the_ariang_web_ui_using/)上，一年前的帖子，而且比较模糊。
结合这个帖子，自己摸索了几天，终于搞通了。
步骤见下，里面几个坑要注意，
>  1. 如果在tunnel上直接分配一个子域名指向内网的aria2服务，是连不上的。如下图所示，仅有第一个子域名指向内网Aria2所在的设备的IP及Aria2的端口6800，外网访问该子域名，不能连接。必须要有另外一个子域名指向该设备IP，见下图第二个。这两个结合使用才可以连，这两个要同时设定。
![alt text](/imgs/Cloudflare-Tunnel远程管理内网的Aria2/cf1_2024-06-10_00-30-55.png)



>  2. 外网浏览器输入上图中的第二个子域名，进入内网设备管理后台页面，找到Aria2并进入，点开ARIANG, 即可进入Aria2控制台设置页面。
> ![alt text](/imgs/Cloudflare-Tunnel远程管理内网的Aria2/cf2_2024-06-10_00-48-15.png)


>  3. 此时始终显示未连接，如下图。不要急，能进入Aria控制台，就已经成功了90%。下图中的3和4是我们要改的参数。
> 注意此处的RPC协议已经是Https了，不是CF Tunnel里设置的Http, 因为CF自动给它们增加了证书变成了Https。
> ![alt text](/imgs/Cloudflare-Tunnel远程管理内网的Aria2/cf3_2024-06-10_01-19-25.png)



>  4. 改动如下图，aria RPC地址改为第一步中的第一个子域名，端口改为443，同时检查端口后的值是否是jsonrpc, 不是的话改回来。改完后点击重新加载Ariang.
> ![alt text](/imgs/Cloudflare-Tunnel远程管理内网的Aria2/cf4_2024-06-10_01-20-50.png)


>     5.控制台刷新后就显示连接成功了。大工告成！
>  ![alt text](/imgs/Cloudflare-Tunnel远程管理内网的Aria2/cf5_2024-06-10_01-22-50.png)


综上，先要能访问内网设备，通过内网设备进入Aria2控制台，才能连上Aria2服务来控制管理。
做此纪录，以备忘。
