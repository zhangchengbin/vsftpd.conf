<div class="lab-edi-doc"><h1 id="-java-web-">搭建 Java Web 开发环境</h1>
<h2 id="-java-">搭建 Java 开发环境</h2>
<blockquote>
<p>任务时间：18min ~ 20min</p>
</blockquote>
<p>此实验教大家如何配置 JDK 、Tomcat 和 Mysql</p>
<h3 id="-jdk">安装 JDK</h3>
<p>JDK 是开发Java程序必须安装的软件，我们查看一下 <code>yum</code> 源里面的 JDK：</p>
<pre><code>yum list java*
</code></pre><p> 选择适合本机的JDK，并安装： </p>
<pre><code>yum install java-1.7.0-openjdk* -y
</code></pre><p>安装完成后，查看是否安装成功：</p>
<pre><code>java -version
</code></pre><h3 id="-tomcat">安装 Tomcat</h3>
<p>Tomcat 是一个应用服务器，是开发和调试 jsp 程序的首选，可以利用它来响应 HTML 页面的访问请求。</p>
<p>进入本地文件夹</p>
<pre><code>cd /usr/local
</code></pre><p>到官网找到 Tomcat 的下载链接，并下载到服务器中, 这里提供了一个快速下载 Tomcat 的地址：</p>
<pre><code>wget https://mc.qcloudimg.com/static/archive/fa66329388f85c08e8d6c12ceb8b2ca3/apache-tomcat-7.0.77.tar.gz
</code></pre><p>解压这个文件夹：</p>
<pre><code>tar -zxf apache-tomcat-7.0.77.tar.gz
</code></pre><p>重命名这个文件<sup>[<a href="#stage-1-step-2-rename-folder">?</a>]</sup>：</p>
<pre><code>mv apache-tomcat-7.0.77 /usr/local/tomcat7
</code></pre><p>进入 bin 文件夹</p>
<pre><code>cd /usr/local/tomcat7/bin
</code></pre><p>给这个文件夹下的所有 shell 脚本授予权限：</p>
<pre><code>chmod 777 *.sh
</code></pre><p>开启tomcat服务：</p>
<pre><code>./startup.sh
</code></pre><p><a id="stage-1-step-2-rename-folder"></a></p>
<blockquote>
<p>重命名是为了方便后续操作, 并非必须步骤</p>
</blockquote>
<h3 id="-mysql">安装 MySQL</h3>
<p>使用 <code>yum</code> 安装 MySQL：</p>
<pre><code>yum install -y mysql-server mysql mysql-devel
</code></pre><p>安装完成后，启动 MySQL 服务：</p>
<pre><code>service mysqld restart
</code></pre><p>设置 MySQL 账户 root 密码：<sup>[<a href="#stage-1-step-3-password">?</a>]</sup></p>
<pre><code>/usr/bin/mysqladmin -u root password 'Password'
</code></pre><p><a id="stage-1-step-3-password"></a></p>
<blockquote>
<p>下面命令中的密码是教程为您自动生成的，为了方便实验的进行，不建议使用其它密码。如果设置其它密码，请把密码记住。</p>
</blockquote>
<h2 id="-tomcat">访问 Tomcat</h2>
<blockquote>
<p>任务时间：3min ~ 5min</p>
</blockquote>
<h3 id="-tomcat">访问 Tomcat</h3>
<p>此时，访问 <a target="_blank" href="http://&lt;您的 CVM IP 地址&gt;:8080" title="null">http://&lt;您的 CVM IP 地址&gt;:8080</a> 可访问到刚才启动的 Tomcat 的内置示例页面 </p>
<h3 id="-">完成实验</h3>
<p>恭喜！您已经成功完成了搭建 Java Web 开发环境的实验任务。</p>
</div>