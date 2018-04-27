<div class="lab-edi-doc"><h1 id="-node-js-">搭建 Node.js 环境</h1>
<h2 id="-node-js-">安装 Node.js 环境</h2>
<blockquote>
<p>任务时间：5min ~ 10min</p>
</blockquote>
<p>Node.js 是运行在服务端的 JavaScript, 是基于 Chrome JavaScript V8 引擎建立的平台。</p>
<h3 id="-node-js">下载并安装 Node.js</h3>
<p>下载最新的稳定版 v6.10.3 到本地</p>
<pre><code>wget https://nodejs.org/dist/v6.10.3/node-v6.10.3-linux-x64.tar.xz
</code></pre><p>下载完成后, 将其解压</p>
<pre><code>tar xvJf node-v6.10.3-linux-x64.tar.xz
</code></pre><p>将解压的 <code>Node.js</code> 目录移动到 <em>/usr/local</em> 目录下</p>
<pre><code>mv node-v6.10.3-linux-x64 /usr/local/node-v6
</code></pre><p>配置 <code>node</code> 软链接到 <em>/bin</em> 目录</p>
<pre><code>ln -s /usr/local/node-v6/bin/node /bin/node
</code></pre><h2 id="-npm">配置和使用 npm</h2>
<blockquote>
<p>任务时间：8min ~ 10min</p>
</blockquote>
<h3 id="-npm">配置 npm</h3>
<p><code>npm</code> 是 <code>Node.js</code> 的包管理和分发工具。它可以让 <code>Node.js</code> 开发者能够更加轻松的共享代码和共用代码片段</p>
<p>下载 <code>node</code> 的压缩包中已经包含了 <code>npm</code> , 我们只需要将其软链接到 <code>bin</code> 目录下即可</p>
<pre><code>ln -s /usr/local/node-v6/bin/npm /bin/npm
</code></pre><h3 id="-">配置环境变量</h3>
<p>将 <em>/usr/local/node-v6/bin</em> 目录添加到 $PATH 环境变量中可以方便地使用通过 npm 全局安装的第三方工具</p>
<pre><code>echo 'export PATH=/usr/local/node-v6/bin:$PATH' &gt;&gt; /etc/profile
</code></pre><p>生效环境变量</p>
<pre><code>source /etc/profile
</code></pre><h3 id="-npm">使用 npm</h3>
<p>通过 <code>npm</code> 安装进程管理模块 <code>forever</code></p>
<pre><code>npm install forever -g
</code></pre><h3 id="-">完成实验</h3>
<p>恭喜！您已经成功完成了搭建 Node.js 环境的实验任务。</p>
</div>