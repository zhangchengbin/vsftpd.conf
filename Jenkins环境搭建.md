<div class="lab-edi-doc"><h1 id="jenkins-">Jenkins 环境搭建</h1>
<h2 id="-jenkins">安装 Jenkins</h2>
<blockquote>
<p>任务时间：10min ~ 15min</p>
</blockquote>
<h3 id="jenkins-">Jenkins 简介</h3>
<p><strong><a target="_blank" href="https://jenkins.io/" title="null">Jenkins</a></strong> 是一个开源软件项目，是基于Java开发的一种持续集成工具，用于监控持续重复的工作，旨在提供一个开放易用的软件平台，使软件的持续集成变成可能。</p>
<h3 id="java-">Java 安装</h3>
<p>首先我们需要准备 Java 环境，使用下面命令来安装 Java：</p>
<pre><code>yum -y install java-1.8.0-openjdk-devel
</code></pre><h3 id="jenkins-">Jenkins 安装</h3>
<p>为了使用 Jenkins 仓库，我们要执行以下命令：</p>
<pre><code>sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
</code></pre><p>如果您以前从 Jenkins 导入过 key，那么 <code>rpm --import</code> 将失败，因为您已经有一个 key。请忽略，继续下面步骤。</p>
<h4 id="-">安装</h4>
<p>接着我们可以使用 <code>yum</code> 安装 Jenkins：</p>
<pre><code>yum -y install jenkins
</code></pre><h2 id="-jenkins">启动 Jenkins</h2>
<blockquote>
<p>任务时间：5min ~ 10min</p>
</blockquote>
<h3 id="-">启动</h3>
<p>启动 Jenkins 并设置为开机启动：</p>
<pre><code>systemctl start jenkins.service
chkconfig jenkins on
</code></pre><h3 id="-">测试访问</h3>
<p>Jenkins 默认运行在 8080端口。</p>
<p>稍等片刻，打开 <a target="_blank" href="http://&lt;您的 CVM IP 地址&gt;:8080" title="null">http://&lt;您的 CVM IP 地址&gt;:8080</a> 测试访问。</p>
<h2 id="-jenkins">进入 Jenkins</h2>
<blockquote>
<p>任务时间：5min ~ 10min</p>
</blockquote>
<h3 id="-">管理员密码</h3>
<p>登入 Jenkins 需要输入管理员密码，按照提示，我们使用以下命令查看初始密码：</p>
<pre><code>cat /var/lib/jenkins/secrets/initialAdminPassword
</code></pre><p>复制密码，填入，进入 Jenkins。</p>
<h3 id="-jenkins">定制 Jenkins</h3>
<p>我们选择默认的 <code>install suggested plugins</code> 来安装插件。</p>
<h3 id="-">创建用户</h3>
<p>请填入相应信息创建用户，然后即可登入 Jenkins 的世界。</p>
<h3 id="-">实验完成</h3>
<p>恭喜，您已完成 <strong>搭建 Jenkins 环境搭建</strong> 实验。</p>
<p>有关 <strong>Jenkins</strong> 的使用实践，请继续关注后续实验。 </p>
</div>