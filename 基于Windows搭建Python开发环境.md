<div class="lab-edi-doc"><h1 id="-windows-python-">基于 Windows 搭建 Python 开发环境</h1>
<h2 id="-">准备</h2>
<blockquote>
<p>任务时间：5min ~ 10min</p>
</blockquote>
<h3 id="-telnet-">开启 Telnet 服务</h3>
<p>实验室的『编辑视图』，文件浏览器及教程步骤检测等依赖于 Telnet 服务，所以需要您在『远程桌面』中添加并开启 Telnet 服务。</p>
<p>具体操作可参考该视频：</p>
<ul>
<li><em>添加并开启 Telnet 服务演示</em></li>
</ul>
<h2 id="-python">安装 Python</h2>
<blockquote>
<p>任务时间：15min ~ 20min</p>
</blockquote>
<p>Python 是一种解释型、面向对象、动态数据类型的高级程序设计语言。</p>
<h3 id="-">下载安装包</h3>
<p>复制下面链接到浏览器<sup>[<a href="#stage-2-step-1-BUBBLE_LABEL1">?</a>]</sup>：</p>
<pre><code>https://www.python.org/downloads
</code></pre><p>在 <strong>Downloads</strong> 下选择 <strong>Python 2</strong> 下载：</p>
<p><img src="https://share-10039692.file.myqcloud.com/lab/ac197229e6/image/a5yek1bfo3/%E5%9B%BE%E7%89%87.png" alt="Python 下载"></p>
<p><a id="stage-2-step-1-BUBBLE_LABEL1"></a></p>
<blockquote>
<p>开始菜单中选择 IE 浏览器</p>
</blockquote>
<h3 id="-python">安装 Python</h3>
<p>下载好安装包后，一路 “Next” 点击安装：</p>
<p><img src="https://share-10039692.file.myqcloud.com/lab/ac197229e6/image/bskg9k0s6q/%E5%9B%BE%E7%89%87.png" alt="Python 安装"></p>
<h3 id="-">配置环境变量</h3>
<p>在 <strong>开始菜单</strong> 中右击 <strong>这台电脑</strong>，选择 <strong>属性（Properties）</strong>：</p>
<p><img src="https://share-10039692.file.myqcloud.com/lab/ac197229e6/image/wi295oxrzs/%E5%9B%BE%E7%89%87.png" alt="Properties"></p>
<p>在弹出的窗口中点击 <strong>高级系统设置</strong>：</p>
<p><img src="https://share-10039692.file.myqcloud.com/lab/ac197229e6/image/yn4vqgv5uy/%E5%9B%BE%E7%89%87.png" alt="高级系统设置"></p>
<p>然后点击 <strong>环境变量</strong>：</p>
<p><img src="https://share-10039692.file.myqcloud.com/lab/ac197229e6/image/x118fv0ghr/%E5%9B%BE%E7%89%87.png" alt="环境变量"></p>
<p>在其中找到 <strong>Path</strong>，双击编辑：</p>
<p><img src="https://share-10039692.file.myqcloud.com/lab/ac197229e6/image/afso3dxkxu/image.png" alt="Path"></p>
<p>在<strong>变量值</strong>最后补充一下路径（注意分号），然后确定保存：</p>
<pre><code>;C:\Python27;C:\Python27\Scripts
</code></pre><p><img src="https://share-10039692.file.myqcloud.com/lab/ac197229e6/image/p4dp59jwab/image.png" alt="变量值"></p>
<h3 id="-">测试</h3>
<p>打开 <strong>Powershell</strong> 或 <strong>命令提示符（CMD）</strong>：</p>
<p><img src="https://share-10039692.file.myqcloud.com/lab/ac197229e6/image/uvwosrs9vn/image.png" alt="Powershell"></p>
<p>输入：</p>
<pre><code>python
</code></pre><p><img src="https://share-10039692.file.myqcloud.com/lab/ac197229e6/image/enmn5u09hs/image.png" alt="Python"></p>
<p>启动 Python 解释器，然后输入：</p>
<pre><code>print('Hello World')
</code></pre><p>回车执行，可看到输出内容，最后输入：</p>
<pre><code>exit()
</code></pre><p>退出解释器。</p>
<p><img src="https://share-10039692.file.myqcloud.com/lab/ac197229e6/image/5ynulhczpx/image.png" alt="Python"></p>
<h2 id="-pip">安装 pip</h2>
<blockquote>
<p>任务时间：10min ~ 15min</p>
</blockquote>
<h3 id="-easy_install">使用 easy_install</h3>
<p><strong>easy_install</strong> 是用来下载安装 Python 公共资源库相关资源包的一个工具，其位于 <code>C:\Python27\Scripts</code> 目录，刚刚我们已经把该目录添加至环境变量，所以我们现在可以直接使用。</p>
<p>打开 <strong>Powershell</strong> 或 <strong>命令提示符（CMD）</strong>，输入：</p>
<pre><code>easy_install pip
</code></pre><p>稍等，即可看到安装完成。</p>
<p><img src="https://share-10039692.file.myqcloud.com/lab/ac197229e6/image/onvgl9xcqq/image.png" alt="easy_install"></p>
<h3 id="-pip">使用 pip</h3>
<p><strong>pip</strong> 是 <strong>easy_install</strong> 的改进版，提供了更完善的包管理功能。</p>
<p>我们来使用 <strong>pip</strong> 安装一个 <strong>requests</strong> 库来测试使用，继续输入：</p>
<pre><code>pip install requests
</code></pre><p><img src="https://share-10039692.file.myqcloud.com/lab/ac197229e6/image/j5fxyp33g4/image.png" alt="pip"></p>
<p>接着输入：</p>
<pre><code>pip list
</code></pre><p>可以查看已安装的库，更多命令，可以输入 <code>pip -h</code> 查看。</p>
<h3 id="-">完成</h3>
<p>恭喜，您已完成 <strong>搭建 Python 2.7 开发环境</strong> 实验！</p>
</div>