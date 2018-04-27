<div class="lab-edi-doc"><h1 id="-thinkjs-">搭建 Thinkjs 开发环境</h1>
<h2 id="-node-js">安装 Node.js</h2>
<blockquote>
<p>任务时间：5min ~ 10min</p>
</blockquote>
<p>ThinkJS 是一款 Node.js 的 MVC 框架，所以安装 ThinkJS 之前需要先安装 Node.js 环境。</p>
<h3 id="-node-js-6-x">安装 Node.js 6.x</h3>
<p>ThinkJS 支持 Node.js 的 0.12 以上版本，本教程以 Node.js 6.x 为例，其他版本安装过程相似</p>
<pre><code>curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
</code></pre><h2 id="-thinkjs">安装 ThinkJS</h2>
<blockquote>
<p>任务时间：15min ~ 20min</p>
</blockquote>
<h3 id="npm-thinkjs-">npm 安装 ThinkJS：</h3>
<p>执行如下命令<sup>[<a href="#stage-2-step-1-install-ThinkJS">安装 ThinkJS</a>]</sup>：</p>
<pre><code>sudo npm install thinkjs@2 -g --verbose
</code></pre><p><a id="stage-2-step-1-install-ThinkJS"></a></p>
<blockquote>
<p>如果安装过程很慢，可以执行命令 <code>sudo npm install thinkjs@2 -g --registry=http://mirrors.tencentyun.com/npm/ --verbose</code> 使用腾讯云的源进行安装；如果安装过 ThinkJS 1.x 版本，需要通过 <code>sudo npm uninstall -g thinkjs-cmd</code> 命令删除原有版本。</p>
</blockquote>
<h3 id="-">创建项目</h3>
<p>执行如下命令<sup>[<a href="#stage-2-step-2-create-project">创建项目</a>]</sup>:</p>
<pre><code>thinkjs new project_path;
</code></pre><p>创建成功将看到提示如截图所示：
<img src="https://mc.qcloudimg.com/static/img/b005085483a59debdd55317404fc98f5/1.png" alt=""></p>
<p><a id="stage-2-step-2-create-project"></a></p>
<blockquote>
<p>project_path 是创建项目目录名，可以使用自定义名字</p>
</blockquote>
<h3 id="-">安装项目依赖</h3>
<p>执行如下命令<sup>[<a href="#stage-2-step-3-install-dependence">安装依赖</a>]</sup>：</p>
<pre><code>cd project_path
npm install --verbose
</code></pre><p><a id="stage-2-step-3-install-dependence"></a></p>
<blockquote>
<p>为了提升速度，推荐使用腾讯云源进行安装，若自定义了项目目录，请将执行命令中的 <code>project_path</code> 替换为自定义的名字</p>
</blockquote>
<h3 id="-">启动项目</h3>
<p>执行如下命令启动项目</p>
<pre><code>npm start
</code></pre><p>启动成功将看到提示如截图所示：
<img src="https://mc.qcloudimg.com/static/img/a77bcab822ff00f725a79b0e35c91bc2/1.png" alt=""></p>
<h3 id="-">大功告成！</h3>
<p>恭喜，ThinkJS 已经启动成功，使用浏览器直接访问 http://&lt;您的 CVM IP 地址&gt;:8360/ 。</p>
</div>