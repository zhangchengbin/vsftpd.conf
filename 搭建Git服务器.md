<div class="lab-edi-doc"><h1 id="-git-">搭建 GIT 服务器教程</h1>
<h2 id="-git">下载安装 git</h2>
<blockquote>
<p>任务时间：5min ~ 10min</p>
</blockquote>
<p>Git 是一款免费、开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。</p>
<p>此实验以 CentOS 7.2 x64 的系统为环境，搭建 git 服务器。</p>
<h3 id="-">安装依赖库和编译工具</h3>
<p>为了后续安装能正常进行，我们先来安装一些相关依赖库和编译工具</p>
<pre><code>yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel
</code></pre><p>安装编译工具</p>
<pre><code>yum install gcc perl-ExtUtils-MakeMaker
</code></pre><h3 id="-git">下载 git</h3>
<p>选一个目录，用来放下载下来的安装包，这里将安装包放在 <em>/usr/local/src</em>  目录里</p>
<pre><code>cd /usr/local/src
</code></pre><p>到官网找一个新版稳定的源码包下载到 <code>/usr/local/src</code> 文件夹里</p>
<pre><code>wget https://www.kernel.org/pub/software/scm/git/git-2.10.0.tar.gz
</code></pre><h3 id="-">解压和编译</h3>
<p>解压下载的源码包</p>
<pre><code>tar -zvxf git-2.10.0.tar.gz
</code></pre><p>解压后进入 git-2.10.0 文件夹</p>
<pre><code>cd git-2.10.0
</code></pre><p>执行编译</p>
<pre><code>make all prefix=/usr/local/git
</code></pre><p>编译完成后, 安装到 /usr/local/git 目录下</p>
<pre><code>make install prefix=/usr/local/git
</code></pre><h2 id="-">配置环境变量</h2>
<blockquote>
<p>任务时间：5min ~ 10min</p>
</blockquote>
<h3 id="-git-path">将 git 目录加入 PATH</h3>
<p>将原来的 PATH 指向目录修改为现在的目录</p>
<pre><code>echo 'export PATH=$PATH:/usr/local/git/bin' &gt;&gt; /etc/bashrc
</code></pre><p>生效环境变量</p>
<pre><code>source /etc/bashrc
</code></pre><p>此时我们能查看 git 版本号，说明我们已经安装成功了。</p>
<pre><code>git --version
</code></pre><h2 id="-git-">创建 git 账号密码</h2>
<blockquote>
<p>任务时间：5min ~ 10min</p>
</blockquote>
<h3 id="-git-">创建 git 账号</h3>
<p>为我们刚刚搭建好的 git 创建一个账号</p>
<pre><code>useradd -m gituser
</code></pre><p>然后为这个账号设置密码<sup>[<a href="#stage-3-step-1-pwd">?</a>]</sup></p>
<pre><code>passwd gituser
</code></pre><p><a id="stage-3-step-1-pwd"></a></p>
<blockquote>
<p>控制台输入创建密码后，输入您自定义的密码，并二次确认。</p>
</blockquote>
<h2 id="-git-">初始化 git 仓库并配置用户权限</h2>
<blockquote>
<p>任务时间：5min ~ 10min</p>
</blockquote>
<h3 id="-git-">创建 git 仓库并初始化</h3>
<p>我们创建 <em>/data/repositories</em> 目录用于存放 git 仓库</p>
<pre><code>mkdir -p /data/repositories
</code></pre><p>创建好后，初始化这个仓库</p>
<pre><code>cd /data/repositories/ &amp;&amp; git init --bare test.git
</code></pre><h3 id="-">配置用户权限</h3>
<p>给 git 仓库目录设置用户和用户组并设置权限</p>
<pre><code>chown -R gituser:gituser /data/repositories
</code></pre><pre><code>chmod 755 /data/repositories
</code></pre><p><sup>[<a href="#stage-4-step-2-git-shell-location">查找 git-shell 所在目录</a>]</sup> , 编辑 <em>/etc/passwd</em> 文件，将最后一行关于 <code>gituser</code> 的登录 shell 配置改为 git-shell 的目录<sup>[<a href="#stage-4-step-2-ssh">?</a>]</sup>如下</p>
<h5 id="-etc-passwd">示例代码：/etc/passwd</h5>
<pre><code class="lang-conf">gituser:x:500:500::/home/gituser:/usr/local/git/bin/git-shell
</code></pre>
<p><a id="stage-4-step-2-git-shell-location"></a></p>
<blockquote>
<p>如果按照刚才的步骤执行, 这个位置应该是 /usr/local/git/bin/git-shell, 否则请通过 <code>which git-shell</code> 命令查看位置</p>
</blockquote>
<p><a id="stage-4-step-2-ssh"></a></p>
<blockquote>
<p>安全目的, 限制 git 账号的 ssh 连接只能是登录 git-shell</p>
</blockquote>
<h3 id="-git-">使用搭建好的 Git 服务</h3>
<p>克隆 test repo 到本地</p>
<pre><code>cd ~ &amp;&amp; git clone gituser@&lt;您的 CVM IP 地址&gt;:/data/repositories/test.git
</code></pre><h3 id="-">实验完成</h3>
<p>恭喜，Git 服务器搭建完成, 从此以后你可以方便地将你的本地代码提交到 Git 服务器托管了</p>
</div>