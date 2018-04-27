<div class="lab-edi-doc"><h1 id="-hadoop-">搭建 Hadoop 伪分布式环境</h1>
<h2 id="-">教程环境和说明</h2>
<blockquote>
<p>任务时间：时间未知</p>
</blockquote>
<h3 id="-">软硬件环境</h3>
<ul>
<li>CentOS 7.2 64位</li>
<li>OpenJDK-1.7</li>
<li>Hadoop-2.7</li>
</ul>
<h3 id="-">关于本教程的说明</h3>
<p>云实验室云主机自动使用<code>root</code>账户登录系统，因此本教程中所有的操作都是以<code>root</code>用户来执行的。若要在自己的云主机上进行本教程的实验，为了系统安全，建议新建一个账户登录后再进行后续操作。</p>
<h2 id="-ssh-">安装 SSH 客户端</h2>
<blockquote>
<p>任务时间：1min ~ 5min</p>
</blockquote>
<h3 id="-ssh">安装SSH</h3>
<p>安装SSH：</p>
<pre><code>sudo yum install openssh-clients openssh-server
</code></pre><p>安装完成后，可以使用下面命令进行测试：</p>
<pre><code>ssh localhost
</code></pre><p>输入root账户的密码，如果可以正常登录，则说明SSH安装没有问题。测试正常后使用<code>exit</code>命令退出ssh。</p>
<h2 id="-java-">安装 JAVA 环境</h2>
<blockquote>
<p>任务时间：5min ~ 10min</p>
</blockquote>
<h3 id="-jdk">安装 JDK</h3>
<p>使用yum来安装1.7版本OpenJDK：</p>
<pre><code>sudo yum install java-1.7.0-openjdk java-1.7.0-openjdk-devel
</code></pre><p>安装完成后，输入<code>java</code>和<code>javac</code>命令，如果能输出对应的命令帮助，则表明jdk已正确安装。</p>
<h3 id="-java-">配置 JAVA 环境变量</h3>
<p>执行命令:</p>
<p><em>编辑 ~/.bashrc</em>，在结尾追加：</p>
<pre><code>export JAVA_HOME=/usr/lib/jvm/java-1.7.0-openjdk
</code></pre><p>保存文件后执行下面命令使JAVA_HOME环境变量生效:</p>
<pre><code>source ~/.bashrc
</code></pre><p>为了检测系统中JAVA环境是否已经正确配置并生效，可以分别执行下面命令:</p>
<pre><code>java -version
$JAVA_HOME/bin/java -version
</code></pre><p>若两条命令输出的结果一致，且都为我们前面安装的openjdk-1.7.0的版本，则表明JDK环境已经正确安装并配置。</p>
<h2 id="-hadoop">安装 Hadoop</h2>
<blockquote>
<p>任务时间：10min ~ 15min</p>
</blockquote>
<h3 id="-hadoop">下载 Hadoop</h3>
<p>本教程使用hadoop-2.7版本，使用wget工具在线下载（注：本教程是从清华大学的镜像源下载，如果下载失败或报错，可以自己在网上找到国内其他一个镜像源下载2.7版本的hadoop即可）：</p>
<pre><code>wget https://mirrors.tuna.tsinghua.edu.cn/apache/hadoop/common/hadoop-2.7.4/hadoop-2.7.4.tar.gz
</code></pre><h3 id="-hadoop">安装 Hadoop</h3>
<p>将Hadoop安装到/usr/local目录下:</p>
<pre><code>tar -zxf hadoop-2.7.4.tar.gz -C /usr/local
</code></pre><p>对安装的目录进行重命名，便于后续操作方便:</p>
<pre><code>cd /usr/local
mv ./hadoop-2.7.4/ ./hadoop
</code></pre><p>检查Hadoop是否已经正确安装:</p>
<pre><code>/usr/local/hadoop/bin/hadoop version
</code></pre><p>如果成功输出hadoop的版本信息，表明hadoop已经成功安装。</p>
<h2 id="hadoop-">Hadoop 伪分布式环境配置</h2>
<blockquote>
<p>任务时间：15min ~ 30min</p>
</blockquote>
<p>Hadoop伪分布式模式使用多个守护线程模拟分布的伪分布运行模式。</p>
<h3 id="-hadoop-">设置 Hadoop 的环境变量</h3>
<p><em>编辑 ~/.bashrc</em>，在结尾追加如下内容：</p>
<pre><code>export HADOOP_HOME=/usr/local/hadoop
export HADOOP_INSTALL=$HADOOP_HOME
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin
</code></pre><p>使Hadoop环境变量配置生效:</p>
<pre><code>source ~/.bashrc
</code></pre><h3 id="-hadoop-">修改 Hadoop 的配置文件</h3>
<p>Hadoop的配置文件位于安装目录的/etc/hadoop目录下，在本教程中即位于/url/local/hadoop/etc/hadoop目录下，需要修改的配置文件为如下两个:</p>
<pre><code>/usr/local/hadoop/etc/hadoop/core-site.xml
/usr/local/hadoop/etc/hadoop/hdfs-site.xml
</code></pre><p><em>编辑 core-site.xml</em>，修改<code>&lt;configuration&gt;&lt;/configuration&gt;</code>节点的内容为如下所示：</p>
<h5 id="-usr-local-hadoop-etc-hadoop-core-site-xml">示例代码：/usr/local/hadoop/etc/hadoop/core-site.xml</h5>
<pre><code>&lt;configuration&gt;
    &lt;property&gt;
        &lt;name&gt;hadoop.tmp.dir&lt;/name&gt;
        &lt;value&gt;file:/usr/local/hadoop/tmp&lt;/value&gt;
        &lt;description&gt;location to store temporary files&lt;/description&gt;
    &lt;/property&gt;
    &lt;property&gt;
        &lt;name&gt;fs.defaultFS&lt;/name&gt;
        &lt;value&gt;hdfs://localhost:9000&lt;/value&gt;
    &lt;/property&gt;
&lt;/configuration&gt;
</code></pre><p>同理，<em>编辑 hdfs-site.xml</em>，修改<code>&lt;configuration&gt;&lt;/configuration&gt;</code>节点的内容为如下所示：</p>
<h5 id="-usr-local-hadoop-etc-hadoop-hdfs-site-xml">示例代码：/usr/local/hadoop/etc/hadoop/hdfs-site.xml</h5>
<pre><code>&lt;configuration&gt;
    &lt;property&gt;
        &lt;name&gt;dfs.replication&lt;/name&gt;
        &lt;value&gt;1&lt;/value&gt;
    &lt;/property&gt;
    &lt;property&gt;
        &lt;name&gt;dfs.namenode.name.dir&lt;/name&gt;
        &lt;value&gt;file:/usr/local/hadoop/tmp/dfs/name&lt;/value&gt;
    &lt;/property&gt;
    &lt;property&gt;
        &lt;name&gt;dfs.datanode.data.dir&lt;/name&gt;
        &lt;value&gt;file:/usr/local/hadoop/tmp/dfs/data&lt;/value&gt;
    &lt;/property&gt;
&lt;/configuration&gt;
</code></pre><h3 id="-namenode">格式化 NameNode</h3>
<p>格式化NameNode:</p>
<pre><code>/usr/local/hadoop/bin/hdfs namenode -format
</code></pre><p>在输出信息中看到如下信息，则表示格式化成功:</p>
<pre><code>Storage directory /usr/local/hadoop/tmp/dfs/name has been successfully formatted.
Exiting with status 0
</code></pre><h3 id="-namenode-datanode-">启动 NameNode 和 DataNode 守护进程</h3>
<p>启动NameNode和DataNode进程:</p>
<pre><code>/usr/local/hadoop/sbin/start-dfs.sh
</code></pre><p>执行过程中会提示输入用户密码，输入root用户密码即可。另外，启动时ssh会显示警告提示是否继续连接，输入yes即可。   </p>
<p>检查 NameNode 和 DataNode 是否正常启动:</p>
<pre><code>jps
</code></pre><p>如果NameNode和DataNode已经正常启动，会显示NameNode、DataNode和SecondaryNameNode的进程信息:</p>
<pre><code>[hadoop@VM_80_152_centos ~]$ jps
3689 SecondaryNameNode
3520 DataNode
3800 Jps
3393 NameNode
</code></pre><h2 id="-hadoop-">运行 Hadoop 伪分布式实例</h2>
<blockquote>
<p>任务时间：10min ~ 20min</p>
</blockquote>
<p>Hadoop自带了丰富的例子，包括 wordcount、grep、sort 等。下面我们将以grep例子为教程，输入一批文件，从中筛选出符合正则表达式<code>dfs[a-z.]+</code>的单词并统计出现的次数。</p>
<h3 id="-hadoop-">查看 Hadoop 自带的例子</h3>
<p>Hadoop 附带了丰富的例子, 执行下面命令可以查看：</p>
<pre><code>cd /usr/local/hadoop
./bin/hadoop jar ./share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.4.jar
</code></pre><h3 id="-hdfs-">在 HDFS 中创建用户目录</h3>
<p>在 HDFS 中创建用户目录 hadoop：</p>
<pre><code>/usr/local/hadoop/bin/hdfs dfs -mkdir -p /user/hadoop
</code></pre><h3 id="-">准备实验数据</h3>
<p>本教程中，我们将以 Hadoop 所有的 xml 配置文件作为输入数据来完成实验。执行下面命令在 HDFS 中新建一个 input 文件夹并将 hadoop 配置文件上传到该文件夹下：</p>
<pre><code>cd /usr/local/hadoop
./bin/hdfs dfs -mkdir /user/hadoop/input
./bin/hdfs dfs -put ./etc/hadoop/*.xml /user/hadoop/input
</code></pre><p>使用下面命令可以查看刚刚上传到 HDFS 的文件:</p>
<pre><code>/usr/local/hadoop/bin/hdfs dfs -ls /user/hadoop/input
</code></pre><h3 id="-">运行实验</h3>
<p>运行实验:</p>
<pre><code>cd /usr/local/hadoop
./bin/hadoop jar ./share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.4.jar grep /user/hadoop/input /user/hadoop/output 'dfs[a-z.]+'
</code></pre><p>上述命令以 HDFS 文件系统中的 input 为输入数据来运行 Hadoop 自带的 grep 程序，提取其中符合正则表达式 dfs[a-z.]+ 的数据并进行次数统计，将结果输出到 HDFS 文件系统的 output 文件夹下。</p>
<h3 id="-">查看运行结果</h3>
<p>上述例子完成后的结果保存在 HDFS 中，通过下面命令查看结果:</p>
<pre><code>/usr/local/hadoop/bin/hdfs dfs -cat /user/hadoop/output/*
</code></pre><p>如果运行成功，可以看到如下结果:</p>
<pre><code>1       dfsadmin
1       dfs.replication
1       dfs.namenode.name.dir
1       dfs.datanode.data.dir
</code></pre><h3 id="-hdfs-">删除 HDFS 上的输出结果</h3>
<p>删除 HDFS 中的结果目录:</p>
<pre><code>/usr/local/hadoop/bin/hdfs dfs -rm -r /user/hadoop/output
</code></pre><p>运行 Hadoop 程序时，为了防止覆盖结果，程序指定的输出目录不能存在，否则会提示错误，因此在下次运行前需要先删除输出目录。</p>
<h3 id="-hadoop-">关闭 Hadoop 进程</h3>
<p>关闭 Hadoop 进程：</p>
<pre><code>/usr/local/hadoop/sbin/stop-dfs.sh
</code></pre><p>再起启动只需要执行下面命令：</p>
<pre><code>/usr/local/hadoop/sbin/start-dfs.sh
</code></pre><h2 id="-">部署完成</h2>
<blockquote>
<p>任务时间：时间未知</p>
</blockquote>
<h3 id="-">大功告成</h3>
<p>恭喜您已经完成了搭建 Hadoop 伪分布式环境的学习，您可以留用或者<em>购买 Linux 版本的 CVM</em> 继续学习。</p>
</div>