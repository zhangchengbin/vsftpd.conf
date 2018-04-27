<div class="lab-edi-doc"><h1 id="-mono-">搭建 Mono 开发环境</h1>
<h2 id="-mono">安装 Mono</h2>
<blockquote>
<p>任务时间：时间未知</p>
</blockquote>
<h3 id="-">安装前的准备</h3>
<p>执行命令安装 yum-utils</p>
<pre><code>yum install yum-utils
</code></pre><p><sup>[<a href="#stage-1-step-1-rpm-import">执行命令添加安装包仓库</a>]</sup></p>
<pre><code>rpm --import "http://keyserver.ubuntu.com/pks/lookup?op=get&amp;search=0x3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF"
</code></pre><p>设置配置</p>
<pre><code>yum-config-manager --add-repo http://download.mono-project.com/repo/centos7/
</code></pre><p><a id="stage-1-step-1-rpm-import"></a></p>
<blockquote>
<p>可能由于网络原因，安装要耐心等待一段时间，大约 5~10min 。</p>
</blockquote>
<h3 id="-mono">安装 Mono</h3>
<p><em>执行命令安装 Mono</em>]</p>
<pre><code>yum -y install mono-complete
</code></pre><p><a id="stage-1-step-2-install-mono"></a></p>
<blockquote>
<p>可能由于网络原因，安装要耐心等待一段时间，大约 10~15min 。</p>
</blockquote>
<h2 id="-mono-">创建并运行 Mono 程序</h2>
<blockquote>
<p>任务时间：时间未知</p>
</blockquote>
<h3 id="-mono-">创建 Mono 程序</h3>
<p>执行命令创建程序文件</p>
<pre><code>cd /home
mkdir monohello
cd monohello/
touch HelloWorld.cs
</code></pre><p>修改 <em>HelloWorld.cs</em> 文件为如下内容</p>
<pre><code>using System;
public class HelloWorld
{
  public static void Main()
    {
        Console.WriteLine("Hello Mono World");
    }
}
</code></pre><h3 id="-mono-">运行 Mono 程序</h3>
<pre><code>mcs HelloWorld.cs
mono HelloWorld.exe
</code></pre><h3 id="-">大功告成！</h3>
<p>恭喜，您的 Mono 程序运行成功，搭建 Mono 开发环境，现在你可以继续进行深入的学习，更多的参考资料请查阅 <a target="_blank" href="http://www.mono-project.com/" title="null">http://www.mono-project.com/</a>。</p>
</div>