# 腾讯云服务器基于 Dnspod 添加泛域名 SSL

**2018-05-02 文章具有时效性，以下内容仅供参考**。

> Let’s Encrypt官方在2018年上线泛域名免费SSL证书。暂时只能通过DNS方式获取，支持的DNS解析有很多，国内可以通过腾讯云的DNSPod.cn域名API和阿里云域名API自动颁发Let’s Encrypt泛域名免费SSL证书。
>
> 本文记录腾讯云服务器使用 Dnspod 添加泛域名 SSL 证书。

## 一、安装依赖

我的服务器是 Ubuntu，执行下面命令安装：

```
sudo install socat
```

## 二、下载 ACME.SH

> 更多关于 **acme.sh** 的信息，请参考 [GitHub Wiki](https://github.com/Neilpang/acme.sh/wiki/%E8%AF%B4%E6%98%8E)，感谢 [Neilpang](https://github.com/Neilpang) 这位大佬。

```
curl https://get.acme.sh | sh
```

## 三、获取域名 API

比如：我的域名是在万网注册的，后来随着服务器迁移到了腾讯云，在使用DNSPod的NS服务器进行解析以后，就可以直接使用腾讯的 **dnspod.cn** 提供的服务器来创建一个授权 API。

来到 [dnspod.cn](https://www.dnspod.cn) 登录你的账号，按照下图位置开启 API。因为我已经开启了，所以下图中显示状态是已开启状态。

![开启 API](https://ws3.sinaimg.cn/large/006tNc79ly1fqxbx890vjj31kw0plgn5.jpg)

开启成功后会有弹窗，请务必保存好 **ID** 和 **Token**。

![开启 API 成功](https://ws4.sinaimg.cn/large/006tNc79ly1fqxc239vzuj30ps0hy76s.jpg)

## 四、配置服务器

登录你的服务器，执行下列代码，将 **your ID** 和 **your Token** 分别换成你自己的，将 **coderlxc.com** 换成你自己的域名。

```
export DP_Id="your ID"

export DP_Key="your Token"

acme.sh   --issue   --dns dns_dp   -d coderlxc.com  -d *.coderlxc.com
```

脚本结束后，会在窗口里显示安装的结果，如果安装成功的话，会提示你证书等文件的存放位置。

一般情况下，证书会存放在 `~/acme.sh/` 下，但脚本的作者已在 **wiki** 中做出强调：**不要直接使用此目录下的文件，里面的文件而且目录结构可能会变化**。

所以我们还需要执行下面的代码，将生成的证书复制到相应的位置，代码里所以指定的参数都会被脚本自动记录下来，并在将来自动更新以后，被再次自动调用。**注意，我这里的路径是 `/etc/nginx/ssl`，需要提前创建好，你也可以换成你自己想要存放的位置并将 `<domain>` 换成你自己的域名**。

```bash
# 递归创建用于存放证书的目录
sudo mkdir -p /etc/nginx/ssl

# 将目录属主改为我的普通用户 ubuntu，这里需要替换成你自己的用户。
sudo chown ubuntu:ubuntu /etc/nginx -R

acme.sh  --installcert  -d  coderlxc.com   \
        --key-file   /etc/nginx/ssl/coderlxc.key \
        --fullchain-file /etc/nginx/ssl/fullchain.cer \
        --reloadcmd  "service nginx force-reload"
```

然后更改服务器配置：

```
server
{
        # 监听 HTTPS 443 端口
        listen 443 ssl http2;
　　　　　# 强制重写 HTTP 为 HTTPS
        if ($server_port !~ 443){
            rewrite ^(/.*)$ https://$host$1 permanent;
        }
        # SSL 证书及协商相关配置，避免浏览器报错，使用完整证书链
        ssl_certificate     /etc/nginx/ssl/fullchain.cer;
        ssl_certificate_key    /etc/nginx/ssl/coderlxc.key;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4:!DH:!DHE;
        ssl_prefer_server_ciphers on;
        ssl_session_cache shared:SSL:10m;
        ssl_session_timeout 10m;
        # 强制将 server_name 里所有域名都重写为 HTTPS（可选）
        error_page 497  https://$host$request_uri;
}
```

然后就可以访问一下你的域名验证一下了。

![验证域名](https://ws2.sinaimg.cn/large/006tKfTcly1fqxe0nji2oj31bo0o8t9z.jpg)