<div class="lab-edi-doc"><h1 id="-hdfs-">搭建基于 HDFS 碎片文件存储服务</h1>
<h2 id="-java-">准备 Java 环境</h2>
<blockquote>
<p>任务时间：5min ~ 10min</p>
</blockquote>
<h3 id="-jdk">安装 JDK</h3>
<p>HDFS 依赖 Java 环境，这里我们使用 yum 安装 JDK 8，在终端中键入如下命令：</p>
<pre><code>yum -y install java-1.8.0-openjdk*
</code></pre><p>使用如下命令查看下 Java 版本，我们可以验证 JDK 是否已成功安装：</p>
<pre><code>java -version
</code></pre><h3 id="-java-">配置 Java 环境变量</h3>
<p>在编辑器中打开文件 <em>/etc/profile</em>，在文件末尾追加如下内容，配置 Java 环境变量：</p>
<pre><code>export JAVA_HOME=/usr/lib/jvm/jre-1.8.0-openjdk
export PATH=$PATH:$JAVA_HOME/bin
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
</code></pre><p>然后执行如下命令，让环境变量生效：</p>
<pre><code>source /etc/profile
</code></pre><p>通过如下命令，验证 Java 环境变量是否已成功配置并且生效：</p>
<pre><code>echo $JAVA_HOME
</code></pre><h2 id="-hdfs-">准备 HDFS 环境</h2>
<blockquote>
<p>任务时间：10min ~ 15min</p>
</blockquote>
<h3 id="-ssh">配置 SSH</h3>
<p>先后执行如下两行命令，配置 SSH 以无密码模式登陆：</p>
<pre><code>ssh-keygen -t dsa -P '' -f ~/.ssh/id_dsa
</code></pre><pre><code>cat ~/.ssh/id_dsa.pub &gt;&gt; ~/.ssh/authorized_keys
</code></pre><p>接着可以验证下，在不输入密码的情况下，应该能使用 ssh 命令成功连接本机：</p>
<pre><code>ssh &lt;您的 CVM IP 地址&gt;
</code></pre><p>紧接着在终端中键入如下命令关闭 ssh 连接：</p>
<pre><code>exit
</code></pre><p>SSH 配置完毕后，我们接着下载并安装 Hadoop。</p>
<h3 id="-hadoop">安装 Hadoop</h3>
<p>首先创建 /data/hadoop 目录，然后进入该目录：</p>
<pre><code>mkdir -p /data/hadoop &amp;&amp; cd $_
</code></pre><p>接着下载 Hadoop 安装包到该目录下：</p>
<pre><code>wget http://archive.apache.org/dist/hadoop/core/hadoop-2.7.1/hadoop-2.7.1.tar.gz
</code></pre><p>解压下载好的 Hadoop 安装包到当前目录：</p>
<pre><code>tar -zxvf hadoop-2.7.1.tar.gz
</code></pre><p>然后将解压后的目录重命名为 hadoop，并且将其移至 /usr/local/ 目录下：</p>
<pre><code>mv hadoop-2.7.1 hadoop &amp;&amp; mv $_ /usr/local/
</code></pre><p>在编辑器中打开 <em>Hadoop 环境配置文件</em>，使用 <code>Ctrl + F</code> 搜索如下行 <sup>[<a href="#stage-2-step-2-search">?</a>]</sup>：</p>
<pre><code>export JAVA_HOME=${JAVA_HOME}
</code></pre><p>将其替换为：</p>
<pre><code>export JAVA_HOME=/usr/lib/jvm/jre-1.8.0-openjdk
</code></pre><p>查看下 Hadoop 的版本：</p>
<pre><code>/usr/local/hadoop/bin/hadoop version
</code></pre><p>可以看到 Hadoop 的版本信息：</p>
<pre><code>Hadoop 2.7.1
</code></pre><p>安装 Hadoop 就是这么简单。至此 Hadoop 已经安装完成，紧接着我们需要做的就是修改 Hadoop 的配置信息。</p>
<p><a id="stage-2-step-2-search"></a></p>
<blockquote>
<p>Mac 用户请使用 <code>Cmd + F</code> 进行搜索</p>
</blockquote>
<h3 id="-hadoop-">修改 Hadoop 配置</h3>
<p>由于我们的实践环境是在单机下进行的，所以此处把 Hadoop 配置为伪分布式模式。</p>
<p>首先新建若干临时文件夹，我们在后续的配置中以及 HDFS 和 Hadoop 的启动过程中会使用到这些文件夹：</p>
<pre><code>cd /usr/local/hadoop &amp;&amp; mkdir -p tmp dfs/name dfs/data
</code></pre><p>修改 HDFS 配置文件 <em>core-site.xml</em>，将 <code>configuration</code> 配置修改为如下内容：</p>
<h5 id="-usr-local-hadoop-etc-hadoop-core-site-xml">示例代码：/usr/local/hadoop/etc/hadoop/core-site.xml</h5>
<pre><code class="lang-xml">&lt;configuration&gt;  
    &lt;property&gt;  
        &lt;name&gt;hadoop.tmp.dir&lt;/name&gt;  
        &lt;value&gt;/usr/local/hadoop/tmp&lt;/value&gt;
    &lt;/property&gt;  
    &lt;property&gt;  
        &lt;name&gt;fs.defaultFS&lt;/name&gt;  
        &lt;value&gt;hdfs://localhost:9000&lt;/value&gt;
    &lt;/property&gt;  
&lt;/configuration&gt;
</code></pre>
<p>修改 HDFS 配置文件 <em>hdfs-site.xml</em>，将 configuration 配置修改为如下内容：</p>
<h5 id="-usr-local-hadoop-etc-hadoop-hdfs-site-xml">示例代码：/usr/local/hadoop/etc/hadoop/hdfs-site.xml</h5>
<pre><code class="lang-xml">&lt;configuration&gt;    
    &lt;property&gt;    
        &lt;name&gt;dfs.replication&lt;/name&gt;    
        &lt;value&gt;1&lt;/value&gt;    
    &lt;/property&gt;    
    &lt;property&gt;    
        &lt;name&gt;dfs.namenode.name.dir&lt;/name&gt;    
        &lt;value&gt;file:/usr/local/hadoop/dfs/name&lt;/value&gt;    
    &lt;/property&gt;    
    &lt;property&gt;    
        &lt;name&gt;dfs.datanode.data.dir&lt;/name&gt;    
        &lt;value&gt;file:/usr/local/hadoop/dfs/data&lt;/value&gt;    
    &lt;/property&gt;    
    &lt;property&gt;
        &lt;name&gt;dfs.permissions&lt;/name&gt;    
        &lt;value&gt;false&lt;/value&gt;    
    &lt;/property&gt;    
&lt;/configuration&gt;
</code></pre>
<p>修改 HDFS 配置文件 <em>yarn-site.xml</em>，将 configuration 配置修改为如下内容：</p>
<h5 id="-usr-local-hadoop-etc-hadoop-yarn-site-xml">示例代码：/usr/local/hadoop/etc/hadoop/yarn-site.xml</h5>
<pre><code class="lang-xml">&lt;configuration&gt;  
    &lt;property&gt;  
        &lt;name&gt;mapreduce.framework.name&lt;/name&gt;  
        &lt;value&gt;yarn&lt;/value&gt;  
    &lt;/property&gt;  

    &lt;property&gt;  
        &lt;name&gt;yarn.nodemanager.aux-services&lt;/name&gt;  
        &lt;value&gt;mapreduce_shuffle&lt;/value&gt;  
    &lt;/property&gt;  
&lt;/configuration&gt;
</code></pre>
<p>以上配置修改完毕后，我们可以尝试启动 Hadoop。</p>
<h3 id="-hadoop">启动 Hadoop</h3>
<p>首先进入如下目录：</p>
<pre><code>cd /usr/local/hadoop/bin/
</code></pre><p>对 HDFS 文件系统进行格式化：</p>
<pre><code>./hdfs namenode -format
</code></pre><p>接着进入如下目录：</p>
<pre><code>cd /usr/local/hadoop/sbin/
</code></pre><p>先后执行如下两个脚本启动 Hadoop：</p>
<pre><code>./start-dfs.sh
</code></pre><pre><code>./start-yarn.sh
</code></pre><p>接着执行如下命令，验证 Hadoop 是否启动成功：</p>
<pre><code>jps
</code></pre><p>如果输出了类似如下的指令，则说明 Hadoop 启动成功啦 <sup>[<a href="#stage-2-step-4-jps-output">?</a>]</sup>：</p>
<pre><code>21713 Jps
21089 SecondaryNameNode
21364 NodeManager
20795 NameNode
20923 DataNode
21245 ResourceManager
</code></pre><p>至此，我们的 HDFS 环境就已经搭建好了。在浏览器中访问如下链接，应该能正常访问（注：若出现安全拦截请选择通过）：</p>
<pre><code>http://&lt;您的 CVM IP 地址&gt;:50070/explorer.html#/
</code></pre><p>接下来，我们实践下如何将碎片文件存储到 HDFS 中。</p>
<p><a id="stage-2-step-4-jps-output"></a></p>
<blockquote>
<p>其中，第 1 列表示进程 ID，第 2 列表示进程名</p>
</blockquote>
<h2 id="-">存储碎片文件</h2>
<blockquote>
<p>任务时间：10min ~ 15min</p>
</blockquote>
<h3 id="-">准备碎片文件</h3>
<p>首先创建目录用于存放碎片文件，进入该目录：</p>
<pre><code>mkdir -p /data/file &amp;&amp; cd /data/file
</code></pre><p>然后执行如下 shell 命令，新建一批碎片文件到该目录下 <sup>[<a href="#stage-3-step-1-empty-files">?</a>]</sup>：</p>
<pre><code>i=1; while [ $i -le 99 ]; do name=`printf "test%02d.txt"  $i`; touch "$name"; i=$(($i+1)); done
</code></pre><p>在终端中执行 <code>ls</code> 命令，可以看到我们已成功创建了一批碎片文件。</p>
<p><a id="stage-3-step-1-empty-files"></a></p>
<blockquote>
<p>这里，为了简单起见，我们批量生成的文件都是空的</p>
</blockquote>
<h3 id="-hdfs-">将碎片文件存储在 HDFS 中</h3>
<p>首先在 HDFS 上新建目录：</p>
<pre><code>/usr/local/hadoop/bin/hadoop fs -mkdir /dest
</code></pre><p>此时，在浏览器是访问如下链接，可以看到 /dest 目录已创建，但是暂时还没有内容：</p>
<pre><code>http://&lt;您的 CVM IP 地址&gt;:50070/explorer.html#/dest
</code></pre><p>接着我们可以上传碎片文件啦！</p>
<p>首先在终端中依次执行以下命令 <sup>[<a href="#stage-3-step-2-supergroup-required">?</a>]</sup>：</p>
<pre><code>groupadd supergroup
</code></pre><pre><code>usermod -a -G supergroup root
</code></pre><p>然后将之前创建的碎片文件上传到 HDFS 中：</p>
<pre><code>cd /data/file &amp;&amp; /usr/local/hadoop/bin/hadoop fs -put *.txt /dest
</code></pre><p>这时，再使用如下命令，我们应该能看到碎片文件已成功上传到 HDFS 中：</p>
<pre><code>/usr/local/hadoop/bin/hadoop fs -ls /dest
</code></pre><p><a id="stage-3-step-2-supergroup-required"></a></p>
<blockquote>
<p>在上传之前，我们需先创建 HDFS 用户组 supergroup，然后将 root 用户添加到该组中，否则会因权限问题页报告异常。</p>
</blockquote>
<h2 id="-">部署完成</h2>
<blockquote>
<p>任务时间：3min ~ 5min</p>
</blockquote>
<h3 id="-">访问服务</h3>
<p>在浏览器是访问如下链接，可以看到 /dest 目录的文件内容：</p>
<pre><code>http://&lt;您的 CVM IP 地址&gt;:50070/explorer.html#/dest
</code></pre><h3 id="-">大功告成</h3>
<p>恭喜您已经完成了搭建基于 HDFS 碎片文件存储服务的学习，您可以留用或者<em>购买 Linux 版本的 CVM</em> 继续学习。</p>
</div>