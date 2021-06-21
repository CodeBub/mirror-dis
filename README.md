# mirror-dis
镜像 for discord 



### 测试 demo 地址 
https://dis.plazz.fun/login 

此地址仅供测试实验，请勿在正式环境使用，否则后果自负


## 配置流程 

### 1.1 购买服务器, 配置环境
*建议，带宽建议5M以上的Hong Kong 服务器*

#### 1.1.1 安装bt工具
wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh
#### 1.1.2 在 bt 中安装 Nginx
略
#### 1.1.3 在 bt 中新建网站
a. 新建网站，示例中新建站点域名 dis.plazz.fun 。 （无数据库，静态站点即可）
b. 为网站配置ssl加密
c. 进入 `配置文件` 部分，删除所有 location 信息，并填入下面自定义location 内容
```
    location / { 
    
      proxy_pass https://discord.com:443;
      proxy_set_header host discord.com;
      proxy_set_header accept-encoding '';
      proxy_set_header referer 'https://discord.com/login';
      proxy_set_header origin 'https://discord.com';
      
        sub_filter_once off;
        sub_filter discord.com dis.plazz.fun;
        sub_filter_types text/html text/css application/javascript;
    }
    
    
    location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|pdf)$
    {
    
      proxy_pass https://discord.com:443;
      proxy_set_header host discord.com;
        expires      30d;
        error_log /dev/null;
        access_log /dev/null;
    }
    
    location ~ .*\.(js|css)?$
    {
      
      proxy_pass https://discord.com:443;
      proxy_set_header host discord.com;
      proxy_set_header accept-encoding '';
        expires      12h;
        error_log /dev/null;
        access_log /dev/null; 
        
        sub_filter_once off;
        sub_filter discord.com dis.plazz.fun;
        sub_filter_types text/html text/css application/javascript;
    }
```
配置完成后重启nginx，完工

### 1.2 测试 
![demo](https://gw.alipayobjects.com/zos/UserAdmin/8yjf4x/abcd111.jpg)
