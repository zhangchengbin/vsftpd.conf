<div class="lab-edi-doc"><h1 id="laravel-">Laravel 环境搭建</h1>
<h2 id="-laravel">安装 Laravel</h2>
<blockquote>
<p>任务时间：20min ~ 30min</p>
</blockquote>
<h3 id="laravel-">Laravel 简介</h3>
<p><strong><a target="_blank" href="https://laravel.com/" title="null">Laravel</a></strong> 是一套简洁、优雅的 PHP Web 开发框架。它可以让你从面条一样杂乱的代码中解脱出来；它可以帮你构建一个完美的 web APP，而且每行代码都可以简洁、富于表达力。</p>
<h3 id="-">安装依赖</h3>
<p>由于默认的 <strong>yum</strong> 源 php 版本低于 Laravel 要求，所以需要添加第三方源：</p>
<pre><code>rpm -Uvh https://mirror.webtatic.com/yum/el7/epel-release.rpm
rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
</code></pre><p>安装 <strong>nginx</strong>、<strong>php</strong> 及相关依赖：</p>
<pre><code>yum -y install nginx mariadb-server php70w php70w-fpm php70w-mysql php70w-mcrypt php70w-dom php70w-mbstring
</code></pre><h3 id="-mariadb-mysql-">配置 Mariadb（MySQL）</h3>
<p>使用以下命令启动 <strong>mysql</strong> 并设为开启启动：</p>
<pre><code>systemctl start mariadb
systemctl enable mariadb
</code></pre><p>首次启用 <strong>mysql</strong> 时，我们需要执行以下指令进行配置：</p>
<pre><code>mysql_secure_installation
</code></pre><p>过程中除下图中两次 <em>输入及确认密码</em> 外，一路按回车键选择<strong>默认值</strong>即可。</p>
<p><img src="https://share-10039692.file.myqcloud.com/lab/ac197229e6/image/rep5b59cnz/image.png" alt="image"></p>
<h3 id="-composer">安装 Composer</h3>
<p><a target="_blank" href="https://getcomposer.org/" title="null">Composer</a> 是 <strong>php</strong> 的依赖管理工具，我们将使用它下载 Laravel 安装包。</p>
<h4 id="-composer">下载 Composer</h4>
<pre><code>curl -sS https://getcomposer.org/installer | php
mv composer.phar /usr/local/bin/composer
</code></pre><p>设置环境变量，只有这样安装后系统才能找到 laravel 的执行文件：</p>
<pre><code>export PATH=$PATH:/root/.config/composer/vendor/bin
</code></pre><h4 id="-swap">设置 Swap</h4>
<p>为了避免 composer 安装应用过程中出现内存不足的问题，我们预先设置一下 <strong>swap</strong> <sup>[<a href="#stage-1-step-4-BUBBLE_LABEL">?</a>]</sup>：</p>
<pre><code>/bin/dd if=/dev/zero of=/var/swap.1 bs=1M count=1024
/sbin/mkswap /var/swap.1
/sbin/swapon /var/swap.1
</code></pre><p><a id="stage-1-step-4-BUBBLE_LABEL"></a></p>
<blockquote>
<p>Swap 分区在系统的物理内存不够用的时候，把硬盘空间中的一部分空间释放出来，以供当前运行的程序使用。</p>
</blockquote>
<h3 id="laravel-">Laravel 安装</h3>
<p>使用 Composer 安装 Laravel：</p>
<pre><code>composer global require "laravel/installer"
</code></pre><h2 id="-laravel">使用 Laravel</h2>
<blockquote>
<p>任务时间：15min ~ 20min</p>
</blockquote>
<h3 id="-">新建项目</h3>
<p>进入 <strong>/home</strong> 目录，我们创建一个 laravel 项目，命名为 <strong>blog</strong>：</p>
<pre><code>cd /home
laravel new blog
</code></pre><p>点击 <em>/home/blog</em> 可查看项目结构。</p>
<h3 id="-">更新项目依赖</h3>
<p>进入 <strong>blog</strong> 项目，使用 <code>composer update --no-scripts</code> 更新项目依赖：</p>
<pre><code>cd blog
composer update --no-scripts
</code></pre><h3 id="-">配置项目</h3>
<h4 id="-">配置目录权限</h4>
<p>为了运行 Laravel，我们需要为一些项目目录配置权限：</p>
<pre><code>sudo chmod 775 /home/blog/storage
sudo chmod 775 /home/blog/bootstrap/cache
</code></pre><h4 id="-">生成密钥</h4>
<p>查看 blog 目录下是否包含 <strong>.env</strong> 文件，如果不存在，则右击 <em>.env.example</em> 文件，将其重命名为 <strong>.env</strong>。</p>
<p>我们使用以下命令来生成一串密钥：</p>
<pre><code>php artisan key:generate
</code></pre><p>执行后会得到如下输出：</p>
<p><code>Application key [...] set successfully.</code></p>
<p>点击打开 <em>/config/app.php</em>，找到如下一行：</p>
<p><code>'key' =&gt; env('APP_KEY'),</code></p>
<p>将生成的<strong>密钥</strong>填入（中括号中部分）：</p>
<p><code>'key' =&gt; env('APP_KEY', '...'),</code></p>
<p>最后点击 <strong>Ctrl + S</strong> 保存。</p>
<h3 id="-">测试启动</h3>
<p>在 <strong>blog</strong> 目录下，我们使用下面命令来启用 <em>开发服务器</em> 测试访问：</p>
<pre><code>sudo php artisan serve --host=0.0.0.0 --port=80
</code></pre><p>接着，我们可以打开 http://&lt;您的 CVM IP 地址&gt; 测试访问。</p>
<h2 id="-nginx">配置 nginx</h2>
<blockquote>
<p>任务时间：15min ~ 20min</p>
</blockquote>
<p>上面的 <strong>serve</strong> Artisan 命令一般只用于本地开发，而生产环境中我们需要使用 Web 服务器，这里我们选用了 <strong>nginx</strong>。</p>
<h3 id="-php-fpm">启动 php-fpm</h3>
<p>首先我们先按 <code>Ctrl + C</code> 停止掉刚刚的 <strong>serve</strong>。</p>
<p>在 nginx 中，我们通过 <strong>php-fpm</strong> 来调用 php，通过下面命令启动 <strong>php-fpm</strong>：</p>
<pre><code>systemctl start php-fpm
systemctl enable php-fpm
</code></pre><p>可以使用下面的命令查看 <strong>php-fpm</strong> 是否启动 <sup>[<a href="#stage-3-step-1-port">?</a>]</sup>：</p>
<pre><code>netstat -nlpt | grep php-fpm
</code></pre><p><a id="stage-3-step-1-port"></a></p>
<blockquote>
<p>php-fpm 默认监听 9000 端口</p>
</blockquote>
<h3 id="-">编辑配置</h3>
<p>打开 <em>/etc/nginx/nginx.conf</em>，<strong>备注或移除</strong>如下内容：</p>
<pre><code>server {
    listen       80 default_server;
    listen       [::]:80 default_server;
    server_name  _;
    root         /usr/share/nginx/html;

    ...

    error_page 500 502 503 504 /50x.html;
        location = /50x.html {
    }
}
</code></pre><p>在 <code>/etc/nginx/conf.d</code> 目录下 <em>创建 php.conf</em>，然后在该文件中添加如下内容：</p>
<pre><code>server {
    listen      80 default_server;
    listen      [::]:80 default_server;
    server_name _;
    root        /home/blog/public;
    index       index.php index.html index.htm;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ .php$ {
        fastcgi_pass   127.0.0.1:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
        include        fastcgi_params;
    }
}
</code></pre><p>启动 Nginx：</p>
<pre><code>systemctl start nginx
systemctl enable nginx
</code></pre><p>我们可以打开 http://&lt;您的 CVM IP 地址&gt; 测试访问。</p>
<h3 id="-">完成实验</h3>
<p>恭喜！您已经完成了 <strong>Laravel 环境搭建</strong> 的全部实验内容，有关 <strong>Laravel</strong> 的更多资料请查看其<a target="_blank" href="https://laravel.com/docs/5.4" title="null">文档</a>（<a target="_blank" href="https://docs.golaravel.com/docs/5.4/installation/" title="null">中文</a>）。</p>
</div>