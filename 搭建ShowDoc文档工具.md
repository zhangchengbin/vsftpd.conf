<div class="lab-edi-doc"><h1 id="-showdoc-">搭建 ShowDoc 文档工具</h1>
<h2 id="-nginx-php-">准备 Nginx + PHP 环境</h2>
<blockquote>
<p>任务时间：10min ~ 20min</p>
</blockquote>
<h3 id="-nginx">安装 Nginx</h3>
<p>使用 <code>yum</code> 安装 Nginx：</p>
<pre><code>yum install nginx
</code></pre><p>修改 <em>/etc/nginx/nginx.conf</em> 文件为如下内容：</p>
<pre><code>user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 2048;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;
    include /etc/nginx/conf.d/*.conf;

    server {
        listen       80;
        server_name  127.0.0.1;
        root         /var/www/html;
        index index.php index.html
        error_page  404              /404.html;
        location = /40x.html {
        }
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
        }
        location ~ .php$ {
            root           /var/www/html;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include        fastcgi_params;
        }
        location ~ /.ht {
            deny  all;
        }
    }
}
</code></pre><p>启动 Nginx 并设置为开机启动：</p>
<pre><code>service nginx start
chkconfig nginx on
</code></pre><h3 id="-php">安装 PHP</h3>
<p>使用 <code>yum</code> 安装 php-fpm：</p>
<pre><code>yum install php php-gd php-fpm php-mcrypt php-mbstring php-mysql php-pdo
</code></pre><p>启动 php-fpm 并设置为开机启动：</p>
<pre><code>service php-fpm start
chkconfig php-fpm on
</code></pre><h2 id="-">创建项目</h2>
<blockquote>
<p>任务时间：10min ~ 20min</p>
</blockquote>
<h3 id="-composer">下载安装 Composer</h3>
<p>Composer 是 PHP 的一个依赖管理工具，推荐使用 Composer 创建 ShowDoc 项目。</p>
<p>执行如下命令<sup>[<a href="#stage-2-step-1-install-composer">安装 Composer</a>]</sup>：</p>
<pre><code>curl -sS https://getcomposer.org/installer | php
mv composer.phar /usr/local/bin/composer
</code></pre><p><a id="stage-2-step-1-install-composer"></a></p>
<blockquote>
<p>安装过程可能需要耗费几分钟</p>
</blockquote>
<h3 id="-composer-">设置 Composer 使用国内镜像</h3>
<p>执行命令<sup>[<a href="#stage-2-step-2-mainland-composer">设置 Composer 使用国内镜像</a>]</sup>：</p>
<pre><code>composer config -g repo.packagist composer https://packagist.phpcomposer.com
</code></pre><p><a id="stage-2-step-2-mainland-composer"></a></p>
<blockquote>
<p>为了避免访问国外网络导致的延迟，推荐使用国内镜像源</p>
</blockquote>
<h3 id="-composer-">使用 Composer 创建项目</h3>
<p>执行命令创建项目：</p>
<pre><code>cd /var/www/html/ &amp;&amp; composer create-project  showdoc/showdoc
</code></pre><h3 id="-showdoc-">设置 showdoc 目录写权限</h3>
<p>执行命令赋予 showdoc 下部分目录的写权限</p>
<pre><code>chmod a+w showdoc/install
chmod a+w showdoc/Sqlite
chmod a+w showdoc/Sqlite/showdoc.db.php
chmod a+w showdoc/Public/Uploads/
chmod a+w showdoc/Application/Runtime
chmod a+w showdoc/server/Application/Runtime
chmod a+w showdoc/Application/Common/Conf/config.php
chmod a+w showdoc/Application/Home/Conf/config.php
</code></pre><p>创建完毕，您现在可以通过浏览器访问 http://&lt;您的 CVM IP 地址&gt;/showdoc/install/ ，进行语言的选择以后即可通过 http://&lt;您的 CVM IP 地址&gt;/showdoc 查看站点效果。</p>
<h2 id="-">准备域名和解析</h2>
<blockquote>
<p>任务时间：15min ~ 30min</p>
</blockquote>
<h3 id="-">域名注册</h3>
<p>注：如果您不需要通过域名访问您的站点，请通过<code>已完成，下一步</code>跳过<code>域名注册</code>环节</p>
<p>如果您需要使用域名，可以<a target="_blank" href="https://dnspod.qcloud.com/?fromSource=lab" title="null">在腾讯云上选购</a>，过程可以参考下面的视频。</p>
<ul>
<li><em>视频 - 在腾讯云上购买域名</em></li>
</ul>
<h3 id="-">域名解析</h3>
<p>注：如果您不需要通过域名访问您的站点，请通过<code>已完成，下一步</code>跳过<code>域名解析</code>环节</p>
<p>域名购买完成后, 需要将域名解析到实验云主机上，实验云主机的 IP 为：</p>
<pre><code>&lt;您的 CVM IP 地址&gt;
</code></pre><p>在腾讯云购买的域名，可以<em>到控制台添加解析记录</em>，过程可参考下面的视频：</p>
<ul>
<li><em>视频 - 如何在腾讯云上解析域名</em> </li>
</ul>
<p>域名设置解析后需要过一段时间才会生效，通过 <code>ping</code> 命令检查域名是否生效 <sup>[<a href="#stage-3-step-2-replace">?</a>]</sup>，如：</p>
<pre><code>ping www.yourdomain.com
</code></pre><p>如果 ping 命令返回的信息中含有你设置的解析的 IP 地址，说明解析成功。</p>
<p><a id="stage-3-step-2-replace"></a></p>
<blockquote>
<p>注意替换下面命令中的 <code>www.yourmpdomain.com</code> 为您自己的注册的域名</p>
</blockquote>
<h3 id="-">大功告成！</h3>
<p>恭喜，您的 ShowDoc 站点已经部署完成，您可以通过浏览器访问查看效果。</p>
<p>通过IP地址查看：<a target="_blank" href="http://&lt;您的 CVM IP 地址&gt;/showdoc" title="null">http://&lt;您的 CVM IP 地址&gt;/showdoc</a></p>
<p>通过域名查看：<a target="_blank" href="http://www.yourdomain.com/showdoc" title="null">http://www.yourdomain.com/showdoc</a>，其中替换 <code>www.yourdomain.com</code> 为之前申请的域名。</p>
</div>