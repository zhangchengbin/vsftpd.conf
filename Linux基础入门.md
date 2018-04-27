<div class="lab-edi-doc"><h1 id="linux-">Linux 基础入门</h1>
<h2 id="-">目录操作</h2>
<blockquote>
<p>任务时间：5min ~ 10min</p>
</blockquote>
<h3 id="-">创建目录</h3>
<p>使用 mkdir 命令创建目录</p>
<pre><code>mkdir $HOME/testFolder
</code></pre><h3 id="-">切换目录</h3>
<p>使用 cd 命令切换目录</p>
<pre><code>cd $HOME/testFolder
</code></pre><p>使用 cd ../ 命令切换到上一级目录</p>
<pre><code>cd ../
</code></pre><h3 id="-">移动目录</h3>
<p>使用 mv 命令移动目录</p>
<pre><code>mv $HOME/testFolder /var/tmp
</code></pre><h3 id="-">删除目录</h3>
<p>使用 rm -rf 命令删除目录</p>
<pre><code>rm -rf /var/tmp/testFolder
</code></pre><h3 id="-">查看目录下的文件</h3>
<p>使用 ls 命令查看 <sup>[<a href="#stage-1-step-5-etc">/etc</a>]</sup> 目录下所有文件和文件夹</p>
<pre><code>ls /etc
</code></pre><p><a id="stage-1-step-5-etc"></a></p>
<blockquote>
<p>/etc 目录默认是 *nix 系统的软件配置文件存放位置</p>
</blockquote>
<h2 id="-">文件操作</h2>
<blockquote>
<p>任务时间：5min ~ 10min</p>
</blockquote>
<h3 id="-">创建文件</h3>
<p>使用 touch 命令创建文件</p>
<pre><code>touch ~/testFile
</code></pre><p>执行 <code>ls</code> 命令, 可以看到刚才新建的 testFile 文件</p>
<pre><code>ls ~
</code></pre><h3 id="-">复制文件</h3>
<p>使用 cp 命令复制文件</p>
<pre><code>cp ~/testFile ~/testNewFile
</code></pre><h3 id="-">删除文件</h3>
<p>使用 rm 命令删除文件, 输入 <code>y</code> 后回车确认删除</p>
<pre><code>rm ~/testFile
</code></pre><h3 id="-">查看文件内容</h3>
<p>使用 cat 命令查看 .bash_history 文件内容</p>
<pre><code>cat ~/.bash_history
</code></pre><h2 id="-">过滤, 管道与重定向</h2>
<blockquote>
<p>任务时间：5min ~ 10min</p>
</blockquote>
<h3 id="-">过滤</h3>
<p>过滤出 /etc/passwd 文件中包含 <code>root</code> 的记录</p>
<pre><code>grep 'root' /etc/passwd
</code></pre><p>递归地过滤出 /var/log/ 目录中包含 <code>linux</code> 的记录</p>
<pre><code>grep -r 'linux' /var/log/
</code></pre><h3 id="-">管道</h3>
<p>简单来说, Linux 中管道的作用是将上一个命令的输出作为下一个命令的输入, 像 pipe 一样将各个命令串联起来执行, 管道的操作符是 |</p>
<p>比如, 我们可以将 cat 和 grep 两个命令用管道组合在一起</p>
<pre><code>cat /etc/passwd | grep 'root'
</code></pre><p>过滤出 /etc 目录中名字包含 <code>ssh</code> 的目录(不包括子目录)</p>
<pre><code>ls /etc | grep 'ssh'
</code></pre><h3 id="-">重定向</h3>
<p>可以使用 &gt; 或 &lt; 将命令的输出重定向到一个文件中</p>
<pre><code>echo 'Hello World' &gt; ~/test.txt
</code></pre><h2 id="-">运维常用命令</h2>
<blockquote>
<p>任务时间：5min ~ 10min</p>
</blockquote>
<h3 id="ping-">ping 命令</h3>
<p>对 cloud.tencent.com 发送 4 个 ping 包, 检查与其是否联通</p>
<pre><code>ping -c 4 cloud.tencent.com
</code></pre><h3 id="netstat-">netstat 命令</h3>
<p>netstat 命令用于显示各种网络相关信息，如网络连接, 路由表, 接口状态等等</p>
<p>列出所有处于监听状态的tcp端口</p>
<pre><code>netstat -lt
</code></pre><p>查看所有的端口信息, 包括 PID 和进程名称</p>
<pre><code>netstat -tulpn
</code></pre><h3 id="ps-">ps 命令</h3>
<p>过滤得到当前系统中的 ssh 进程信息</p>
<pre><code>ps -aux | grep 'ssh'
</code></pre><h3 id="-">完成实验</h3>
<p>恭喜！您已经成功完成了 Linux 入门运维的实验任务，您可以留用或者<em>购买 Linux 版本的 CVM</em> 继续使用。</p>
</div>