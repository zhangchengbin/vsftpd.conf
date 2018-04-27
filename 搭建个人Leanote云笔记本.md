<div class="lab-edi-doc"><h1 id="-">使用腾讯云自建一个专属于自己的网络笔记本</h1>
<h2 id="-mongodb">下载启动 MongoDB</h2>
<blockquote>
<p>任务时间：10min ~ 15min</p>
</blockquote>
<p>Leanote 依赖 MongoDB 作为数据存储，下面开始安装 MongoDB：</p>
<h3 id="-mongodb">下载 MongoDB</h3>
<p>进入 <em>/home</em> 目录，并下载 MongoDB：</p>
<pre><code>cd /home
</code></pre><p>下载源码：</p>
<pre><code>wget https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-3.0.1.tgz
</code></pre><p>解压缩源码包：</p>
<pre><code>tar -xzvf mongodb-linux-x86_64-3.0.1.tgz
</code></pre><h3 id="-">创建用于存储的文件夹目录</h3>
<pre><code>mkdir -p /data/db
</code></pre><p>配置 MongoDB 的环境变量：</p>
<p>编辑 <em>/etc/profile</em>，在文件末尾追加以下配置：</p>
<h5 id="-etc-profile">示例代码：/etc/profile</h5>
<pre><code>
export PATH=$PATH:/home/mongodb-linux-x86_64-3.0.1/bin
</code></pre><p>并执行以下命令，使环境变量生效。</p>
<pre><code>source /etc/profile
</code></pre><h3 id="-mongodb-3-5-">启动 MongoDB（启动需要 3 ~ 5 分钟，耐心等待）：</h3>
<pre><code>mongod --bind_ip localhost --port 27017 --dbpath /data/db/ --logpath=/var/log/mongod.log --fork
</code></pre><checker type="output-contains" command="ps -ef | grep mongod | gr" hint="请确认已启动 MongoDB">
   <keyword regex="mongod">
</keyword></checker>




<h2 id="-leanote">安装 Leanote</h2>
<blockquote>
<p>任务时间：10min ~ 15min</p>
</blockquote>
<p>Leanote 是一款 Linux 下开源的软件，下面开始安装 Leanote：</p>
<h3 id="-leanote">下载 Leanote</h3>
<p>先进入 <em>/home</em> 目录</p>
<pre><code>cd /home
</code></pre><p>下载 Leanote 源码</p>
<pre><code>wget https://iweb.dl.sourceforge.net/project/leanote-bin/2.4/leanote-linux-amd64-v2.4.bin.tar.gz
</code></pre><h3 id="-">解开压缩包：</h3>
<pre><code>tar -zxvf leanote-linux-amd64-v2.4.bin.tar.gz
</code></pre><h3 id="-leanote-">编辑 Leanote 配置文件</h3>
<p>编辑文件 <em>app.conf</em>，在文件中找到 <code>app.secret=</code> 项，并修改为如下内容：</p>
<pre><code>app.secret=qcloud666
</code></pre><h3 id="-">初始化数据库</h3>
<p>导入初始化数据：</p>
<pre><code>mongorestore -h localhost -d leanote --dir /home/leanote/mongodb_backup/leanote_install_data/
</code></pre><checker type="output-contains" command="ls -la /data/db" hint="请检查初始化数据是否导入数据库">
   <keyword regex="leanote">
</keyword></checker>



<h3 id="-leanote-">启动 Leanote 服务</h3>
<pre><code>nohup /bin/bash /home/leanote/bin/run.sh &gt;&gt; /var/log/leanote.log 2&gt;&amp;1 &amp;
</code></pre><h2 id="-">准备域名和证书</h2>
<blockquote>
<p>任务时间：15min ~ 30min</p>
</blockquote>
<p>注：如果您不需要通过域名访问 Leanote 云笔记本则可以直接点击“已完成，下一步”跳过域名和证书的准备环节</p>
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
<h2 id="-leanote-">访问 Leanote 云笔记本</h2>
<blockquote>
<p>任务时间：时间未知</p>
</blockquote>
<h3 id="-ip-">通过 ip 访问笔记本</h3>
<p>通过访问 http://&lt;您的 CVM IP 地址&gt;:9000 就可以了使用自己的笔记本。</p>
<ul>
<li>初始化账户： <code>admin</code></li>
<li>初始化密码： <code>abc123</code></li>
</ul>
<p>请务必修改密码已确保使用安全！</p>
<h3 id="-">通过域名访问笔记本</h3>
<p>如果您申请了域名，可以将 Ip 地址替换为对应的域名作为访问凭据，如：<a target="_blank" href="http://www.yourmpdomain.com:9000" title="null">http://www.yourmpdomain.com:9000</a> , 注：请将 <code>www.yourmpdomain.com</code> 替换为您申请的域名。</p>
<h3 id="-">大功告成</h3>
<p>恭喜！您已经成功完成了搭建 Leanote 云笔记本的实验任务。</p>
</div>