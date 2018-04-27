<div class="lab-edi-doc"><h1 id="express-">Express 入门</h1>
<h2 id="-nodejs">安装 NodeJS</h2>
<blockquote>
<p>任务时间：5min ~ 10min</p>
</blockquote>
<h3 id="-nodejs">安装 NodeJS</h3>
<p>在终端中，使用下面的命令安装 NodeJS：</p>
<pre><code>curl --silent --location https://rpm.nodesource.com/setup_8.x | sudo bash -
yum -y install nodejs
</code></pre><p>安装完成后，可使用下面的命令测试安装结果：</p>
<pre><code>node -v
</code></pre><h2 id="-express">安装 Express</h2>
<blockquote>
<p>任务时间：5min ~ 10min</p>
</blockquote>
<h3 id="-">创建工作目录</h3>
<p>使用下面的命令在服务器创建一个工作目录：</p>
<pre><code>mkdir -p /data/release/hello
</code></pre><p>进入此工作目录：</p>
<pre><code>cd /data/release/hello
</code></pre><h3 id="-">初始化项目</h3>
<p>通过 <code>npm init</code> 命令为你的应用创建一个 <code>package.json</code> 文件。欲了解 <code>package.json</code> 是如何起作用的，请参考 <em>Specifics of npm’s package.json handling</em>。</p>
<pre><code>npm init
</code></pre><p>此命令将要求你输入几个参数，例如此应用的名称和版本。 除 <code>entry point: (index.js)</code> 参数外，其他参数你可以直接按 “<strong>回车</strong>” 键接受默认设置即可。</p>
<p>对于 <code>entry point: (index.js)</code> 参数，键入 <code>app.js</code> 或者你所希望的名称，这是当前应用的入口文件；如果你希望采用默认的 index.js 文件名，只需按 “<strong>回车</strong>” 键即可。</p>
<h3 id="-express">安装 Express</h3>
<p>接下来安装 Express 并将其保存到依赖列表中：</p>
<pre><code>npm install express --save
</code></pre><p>如果只是临时安装 Express，不想将它添加到依赖列表中，只需略去 <code>--save</code> 参数即可 <sup>[<a href="#stage-2-step-3-icon">?</a>]</sup>：</p>
<pre><code>npm install express
</code></pre><p><a id="stage-2-step-3-icon"></a></p>
<blockquote>
<p>安装 Node 模块时，如果指定了 <code>--save</code> 参数，那么此模块将被添加到 <code>package.json</code> 文件中 <code>dependencies</code> 依赖列表中。 然后通过 <code>npm install</code> 命令即可自动安装依赖列表中所列出的所有模块。</p>
</blockquote>
<h2 id="hello-world">Hello world</h2>
<blockquote>
<p>任务时间：5min ~ 10min</p>
</blockquote>
<h3 id="-app-js">创建 app.js</h3>
<p>在 hello 目录中，<em>创建 app.js</em>，然后将下列代码复制进去：</p>
<h5 id="-data-release-hello-app-js">示例代码：/data/release/hello/app.js</h5>
<pre><code class="lang-javascript">var express = require('express');
var app = express();

app.get('/', function (req, res) {
  res.send('Hello World!');
});

var server = app.listen(3000, function () {
  console.log('Example app listening on port 3000!');
});
</code></pre>
<p>完成后，使用 <code>Ctrl + S</code> 保存文件。</p>
<p><sup>[<a href="#stage-3-step-1-icon">?</a>]</sup> 上面的代码启动一个服务并监听从 3000 端口进入的所有连接请求。他将对所有 (/) URL 或 <strong>路由</strong> 返回 “Hello World!” 字符串。对于其他所有路径全部返回 <strong>404 Not Found</strong> 。</p>
<p><a id="stage-3-step-1-icon"></a></p>
<blockquote>
<p>req (请求) 和 res (响应) 与 Node 提供的对象完全一致，因此，你可以调用 req.pipe()、req.on('data', callback) 以及任何 Node 提供的方法。</p>
</blockquote>
<h3 id="-">启动应用</h3>
<p>通过如下命令启动此应用：</p>
<pre><code>node app.js
</code></pre><p>然后在浏览器中打开 http://&lt;您的 CVM IP 地址&gt;:3000 并查看输出结果。</p>
<p>（如果访问不成功，可能是机器安全组禁用了 3000 端口所致，你可以前往<em>控制台</em>修改安全组配置。）</p>
<p>该步骤完成后，可使用 <code>Ctrl + C</code> 终止运行。</p>
<h2 id="express-">Express 应用生成器</h2>
<blockquote>
<p>任务时间：5min ~ 10min</p>
</blockquote>
<h3 id="-express-">安装 Express 应用生成器</h3>
<p>通过应用生成器工具 express 可以快速创建一个应用的骨架。</p>
<p>通过如下命令安装：</p>
<pre><code>npm install express-generator -g
</code></pre><p>-h 选项可以列出所有可用的命令行选项：</p>
<pre><code>express -h
</code></pre><p>将得到输出：</p>
<pre><code>  Usage: express [options] [dir]

  Options:

    -h, --help          output usage information
    -V, --version       output the version number
    -e, --ejs           add ejs engine support (defaults to jade)
        --hbs           add handlebars engine support
    -H, --hogan         add hogan.js engine support
    -c, --css &lt;engine&gt;  add stylesheet &lt;engine&gt; support (less|stylus|compass|sass) (defaults to plain css)
        --git           add .gitignore
    -f, --force         force on non-empty directory
</code></pre><h3 id="-">创建项目</h3>
<p>进入工作目录：</p>
<pre><code>cd /data/release
</code></pre><p>执行如下命令，在当前工作目录下创建一个命名为 <code>myapp</code> 的应用：</p>
<pre><code>express myapp
</code></pre><p>完成后，<em>点击查看 myapp 项目</em>。</p>
<p>生成的应用程序具有以下目录结构：</p>
<pre><code>.
├── app.js
├── bin
│   └── www
├── package.json
├── public
│   ├── images
│   ├── javascripts
│   └── stylesheets
│       └── style.css
├── routes
│   ├── index.js
│   └── users.js
└── views
    ├── error.pug
    ├── index.pug
    └── layout.pug

7 directories, 9 files
</code></pre><h3 id="-">启动应用</h3>
<p>进入该应用目录：</p>
<pre><code>cd myapp
</code></pre><p>然后安装所有依赖包：</p>
<pre><code>npm install
</code></pre><p>启动这个应用（MacOS 或 Linux 平台）<sup>[<a href="#stage-4-step-3-icon">?</a>]</sup>：</p>
<pre><code>DEBUG=myapp npm start
</code></pre><p>然后在浏览器中打开 http://&lt;您的 CVM IP 地址&gt;:3000 网址就可以看到这个应用了。</p>
<p>（该步骤完成后，可使用 <code>Ctrl + C</code> 终止运行。）</p>
<p><a id="stage-4-step-3-icon"></a></p>
<blockquote>
<p>Windows 平台使用如下命令：<code>set DEBUG=myapp &amp; npm start</code> </p>
</blockquote>
<h2 id="-">基本路由</h2>
<blockquote>
<p>任务时间：10min ~ 15min</p>
</blockquote>
<h3 id="express-">Express 路由简介</h3>
<p><strong>路由（Routing）</strong>是由一个 URI（或者叫路径）和一个特定的 HTTP 方法（GET、POST 等）组成的，涉及到应用如何响应客户端对某个网站节点的访问。<br></p>
<p>每一个路由都可以有一个或者多个处理器函数，当匹配到路由时，这些函数将被执行。<br></p>
<p>路由的定义由如下结构组成：</p>
<pre><code>app.METHOD(PATH, HANDLER)
</code></pre><p>其中：</p>
<ul>
<li><code>app</code> 是一个 express 实例；</li>
<li><code>METHOD</code> 是某个 <em>HTTP 请求方式</em> 中的一个</li>
<li><code>PATH</code> 是服务器端的路径；</li>
<li><code>HANDLER</code> 是当路由匹配到时需要执行的函数。</li>
</ul>
<h3 id="-express-">一个简单的 Express 路由</h3>
<h4 id="-hello-">修改 hello 项目</h4>
<p>返回开始创建的 <code>hello</code> 项目：</p>
<pre><code>cd /data/release/hello
</code></pre><p><em>编辑 app.js</em>，参考修改如下：</p>
<h5 id="-data-release-hello-app-js">示例代码：/data/release/hello/app.js</h5>
<pre><code class="lang-javascript">var express = require('express');
var app = express();

// 对网站首页的访问返回 "Hello World!" 字样
app.get('/', function (req, res) {
  res.send('Hello World!');
});

// 网站首页接受 POST 请求
app.post('/', function (req, res) {
  res.send('Got a POST request');
});

// /user 节点接受 PUT 请求
app.put('/user', function (req, res) {
  res.send('Got a PUT request at /user');
});

// /user 节点接受 DELETE 请求
app.delete('/user', function (req, res) {
  res.send('Got a DELETE request at /user');
});

var server = app.listen(3000, function () {
  console.log('Example app listening on port 3000!');
});
</code></pre>
<h4 id="-">启动应用</h4>
<pre><code>node app.js
</code></pre><p>（该步骤完成后，可使用 <code>Ctrl + C</code> 终止运行。）</p>
<h4 id="-">测试</h4>
<p>你可以使用 <code>curl</code> 命令或 <code>Postman</code> 等工具进行测试。</p>
<p>如在本地终端执行：</p>
<pre><code>curl -X POST http://&lt;您的 CVM IP 地址&gt;:3000
</code></pre><pre><code>curl -X PUT http://&lt;您的 CVM IP 地址&gt;:3000/user
</code></pre><pre><code>curl -X DELETE http://&lt;您的 CVM IP 地址&gt;:3000/user
</code></pre><h2 id="-">静态文件</h2>
<blockquote>
<p>任务时间：5min ~ 10min</p>
</blockquote>
<h3 id="-express-">利用 Express 托管静态文件</h3>
<p>通过 Express 内置的 <code>express.static</code> 可以方便地托管静态文件，例如图片、CSS、JavaScript 文件等。</p>
<h4 id="-">创建静态目录</h4>
<p>创建 public 目录：</p>
<pre><code>mkdir -p /data/release/hello/public
</code></pre><p>在 public 目录下，<em>创建 hello.html</em>，然后复制下列代码到 <strong>hello.html</strong> 中：</p>
<h5 id="-data-release-hello-public-hello-html">示例代码：/data/release/hello/public/hello.html</h5>
<pre><code class="lang-html">&lt;!DOCTYPE html&gt;
&lt;html lang="en"&gt;
&lt;head&gt;
  &lt;meta charset="UTF-8"&gt;
  &lt;title&gt;Document&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
  &lt;h1&gt;Hello World&lt;/h1&gt;
&lt;/body&gt;
&lt;/html&gt;
</code></pre>
<h4 id="-">修改应用</h4>
<p><em>编辑 app.js</em>，参考修改如下：</p>
<h5 id="-data-release-hello-app-js">示例代码：/data/release/hello/app.js</h5>
<pre><code class="lang-javascript">var express = require('express');
var app = express();

app.use(express.static('public'));

var server = app.listen(3000, function () {
  console.log('Example app listening on port 3000!');
});
</code></pre>
<p>我们在 <strong>app.js</strong> 中将静态资源文件所在的目录作为参数传递给 <code>express.static</code> 中间件，这样就可以提供静态资源文件的访问了。</p>
<h4 id="-">启动应用</h4>
<pre><code>node app.js
</code></pre><p>在浏览器中打开 http://&lt;您的 CVM IP 地址&gt;:3000/hello.html 网址就可以看到这个文件了。</p>
<p><br></p>
<p>你还可以将本地的文件通过<strong>拖拽</strong>至左边目录树的 <strong>public</strong> 目录上传文件来测试。</p>
<p>假设在 public 目录放置了图片、CSS 和 JavaScript 文件，你就可以从浏览器中访问：</p>
<pre><code>http://&lt;您的 CVM IP 地址&gt;:3000/images/kitten.jpg
http://&lt;您的 CVM IP 地址&gt;:3000/css/style.css
http://&lt;您的 CVM IP 地址&gt;:3000/js/app.js
http://&lt;您的 CVM IP 地址&gt;:3000/images/bg.png
http://&lt;您的 CVM IP 地址&gt;:3000/hello.html
</code></pre><h3 id="static-">static 中间件更多用法</h3>
<h4 id="-">多个目录</h4>
<p>如果你的静态资源存放在多个目录下面，你可以多次调用 <code>express.static</code> 中间件：</p>
<pre><code>app.use(express.static('public'));
app.use(express.static('files'));
</code></pre><p>访问静态资源文件时，<code>express.static</code> 中间件会根据目录添加的顺序查找所需的文件。</p>
<h4 id="-">指定路径</h4>
<p>如果你希望所有通过 <code>express.static</code> 访问的文件都存放在一个“虚拟（virtual）”目录（即目录根本不存在）下面，可以通过为静态资源目录指定一个挂载路径的方式来实现，如下所示：</p>
<p><em>编辑 app.js</em>，参考修改如下：</p>
<h5 id="-data-release-hello-app-js">示例代码：/data/release/hello/app.js</h5>
<pre><code class="lang-javascript">var express = require('express');
var app = express();

app.use('/static', express.static('public'));

var server = app.listen(3000, function () {
  console.log('Example app listening on port 3000!');
});
</code></pre>
<p>启动应用：</p>
<pre><code>node app.js
</code></pre><p>现在，你就可以通过带有 “/static” 前缀的地址来访问 public 目录下面的文件了。如：</p>
<p>http://&lt;您的 CVM IP 地址&gt;:3000/static/hello.html</p>
<h2 id="-">完成</h2>
<blockquote>
<p>任务时间：1min</p>
</blockquote>
<h3 id="-">完成实验</h3>
<p>恭喜！您已经完成了 Express 入门的全部实验内容！</p>
<p>更多 Express 相关资料，请查看 <em>Express 官网</em> 。</p>
</div>