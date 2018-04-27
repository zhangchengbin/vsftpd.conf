<div class="lab-edi-doc"><h1 id="-ftp-">搭建 FTP 文件服务</h1>
<h2 id="-ftp-">安装并启动 FTP 服务</h2>
<blockquote>
<p>任务时间：5min ~ 10min</p>
</blockquote>
<h3 id="-vsftpd">安装 VSFTPD</h3>
<p>使用 <code>apt-get</code> 安装 <a target="_blank" href="vsftpd" title="null">vsftpd</a>：</p>
<pre><code>sudo apt-get install vsftpd -y
</code></pre><p><a id="stage-1-step-1-vsftpd"></a></p>
<blockquote>
<p><code>vsftpd</code> 是在 Linux 上被广泛使用的 FTP 服务器，根据其[官网介绍][<a target="_blank" href="https://security.appspot.com/vsftpd.html]，它可能是" title="null">https://security.appspot.com/vsftpd.html]，它可能是</a> UNIX-like 系统下最安全和快速的 FTP 服务器软件。</p>
</blockquote>
<h3 id="-vsftpd">启动 VSFTPD</h3>
<p>安装完成后 VSFTPD 会自动启动，通过 <code>netstat</code> 命令可以看到系统已经<sup>[<a href="#stage-1-step-2-21">监听了 21 端口</a>]</sup>：</p>
<pre><code>sudo netstat -nltp | grep 21
</code></pre><p>如果没有启动，可以手动开启 VSFTPD 服务：</p>
<pre><code>sudo systemctl start vsftpd.service
</code></pre><p><a id="stage-1-step-2-21"></a></p>
<blockquote>
<p>FTP 协议默认使用 21 端口作为服务端口</p>
</blockquote>
<h2 id="-">配置用户访问目录</h2>
<blockquote>
<p>任务时间：5min ~ 10min</p>
</blockquote>
<h3 id="-">新建用户主目录</h3>
<pre><code>sudo mkdir /home/uftp
</code></pre><p>执行完后，在这里 <a target="_blank" href="/home/uftp" title="null">/home/uftp</a>  <sup>[<a href="#stage-2-step-1-ftp-home">?</a>]</sup> 就能看到新建的文件夹 uftp 了。</p>
<p>创建登录欢迎文件 <sup>[<a href="#stage-2-step-1-welcome">?</a>]</sup>：</p>
<pre><code>sudo touch /home/uftp/welcome.txt
</code></pre><p><a id="stage-2-step-1-welcome"></a></p>
<blockquote>
<p>方便用户登录后可以看到欢迎信息，并且确定用户确实登录到了主目录上。</p>
</blockquote>
<p><a id="stage-2-step-1-ftp-home"></a></p>
<blockquote>
<p>用户的主目录是用户通过 FTP 登录后看到的根目录</p>
</blockquote>
<h3 id="-uftp-">新建用户 uftp 并设置密码</h3>
<p>创建一个用户 <code>uftp</code> <sup>[<a href="#stage-2-step-2-user">?</a>]</sup>：</p>
<pre><code>sudo useradd -d /home/uftp -s /bin/bash uftp
</code></pre><p>为用户 <code>uftp</code> 设置密码 <sup>[<a href="#stage-2-step-2-password">?</a>]</sup>：</p>
<pre><code>sudo passwd uftp
</code></pre><p>删除掉 pam.d 中 vsftpd，因为该配置文件会导致使用用户名登录 ftp 失败：</p>
<pre><code>sudo rm /etc/pam.d/vsftpd
</code></pre><p><a id="stage-2-step-2-user"></a></p>
<blockquote>
<p>为了方便后面的实验步骤，不建议使用其它的用户名</p>
</blockquote>
<p><a id="stage-2-step-2-password"></a></p>
<blockquote>
<p>请记住设置的密码以用于后续步骤</p>
</blockquote>
<h3 id="-ftp-">限制该用户仅能通过 FTP 访问</h3>
<p>限制用户 <code>uftp</code> 只能通过 FTP 访问服务器，而不能直接登录服务器：</p>
<pre><code>sudo usermod -s /sbin/nologin uftp
</code></pre><h3 id="-vsftpd-">修改 vsftpd 配置</h3>
<pre><code>sudo chmod a+w /etc/vsftpd.conf
</code></pre><p>修改 <a target="_blank" href="/etc/vsftpd.conf" title="null">/etc/vsftpd.conf</a> 文件中的配置（直接将如下配置添加到配置文件最下方）：</p>
<pre><code># 限制用户对主目录以外目录访问
chroot_local_user=YES

# 指定一个 userlist 存放允许访问 ftp 的用户列表
userlist_deny=NO
userlist_enable=YES

# 记录允许访问 ftp 用户列表
userlist_file=/etc/vsftpd.user_list

# 不配置可能导致莫名的530问题
seccomp_sandbox=NO

# 允许文件上传
write_enable=YES

# 使用utf8编码
utf8_filesystem=YES
</code></pre><p>新建文件 <code>/etc/vsftpd.user_list</code>，用于存放允许访问 ftp 的用户：</p>
<pre><code>sudo touch /etc/vsftpd.user_list
sudo chmod a+w /etc/vsftpd.user_list
</code></pre><p>修改 <a target="_blank" href="/etc/vsftpd.user_list" title="null">/etc/vsftpd.user_list</a> ，加入刚刚创建的用户：</p>
<h5 id="-etc-vsftpd-user_list">示例代码：/etc/vsftpd.user_list</h5>
<pre><code>uftp
</code></pre><h3 id="-">设置访问权限</h3>
<p>设置主目录访问权限（只读）：</p>
<pre><code>sudo chmod a-w /home/uftp
</code></pre><p>新建公共目录，并设置权限（读写）：</p>
<pre><code>sudo mkdir /home/uftp/public &amp;&amp; sudo chmod 777 -R /home/uftp/public
</code></pre><p>重启vsftpd 服务：</p>
<pre><code>sudo systemctl restart vsftpd.service
</code></pre><h2 id="-">准备域名和证书</h2>
<blockquote>
<p>任务时间：15min ~ 30min</p>
</blockquote>
<p>注：如果您不需要通过域名访问 FTP 服务器则可以直接点击“已完成，下一步”跳过域名和证书的准备环节</p>
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
<p>域名设置解析后需要过一段时间才会生效，通过 <code>ping</code> 命令检查域名是否生效 <sup>[<a href="#stage-3-step-2-replace">?</a>]</sup>，如：</p>
<pre><code>ping www.yourdomain.com
</code></pre><p>如果 ping 命令返回的信息中含有你设置的解析的 IP 地址，说明解析成功。</p>
<p><a id="stage-3-step-2-replace"></a></p>
<blockquote>
<p>注意替换下面命令中的 <code>www.yourmpdomain.com</code> 为您自己的注册的域名</p>
</blockquote>
<h2 id="-ftp-">访问 FTP 服务</h2>
<blockquote>
<p>任务时间：5min ~ 10min</p>
</blockquote>
<p>FTP 服务已安装并配置完成，下面我们来使用该 FTP 服务</p>
<h3 id="-ftp-">访问 FTP 服务</h3>
<p>根据您个人的工作环境，选择一种方式来访问已经搭建的 FTP 服务</p>
<h4 id="-ftp-">通过 FTP 客户端工具访问</h4>
<p>FTP 客户端工具众多，下面推荐两个常用的：</p>
<ul>
<li><em>FileZilla</em> - 跨平台的 FTP 客户端，支持 Windows 和 Mac</li>
<li><em>WinSCP</em> - Windows 下的 FTP 和 SFTP 连接客户端</li>
</ul>
<p>下载和安装 FTP 客户端后，使用下面的凭据进行连接即可：</p>
<p><sup>[<a href="#stage-4-step-1-host">主机</a>]</sup>：</p>
<pre><code>&lt;您的 CVM IP 地址&gt;
</code></pre><p>用户：</p>
<pre><code>uftp
</code></pre><p>输入密码后，如果能够正常连接，那么大功告成，您可以开始使用属于您自己的 FTP 服务器了！</p>
<p>接下来，请上传任意一张图片到您的 FTP 服务器上的 uftp 的 public 目录下，然后，就可以在 <a target="_blank" href="/home/uftp/public" title="null">/home/uftp/public</a> 中看到了。</p>
<h4 id="-windows-">通过 Windows 资源管理器访问</h4>
<p>Windows 用户可以复制下面的<sup>[<a href="#stage-4-step-1-address">链接</a>]</sup>到资源管理器的地址栏访问：</p>
<pre><code>ftp://uftp:你的密码@&lt;您的 CVM IP 地址&gt;
</code></pre><p><a id="stage-4-step-1-host"></a></p>
<blockquote>
<p>如果您申请了域名，可以将Ip 地址替换为对应的域名作为访问凭据</p>
</blockquote>
<p><a id="stage-4-step-1-address"></a></p>
<blockquote>
<p>如果您申请了域名，可以将链接中的 Ip 地址替换为对应的域名访问 FTP 服务</p>
</blockquote>
<h3 id="-">大功告成</h3>
<p>恭喜！您已经成功完成了搭建 FTP 服务器的实验任务。</p>
</div>