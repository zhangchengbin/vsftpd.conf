<div class="lab-edi-doc"><h1 id="-pyspider-">搭建 PySpider 爬虫服务</h1>
<h2 id="-">环境准备</h2>
<blockquote>
<p>任务时间：5min ~ 10min</p>
</blockquote>
<h3 id="-">前置环境部署</h3>
<p>在开始部署前，我们需要做一些前置准备 <sup>[<a href="#stage-1-step-1-icon_prepare">?</a>]</sup>。</p>
<p>yum 更新 <sup>[<a href="#stage-1-step-1-icon_please_wait">?</a>]</sup></p>
<pre><code>yum update -y
</code></pre><p>安装开发编译工具</p>
<pre><code>yum install gcc gcc-c++ -y
</code></pre><p>安装依赖库</p>
<pre><code>yum install python-pip python-devel python-distribute libxml2 libxml2-devel python-lxml libxslt libxslt-devel openssl openssl-devel -y
</code></pre><p>升级pip</p>
<pre><code>pip install --upgrade pip
</code></pre><p><a id="stage-1-step-1-icon_prepare"></a></p>
<blockquote>
<p>该步骤可选，但为了部署的稳定性，推荐执行</p>
</blockquote>
<p><a id="stage-1-step-1-icon_please_wait"></a></p>
<blockquote>
<p>该步骤耗时可能较长（5~10min），请耐心等待</p>
</blockquote>
<h2 id="-mariadb">部署 mariadb</h2>
<blockquote>
<p>任务时间：10min ~ 20min</p>
</blockquote>
<p>由于 CentOS 7 中 MySQL 数据库已从默认的程序列表中移除，我们使用 mariadb 代替。</p>
<h3 id="-mariadb">安装 mariadb</h3>
<pre><code>yum install mariadb-server mariadb -y
</code></pre><h3 id="-mariadb-">启动 mariadb 服务</h3>
<pre><code>systemctl start mariadb
</code></pre><h3 id="-root-">设置 root 密码</h3>
<p>默认的root用户密码为空，你可以使用以下命令来创建 root 用户的密码：</p>
<p><em>（该步骤也可以跳过，password 后的 <code>Password</code> 可以改为任何你希望设置的密码）</em></p>
<pre><code>mysqladmin -u root password "Password"
</code></pre><h3 id="-">检查是否安装成功</h3>
<p>现在你可以尝试通过以下命令来连接到 Mysql 服务器 <sup>[<a href="#stage-2-step-4-icon_for_none_pass">?</a>]</sup></p>
<pre><code>mysql -u root -p
</code></pre><p>然后输入您刚才设置的密码 （ 默认：<code>Password</code> ），如果一切正常，您应该可以在命令行看到以 <code>MariaDB [(none)]&gt;</code> 或 <code>mysql&gt;</code> 开头的提示了，说明连接成功。</p>
<p>此时输入 <code>SHOW DATABASES;</code> 并回车，应该可以看到类似下面这样的输出，说明一切正常。</p>
<pre><code>mysql&gt; SHOW DATABASES;
+----------+
| Database |
+----------+
| mysql    |
| test     |
+----------+
2 rows in set (0.13 sec)
</code></pre><p>完成后，可以通过快捷键 <code>Ctrl+C</code> 或命令行键入 <code>exit</code> 来退出，进入下一步。</p>
<p><a id="stage-2-step-4-icon_for_none_pass"></a></p>
<blockquote>
<p>如果您未设置密码，直接使用 <code>mysql</code> 即可 </p>
</blockquote>
<h2 id="-redis">部署 redis</h2>
<blockquote>
<p>任务时间：10min ~ 20min</p>
</blockquote>
<h3 id="-">下载、解压安装包</h3>
<h4 id="-">下载安装包</h4>
<pre><code>wget http://download.redis.io/redis-stable.tar.gz
</code></pre><h4 id="-">解压安装包</h4>
<pre><code>tar -xzvf redis-stable.tar.gz
</code></pre><h4 id="-usr-local-">移动解压包到 /usr/local 内</h4>
<pre><code>mv redis-stable /usr/local/redis
</code></pre><h4 id="-">编译安装</h4>
<pre><code>cd /usr/local/redis
make
make install
</code></pre><h3 id="-redis-">设置 redis 配置</h3>
<h4 id="-">设置配置文件路径</h4>
<pre><code>mkdir -p /etc/redis
cp /usr/local/redis/redis.conf /etc/redis/redis.conf
</code></pre><p>修改 <em>/etc/redis/redis.conf </em> 文件的 <code>daemonize</code> 配置项为如下：</p>
<pre><code>daemonize yes
</code></pre><h3 id="-redis-">启动 redis 服务</h3>
<pre><code>/usr/local/bin/redis-server /etc/redis/redis.conf
</code></pre><h2 id="-pyspider">部署 pyspider</h2>
<blockquote>
<p>任务时间：10min ~ 20min</p>
</blockquote>
<h3 id="-">安装依赖</h3>
<pre><code>pip install --upgrade chardet
easy_install mysql-connector==2.1.3
easy_install redis
</code></pre><h3 id="-pyspider">安装 pyspider</h3>
<pre><code>pip install pyspider
</code></pre><h3 id="-pyspider">配置 pyspider</h3>
<p>首先创建配置目录</p>
<pre><code>mkdir /etc/pyspider
</code></pre><p>然后 <code>/etc/pyspider</code> 目录下<em>创建 pyspider.conf.json</em>，参考下面的内容。</p>
<p>具体配置的说明文档请参考 <a target="_blank" href="http://docs.pyspider.org/en/latest/Deployment/#configjson" title="null">官方文档</a></p>
<h5 id="-etc-pyspider-pyspider-conf-json">示例代码：/etc/pyspider/pyspider.conf.json</h5>
<pre><code class="lang-javascript">{
  "taskdb": "mysql+taskdb://root:Password@127.0.0.1:3306/taskdb",
  "projectdb": "mysql+projectdb://root:Password@127.0.0.1:3306/projectdb",
  "resultdb": "mysql+resultdb://root:Password@127.0.0.1:3306/resultdb",
  "message_queue": "redis://127.0.0.1:6379/db",
  "webui": {
    "username": "root",
    "password": "Password",
    "need-auth": true
  }
}
</code></pre>
<p>其中 mysql 配置中的 <code>root</code> 为您 mysql 的用户名， <code>root:</code> 后面的 <code>Password</code> 为您刚设置的密码。</p>
<p><code>webui</code> 配置中的 username 及 password 为您访问 WebUI 时候需要的用户名，你也可以不设置用户名密码，直接将 <code>need-auth</code> 设为 <code>false</code> 即可。</p>
<h3 id="-">启动服务</h3>
<pre><code>pyspider -c /etc/pyspider/pyspider.conf.json
</code></pre><p>如果一切正常，现在访问 <a target="_blank" href="http://&lt;您的 CVM IP 地址&gt;:5000" title="null">http://&lt;您的 CVM IP 地址&gt;:5000</a>，您应该可以看到 pyspider dashboard 的首页了。</p>
<p>服务能够正常启动后，我们需要让它能够在后台运行，您可以通过以下命令让服务在后台运行</p>
<pre><code>nohup pyspider -c /etc/pyspider/pyspider.conf.json &amp;
</code></pre><p>也可以使用官方推荐的 <a target="_blank" href="http://supervisord.org/" title="null">Supervisor</a> 来启动，这里就不详细介绍了，具体用法可以参考 Supervisor 的文档</p>
<h2 id="-">部署完成</h2>
<blockquote>
<p>任务时间：1min ~ 2min</p>
</blockquote>
<h3 id="-">访问服务</h3>
<p>此时您可以访问 <a target="_blank" href="http://&lt;您的 CVM IP 地址&gt;:5000" title="null">http://&lt;您的 CVM IP 地址&gt;:5000</a> 使用您的爬虫来搜集数据了，具体 pyspider 爬虫脚本的编写及使用教程可以参考 <em>网上资料</em>。</p>
<h3 id="-">大功告成</h3>
<p>恭喜您已经完成了搭建 PySpider 爬虫服务的学习，您可以留用或者<em>购买 Linux 版本的 CVM</em> 继续学习。</p>
</div>