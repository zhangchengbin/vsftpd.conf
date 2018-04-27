<div class="lab-edi-doc"><h1 id="-windows-php-iis-">基于 Windows 搭建 PHP 开发环境（IIS）</h1>
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
<h2 id="-php">安装 PHP</h2>
<blockquote>
<p>任务时间：15min ~ 20min</p>
</blockquote>
<h3 id="-">安装依赖</h3>
<p>为了能正常运行 PHP，我们需要安装其依赖的运行库。</p>
<p>复制下面链接到浏览器<sup>[<a href="#stage-2-step-1-BUBBLE_LABEL1">?</a>]</sup>：</p>
<pre><code>https://www.microsoft.com/zh-CN/download/details.aspx?id=48145
</code></pre><p>点击下载，选择 <strong>x64</strong> 版本下载：</p>
<p><img src="https://share-10039692.file.myqcloud.com/lab/ac197229e6/image/1j02h6ptv4/%E5%9B%BE%E7%89%87.png" alt="image"></p>
<p>如弹出阻止框，请点击<strong>允许</strong>：</p>
<p><img src="https://share-10039692.file.myqcloud.com/lab/9800988793/image/71ni6fdojy/image.png" alt="image"></p>
<p>下载后，运行安装该文件：</p>
<p><img src="https://share-10039692.file.myqcloud.com/lab/ac197229e6/image/8bthfo4198/%E5%9B%BE%E7%89%87.png" alt="image"></p>
<p><a id="stage-2-step-1-BUBBLE_LABEL1"></a></p>
<blockquote>
<p>开始菜单中选择 IE 浏览器</p>
</blockquote>
<h3 id="-">下载安装包</h3>
<p>复制下面链接到浏览器:</p>
<pre><code>http://windows.PHP.net/download
</code></pre><p>选择 <strong>x64 Non Thread Safe</strong> 版本下载：</p>
<p><img src="https://share-10039692.file.myqcloud.com/lab/ac197229e6/image/ystij4uhub/%E5%9B%BE%E7%89%87.png" alt="image"></p>
<h3 id="-">解压、移动</h3>
<p><strong>解压</strong>下载的压缩包：</p>
<p><img src="https://share-10039692.file.myqcloud.com/lab/ac197229e6/image/qiabqsovu0/%E5%9B%BE%E7%89%87.png" alt="image"></p>
<p>将解压后的文件夹移动至 C 盘，然后重命名为 <strong>php</strong>：</p>
<p><img src="https://share-10039692.file.myqcloud.com/lab/ac197229e6/image/02esvl87kc/%E5%9B%BE%E7%89%87.png" alt="image"></p>
<h2 id="-iis">配置 IIS</h2>
<blockquote>
<p>任务时间：25min ~ 30min</p>
</blockquote>
<h3 id="-iis">安装 IIS</h3>
<p>打开<strong>服务器管理器</strong>：</p>
<p><img src="https://share-10039692.file.myqcloud.com/lab/ac197229e6/image/nvtwlvxaie/%E5%9B%BE%E7%89%87.png" alt="image"></p>
<p>一路选择默认值，在 <strong>『服务器角色』</strong> 中勾选 <strong>Web 服务器(IIS)</strong>：</p>
<p><img src="https://share-10039692.file.myqcloud.com/lab/ac197229e6/image/yqsapu5f29/%E5%9B%BE%E7%89%87.png" alt="image"></p>
<p>在 <strong>『角色服务』</strong> 中的 <strong>应用程序开发</strong> 中勾选 <strong>CGI</strong>：</p>
<p><img src="https://share-10039692.file.myqcloud.com/lab/ac197229e6/image/7onkes1b9m/%E5%9B%BE%E7%89%87.png" alt="image"></p>
<p>点击下一步、安装，等待安装完成：</p>
<p><img src="https://share-10039692.file.myqcloud.com/lab/ac197229e6/image/g22suhddvj/%E5%9B%BE%E7%89%87.png" alt="image"></p>
<h3 id="-">测试访问</h3>
<p>安装完成后，打开浏览器，访问：</p>
<pre><code>http://localhost
</code></pre><p>即可看到 IIS 欢迎页面。</p>
<p>你也可以访问 http://&lt;您的 CVM IP 地址&gt; 在外网查看。</p>
<p><img src="https://share-10039692.file.myqcloud.com/lab/ac197229e6/image/qhjqz005lq/%E5%9B%BE%E7%89%87.png" alt="image"></p>
<h3 id="iis-">IIS - 添加模块映射</h3>
<p>在开始菜单中，找到 <strong>IIS 管理器</strong>，打开它：</p>
<p><img src="https://share-10039692.file.myqcloud.com/lab/ac197229e6/image/3a1c59oiro/%E5%9B%BE%E7%89%87.png" alt="image"></p>
<p><img src="https://share-10039692.file.myqcloud.com/lab/ac197229e6/image/jgyiyty4bs/%E5%9B%BE%E7%89%87.png" alt="image"></p>
<p>点击左侧默认生成的服务器，然后双击面板中 <strong>『处理程序映射』</strong>：</p>
<p><img src="https://share-10039692.file.myqcloud.com/lab/ac197229e6/image/ikott2usqn/%E5%9B%BE%E7%89%87.png" alt="image"></p>
<p>点击面板右侧的 <strong>添加模块映射</strong>：</p>
<p><img src="https://share-10039692.file.myqcloud.com/lab/ac197229e6/image/3bka0o97fg/%E5%9B%BE%E7%89%87.png" alt="image"></p>
<p>按下图填入、选择相应信息，确认添加：</p>
<p><img src="https://share-10039692.file.myqcloud.com/lab/ac197229e6/image/u0wiqf32cm/%E5%9B%BE%E7%89%87.png" alt="image"></p>
<p>（其中选择 <strong>可执行文件</strong> 时，注意更改右下角文件类型为 <strong>.exe</strong>）
<img src="https://share-10039692.file.myqcloud.com/lab/ac197229e6/image/o022wkolno/%E5%9B%BE%E7%89%87.png" alt="image"></p>
<h3 id="iis-">IIS - 默认文档</h3>
<p>双击面板中 <strong>『默认文档』</strong>：</p>
<p><img src="https://share-10039692.file.myqcloud.com/lab/ac197229e6/image/zg2ylhsu05/%E5%9B%BE%E7%89%87.png" alt="image"></p>
<p>点击右上角 <strong>添加</strong>，填入 <code>index.php</code>：</p>
<p><img src="https://share-10039692.file.myqcloud.com/lab/ac197229e6/image/0gvgtuzqbl/%E5%9B%BE%E7%89%87.png" alt="image"></p>
<h3 id="-index-php">创建 index.php</h3>
<p>转至『编辑视图』，点击目录树上方<strong>刷新</strong>图标，然后展开 <em>inetpub\wwwroot</em>。</p>
<p>右键点击 <strong>wwwroot</strong>，新建文件，命名为 <code>index.php</code>，然后点击打开该文件。</p>
<p>填入内容：</p>
<pre><code>&lt;?php 
    phpinfo();
?&gt;
</code></pre><p>最后 <code>Ctrl + S</code> 保存。</p>
<p><img src="https://share-10039692.file.myqcloud.com/lab/ac197229e6/image/dhfbtzvhyu/%E5%9B%BE%E7%89%87.png" alt="image"></p>
<h3 id="-">测试连接</h3>
<p>在浏览器访问该地址:</p>
<pre><code>http://localhost
</code></pre><p>可看到刚刚添加的 <code>index.php</code> 页内容。</p>
<p>你也可以访问 http://&lt;您的 CVM IP 地址&gt; 在外网查看。</p>
<p><img src="https://share-10039692.file.myqcloud.com/lab/ac197229e6/image/rqv5tmxae3/%E5%9B%BE%E7%89%87.png" alt="image"></p>
<h3 id="-">完成</h3>
<p>恭喜，您已完成 <strong>搭建 PHP 开发环境</strong> 实验！</p>
</div>