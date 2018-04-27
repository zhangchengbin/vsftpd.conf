<div class="lab-edi-doc"><h1 id="maven-">Maven 安装与使用</h1>
<h2 id="-maven">安装 Maven</h2>
<blockquote>
<p>任务时间：10min ~ 15min</p>
</blockquote>
<h3 id="maven-">Maven 简介</h3>
<p><strong><a target="_blank" href="http://maven.apache.org/" title="null">Apache Maven</a></strong> 是一个软件项目管理及自动构建工具，由 Apache 软件基金会所提供。基于项目对象模型（缩写：POM）概念，Maven 利用一小段描述信息能管理一个项目的构建、报告和文档等步骤。</p>
<h3 id="java-">Java 安装</h3>
<p>首先我们需要准备 Java 开发环境，使用下面命令来安装 Java：</p>
<pre><code>yum -y install java-1.8.0-openjdk-devel
</code></pre><h3 id="maven-">Maven 下载</h3>
<p>我们可以从<em>官网下载页</em>获取最新的下载链接（Binary tar.gz archive）。</p>
<p>然后我们使用 <code>wget</code> 命令将其下载：</p>
<pre><code>cd /home
wget http://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/3.5.0/binaries/apache-maven-3.5.0-bin.tar.gz
</code></pre><p>解压压缩包：</p>
<pre><code>tar xzvf apache-maven-3.5.0-bin.tar.gz
</code></pre><p>将文件夹移动至 <code>/usr/local/</code> 目录：</p>
<pre><code>mv apache-maven-3.5.0  /usr/local/apache-maven
</code></pre><h3 id="-">配置环境变量</h3>
<p>编辑 <em>/etc/profile</em>，在最下方添加：</p>
<pre><code>MAVEN_HOME=/usr/local/apache-maven
export MAVEN_HOME
export PATH=${PATH}:${MAVEN_HOME}/bin
</code></pre><p><code>Ctrl + S</code> 保存文件，并运行如下命令使环境变量生效：</p>
<pre><code>source /etc/profile
</code></pre><p>检查 Maven 是否成功安装：</p>
<pre><code>mvn -version
</code></pre><h2 id="maven-">Maven 简单使用</h2>
<blockquote>
<p>任务时间：25min ~ 30min</p>
</blockquote>
<h3 id="-">构建</h3>
<p>我们可以通过 <code>archetype:generate</code> 命令快速构建出项目骨架。</p>
<h4 id="hello-world">Hello World</h4>
<p>我们使用该命令创建一个 <code>helloworld</code> 项目。过程中可一路回车键选择默认值。</p>
<pre><code>cd /home
mvn archetype:generate -DgroupId=helloworld -DartifactId=helloworld
</code></pre><ul>
<li><strong> <code>mvn</code> 指令首次执行时，会从远程“中央仓库”下载一些必需的文件，请耐心等待。</strong></li>
</ul>
<h4 id="-">项目结构</h4>
<p>点击 <em>/home/helloworld</em> 查看项目结构。</p>
<p>其中：</p>
<ul>
<li><strong>/pom.xml</strong> 为项目对象模型（Maven 项目配置）</li>
<li><strong>/src/main/java</strong> 用于存放源代码</li>
<li><strong>/src/test/java</strong> 用于存放单元测试代码</li>
<li><strong>/src/target</strong> 用于存放编译、打包后的输出文件</li>
</ul>
<h3 id="-">编译</h3>
<p>进入项目目录：</p>
<pre><code>cd /home/helloworld
</code></pre><p>执行编译：</p>
<pre><code>mvn compile
</code></pre><p>重新开启 <strong>helloworld</strong> 项目文件夹，可以看到生成 <strong>target</strong> 目录。</p>
<h3 id="-">运行</h3>
<p>你可以使用 <code>mvn</code> 指明 <strong>mainClass</strong> 来运行项目：</p>
<pre><code>mvn exec:java -Dexec.mainClass="helloworld.App"
</code></pre><p>完成后可看到终端输出了：</p>
<p><strong>Hello World!</strong></p>
<h3 id="-">测试</h3>
<p>我们可以通过 <code>test</code> 指令来运行单元测试代码。</p>
<pre><code>mvn test
</code></pre><p>完成后可看到终端输出测试结果。</p>
<h3 id="-">打包</h3>
<p>通过 <code>package</code> 指令来执行打包。</p>
<pre><code>mvn package
</code></pre><p>重新开启 <code>target</code> 目录，可看到生成了 <strong>.jar</strong> 文件。</p>
<ul>
<li>从输出的日志可以看到，执行 <code>package</code> 前，会先执行 <code>compile</code> 及 <code>test</code>，最后执行了打包。</li>
</ul>
<h3 id="-">完成实验</h3>
<p>恭喜！您已经成功完成了 <strong>Maven 安装与入门</strong> 的实验任务，您可以选择 <strong>留用</strong> 继续使用该环境。</p>
</div>