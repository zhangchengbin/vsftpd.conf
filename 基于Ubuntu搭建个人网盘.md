<div class="lab-edi-doc"><h1 id="-seafile-">搭建 Seafile 专属网盘</h1>
<h2 id="-">准备域名</h2>
<blockquote>
<p>任务时间：15min ~ 20min</p>
</blockquote>
<h3 id="-">域名注册</h3>
<p>如果您还没有域名，可以<a target="_blank" href="https://dnspod.qcloud.com/?fromSource=lab" title="null">在腾讯云上选购</a>，过程可以参考下面的视频：</p>
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
<h2 id="-seafile-">安装 Seafile 服务器</h2>
<blockquote>
<p>任务时间：15min ~ 20min</p>
</blockquote>
<h3 id="-">安装依赖环境</h3>
<p>在 Debian/Ubuntu 系统下，可以使用以下命令安装 MySQL：</p>
<pre><code>sudo apt-get update
sudo apt-get install mysql-server
</code></pre><p>使用以下命令安装 Python 相关依赖：</p>
<pre><code>sudo apt-get install python2.7 python-setuptools python-imaging python-ldap python-mysqldb python-memcache python-urllib3
</code></pre><p>安装 MySQL 过程需要为 MySQL 的 root 用户设置新密码，请记住该<strong>[密码]</strong>以供后面步骤使用。</p>
<h3 id="-seafile-">为 Seafile 创建一个用户</h3>
<p>创建 Seafile 用户，使用它运行 Seafile 服务：</p>
<pre><code>sudo useradd -m -s /bin/bash seafile
</code></pre><p>为该用户设置密码：</p>
<pre><code>sudo passwd seafile
</code></pre><h3 id="-seafile">下载Seafile</h3>
<p>切换到新用户，需要输入你刚才为seafile用户设置的密码：</p>
<pre><code>su - seafile
</code></pre><p>切换目录：</p>
<pre><code>cd ~
</code></pre><p><em>这里</em>可以查看获取最新 Seafile 下载链接，参考以下命令进行下载。</p>
<pre><code>wget http://seafile-downloads.oss-cn-shanghai.aliyuncs.com/seafile-server_6.1.1_i386.tar.gz
</code></pre><p>解压:</p>
<pre><code>tar -xzf seafile-server_*
mv seafile-server-*/ seafile-server/
</code></pre><h3 id="-seafile">配置 Seafile</h3>
<p>运行Seafile设置脚本，并回答预设问题：</p>
<pre><code>cd seafile-server*
./setup-seafile-mysql.sh
</code></pre><p>执行过程输入参数如下图：</p>
<p>其中：</p>
<ul>
<li><code>[ This server's ip or domain ]</code> 字段输入教程第一步申请的域名或者IP地址（&lt;您的 CVM IP 地址&gt;）。</li>
<li>mysql 的 <code>[ root password ]</code> 字段输入数据库密码。</li>
<li>其他字段一路回车使用默认值。</li>
</ul>
<p><img src="https://mc.qcloudimg.com/static/img/de2fb7e13f26794671aec59bcd0c9165/1.png" alt=""></p>
<h3 id="-seafile">启动 Seafile</h3>
<pre><code>./seafile.sh start
./seahub.sh start
</code></pre><p>执行过程输入参数如截图所示，其中 <code>[ admin email ]</code> 设置为您登录网盘的帐号，如 <code>admin@qcloudlab.wang</code>。</p>
<p><code>[ admin password ]</code> 和 <code>[ admin password again ]</code> 设置为登录网盘的密码，如 <code>admin_Password</code>：</p>
<p><img src="https://mc.qcloudimg.com/static/img/d243d5763695b4d926e9d7fdd0e0e924/1.png" alt=""></p>
<h3 id="-">大功告成！</h3>
<p>恭喜，您的 Seafile 已经部署完成，您现在拥有专属的网盘了，登录的帐号密码为您启动 Seafile 步骤中设置的邮箱和密码。</p>
<ul>
<li>可以通过 IP 访问网盘：<a target="_blank" href="http://&lt;您的域名&gt;:8000" title="null">http://&lt;您的域名&gt;:8000</a></li>
<li>可以通过域名访问网盘：如 <a target="_blank" href="http://www.yourdomain.com:8000" title="null">http://www.yourdomain.com:8000</a> ，其中 <code>www.yourdomain.com</code> 替换为您注册的域名</li>
</ul>
</div>