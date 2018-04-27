<div class="lab-edi-doc"><h1 id="-docker-">搭建 Docker 环境</h1>
<h2 id="-docker">安装与配置 Docker</h2>
<blockquote>
<p>任务时间：10min ~ 20min</p>
</blockquote>
<h3 id="-docker">安装 Docker</h3>
<p>Docker 软件包已经包括在默认的 CentOS-Extras 软件源里。因此想要安装 docker，只需要运行下面的 yum 命令：</p>
<pre><code>yum install docker-io -y
</code></pre><p>直接yum安装，安装成功后查看版本</p>
<pre><code>docker -v
</code></pre><p>启动docker</p>
<pre><code>service docker start
</code></pre><p>设置开机启动</p>
<pre><code>chkconfig docker on
</code></pre><h3 id="-docker">配置 Docker</h3>
<p>因为国内访问 Docker Hub 较慢, 可以使用腾讯云提供的国内镜像源, 加速访问 Docker Hub</p>
<p>依次执行以下命令</p>
<pre><code>echo "OPTIONS='--registry-mirror=https://mirror.ccs.tencentyun.com'" &gt;&gt; /etc/sysconfig/docker
</code></pre><pre><code>systemctl daemon-reload
</code></pre><pre><code>service docker restart
</code></pre><h2 id="docker-">Docker 的简单操作</h2>
<blockquote>
<p>任务时间：10min ~ 20min</p>
</blockquote>
<h3 id="-">下载镜像</h3>
<p>下载一个官方的 CentOS 镜像到本地</p>
<pre><code>docker pull centos
</code></pre><p>下载好的镜像就会出现在镜像列表里</p>
<pre><code>docker images
</code></pre><h3 id="-">运行容器</h3>
<p>这时我们可以在刚才下载的 CentOS 镜像生成的容器内操作了。 </p>
<p>生成一个 centos 镜像为模板的容器并使用 bash shell</p>
<pre><code>docker run -it centos /bin/bash
</code></pre><p>这个时候可以看到命令行的前端已经变成了 [root@(一串 hash Id)] 的形式, 这说明我们已经成功进入了 CentOS 容器。</p>
<p>在容器内执行任意命令, 不会影响到宿主机, 如下</p>
<pre><code>mkdir -p /data/simple_docker
</code></pre><p>可以看到 /data 目录下已经创建成功了 simple_docker 文件夹</p>
<pre><code>ls /data
</code></pre><p>退出容器</p>
<pre><code>exit
</code></pre><p>查看宿主机的 /data 目录, 并没有 simple_docker 文件夹, 说明容器内的操作不会影响到宿主机</p>
<pre><code>ls /data
</code></pre><h3 id="-">保存容器</h3>
<p>查看所有的容器信息， 能获取容器的id</p>
<pre><code>docker ps -a
</code></pre><p>然后执行如下命令<sup>[<a href="#stage-2-step-3-icon">?</a>]</sup>，保存镜像：</p>
<pre><code>docker commit -m="备注" 你的CONTAINER_ID 你的IMAGE
</code></pre><p><a id="stage-2-step-3-icon"></a></p>
<blockquote>
<p>请自行将 -m 后面的信息改成自己的容器的信息</p>
</blockquote>
<h3 id="-">大功告成！</h3>
<p>恭喜你结束了 Docker 的教程并学会了 Docker 的一些基本操作, 接下来, 您可以购买并体验腾讯云提供的 Docker 服务</p>
</div>