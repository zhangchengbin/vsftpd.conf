<div class="lab-edi-doc"><h1 id="-firekylin-">搭建 Firekylin 个人网站</h1>
<h2 id="-">准备域名</h2>
<blockquote>
<p>任务时间：15min ~ 20min</p>
</blockquote>
<h3 id="-">域名注册</h3>
<p>如果您还没有域名，可以<a target="_blank" href="https://dnspod.qcloud.com/?fromSource=lab" title="null">在腾讯云上选购</a>，过程可以参考下面的视频。</p>
<ul>
<li><em>视频 - 在腾讯云上购买域名</em></li>
</ul>
<h3 id="-">域名解析</h3>
<p>域名购买完成后, 需要将域名解析到实验云主机上，实验云主机的 IP 为：</p>
<pre><code>&lt;您的 CVM IP 地址&gt;
</code></pre><p>在腾讯云购买的域名，可以<em>到控制台添加解析记录</em>，过程可参考下面的视频：</p>
<ul>
<li><em>视频 - 如何在腾讯云上解析域名</em> </li>
</ul>
<p>域名设置解析后需要过一段时间才会生效，通过 <code>ping</code> 命令检查域名是否生效 <sup>[<a href="#stage-1-step-2-replace">?</a>]</sup>，如：</p>
<pre><code>ping www.yourdomain.com
</code></pre><p>如果 ping 命令返回的信息中含有你设置的解析的 IP 地址，说明解析成功。</p>
<p><a id="stage-1-step-2-replace"></a></p>
<blockquote>
<p>注意替换下面命令中的 <code>www.yourdomain.com</code> 为您自己的注册的域名</p>
</blockquote>
<h2 id="-">运行环境准备</h2>
<blockquote>
<p>任务时间：10min ~ 20min</p>
</blockquote>
<h3 id="-node-js">安装 Node.js</h3>
<p>使用 yum 命令安装 Node.js</p>
<pre><code>yum install nodejs -y
</code></pre><h3 id="-npm-pm2">使用 NPM 安装 PM2</h3>
<p>通过 NPM 安装进程管理模块 PM2。它是 Node.js 的一个进程管理模块，之后我们会使用它来管理我们的个人网站进程。</p>
<pre><code>npm install pm2 -g
</code></pre><h3 id="-mysql">安装 MySQL</h3>
<p><sup>[<a href="#stage-2-step-3-yum-install-mysql">使用 yum 安装 MySQL</a>]</sup>：</p>
<pre><code>wget http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm
rpm -ivh mysql-community-release-el7-5.noarch.rpm
yum install mysql-community-server -y
</code></pre><p>启动 MySQL 服务：</p>
<pre><code>service mysqld restart
</code></pre><p>设置 MySQL 账户 <sup>[<a href="#stage-2-step-3-mysql-password">root 密码</a>]</sup>：</p>
<pre><code>/usr/bin/mysqladmin -u root password 'Password4Firekylin'
</code></pre><p><a id="stage-2-step-3-yum-install-mysql"></a></p>
<blockquote>
<p>安装过程需要持续大概 15-20min 左右，请耐心等待。</p>
</blockquote>
<p><a id="stage-2-step-3-mysql-password"></a></p>
<blockquote>
<p>下面命令中的密码是教程为您自动生成的，为了方便实验的进行，不建议使用其它密码。如果设置其它密码，请把密码记住，在后续的步骤会使用到。</p>
</blockquote>
<h3 id="-nginx">安装 Nginx</h3>
<p>在 CentOS 上，可直接使用 <sup>[<a href="#stage-2-step-4-aptget">yum</a>]</sup> 来安装 Nginx</p>
<pre><code>yum install nginx -y
</code></pre><p><a id="stage-2-step-4-aptget"></a></p>
<blockquote>
<p>如果是 Debian/Ubuntu 则使用 apt-get 安装即可。</p>
</blockquote>
<h2 id="-firekylin">安装并配置 Firekylin</h2>
<blockquote>
<p>任务时间：5min ~ 10min</p>
</blockquote>
<h3 id="-firekylin">安装 Firekylin</h3>
<p>在服务器上下载安装包</p>
<pre><code>wget https://firekylin.org/release/latest.tar.gz
</code></pre><p>解压安装包</p>
<pre><code>tar zvxf latest.tar.gz
</code></pre><p>安装程序依赖</p>
<pre><code>cd firekylin
npm install
</code></pre><p>复制项目下的 <code>pm2_default.json</code> 文件生成新文件 <code>pm2.json</code></p>
<pre><code>cp pm2_default.json pm2.json
</code></pre><p>修改 <em>pm2.json</em> 文件中的 cwd 配置值为项目的当前路径 <code>/root/firekylin</code>：</p>
<pre><code>{
  "apps": [{
    "name": "firekylin",
    "script": "www/production.js",
    "cwd": "/root/firekylin",
    "exec_mode": "fork",
    "max_memory_restart": "1G",
    "autorestart": true,
    "node_args": [],
    "args": [],
    "env": {

    }
  }]
}
</code></pre><p>然后通过以下命令启动项目</p>
<pre><code>pm2 startOrReload pm2.json
</code></pre><p>Firekylin 已经启动成功，使用浏览器直接访问 http://&lt;您的 CVM IP 地址&gt;:8360/ 即可看到 Firekylin 的配置界面。</p>
<h3 id="-">配置信息</h3>
<p>通过访问 http://&lt;您的 CVM IP 地址&gt;:8360/ 配置信息，配置过程输入参数如截图所示，其中数据库信息中的<code>帐号</code>字段设置为 <code>root</code>，<code>密码</code>字段设置为 <code>Password4Firekylin</code>，<code>数据库名</code>字段设置为 <code>firekylin</code>，<code>主机</code>字段设置为 <code>127.0.0.1</code>，其他字段使用默认值；后台管理帐号中的<code>帐号</code>字段使用默认值 <code>admin</code>，<code>密码</code>字段设置为 <code>Password4Admin</code>：</p>
<p><img src="https://mc.qcloudimg.com/static/img/2b6b8757d891a5c67581f64b0c75cc42/1.png" alt=""></p>
<p>配置完成后可以通过后台管理帐号设置的<code>帐号</code>和<code>密码</code>登录<em>博客管理后台</em>，其值分别为 <code>admin</code> 和 <code>Password4Admin</code>，截图如下所示：</p>
<p><img src="https://mc.qcloudimg.com/static/img/d5b5b0b892c165eb6d80e8a699d22657/1.png" alt=""></p>
<h3 id="-nginx">配置 Nginx</h3>
<p>下面我们就配置 Nginx 使用域名访问我们的网站了。</p>
<p>复制项目下的 nginx_default.conf 为 nginx.conf</p>
<pre><code>cp nginx_default.conf nginx.conf
</code></pre><p>修改 <em>nginx.conf</em> 文件</p>
<pre><code>server {
    listen 80;
    server_name www.yourdomain.com; #将 www.yourdomain.com 替换为之前注册并解析的域名
    root /root/firekylin;
    set $node_port 8360;

    index index.js index.html index.htm;

    location ^~ /.well-known/acme-challenge/ {
      alias /root/firekylin/ssl/challenges/;
      try_files $uri = 404;
    }

    location / {
        proxy_http_version 1.1;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        proxy_set_header X-NginX-Proxy true;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_pass http://127.0.0.1:$node_port$request_uri;
        proxy_redirect off;
    }

    location = /development.js {
        deny all;
    }
    location = /testing.js {
        deny all;
    }

    location = /production.js {
        deny all;
    }
}
</code></pre><p>将 nginx.conf 文件软链到 nginx 配置目录下</p>
<pre><code>ln -s /root/firekylin/nginx.conf /etc/nginx/conf.d/firekylin.conf
</code></pre><p>重启 Nginx</p>
<pre><code>service nginx restart
</code></pre><p><a id="stage-3-step-3-nginx-conf-tip"></a></p>
<blockquote>
<p>server_name 的值为你的域名，root 为你的项目所在路径，$node_port 的值为 Firekylin 启动端口，默认为 8360。</p>
</blockquote>
<h3 id="-">大功告成！</h3>
<p>恭喜，您的 Firekylin 已经部署完成，尽情折腾吧：</p>
<p>博客访问地址：<a target="_blank" href="http://&lt;您的域名&gt;" title="null">http://&lt;您的域名&gt;</a>   </p>
<p>博客后台地址：<a target="_blank" href="http://&lt;您的域名&gt;/admin" title="null">http://&lt;您的域名&gt;/admin</a></p>
</div>