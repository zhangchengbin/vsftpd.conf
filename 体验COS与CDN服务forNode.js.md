<div class="lab-edi-doc"><h1 id="-cos-cdn-for-node-js">体验 COS 与 CDN 服务 for Node.js</h1>
<h2 id="cdn-">CDN 服务申请</h2>
<blockquote>
<p>任务时间：30min ~ 60min</p>
</blockquote>
<h3 id="-">实名认证与申请服务</h3>
<p>腾讯云提供了 <a target="_blank" href="https://www.qcloud.com/product/cdn?fromSource=lab" title="null">CDN 的加速服务</a>, 但使用服务前您需要实名认证, 点击下面的视频了解如何申请该服务</p>
<p><em>如何申请腾讯云的 CDN 加速服务</em></p>
<p>如果您已经实名认证，可以直接跳过这一步</p>
<h2 id="-cos-">使用 COS 服务</h2>
<blockquote>
<p>任务时间：30min ~ 60min</p>
</blockquote>
<h3 id="-cos-">开通 COS 并上传文件</h3>
<p>CDN 开通后, 我们既可以使用 CDN 全站加速的功能, 也可以使用 CDN 部署资源文件的功能，本实验使用 COS/CDN 超出免费配额可能产生小额费用。</p>
<p>在使用 CDN 部署资源文件的功能前, 我们需要开通腾讯云的 COS 服务, 点击下面的视频了解如何申请该服务</p>
<p><em>如何申请和使用腾讯云的 COS 服务</em></p>
<h3 id="cos-cdn-">COS 文件地址与 CDN 加速地址</h3>
<p>文件通过腾讯云官网控制台web页面上传后, 通过查看上传文件信息可以得到形如 <code>Bucket名称-id.cos地区缩写.myqcloud.com/文件名</code> 的 URL 地址, 将该地址中的 <code>cos地区缩写</code> 改为 <code>file</code> 即可得到 CDN 的加速地址. </p>
<p>使用该加速地址来访问的资源文件可以得到更快的响应, 达到加速的目的. 如: </p>
<p>文件地址:</p>
<pre><code>http://cloud-1251435248.costj.myqcloud.com/hello.txt
</code></pre><p>对应的 CDN 地址:</p>
<pre><code>http://cloud-1251435248.file.myqcloud.com/hello.txt
</code></pre><p>cos地址与对应园区的关系</p>
<ul>
<li><code>costj</code> -- 华北(天津园区)</li>
<li><code>cossh</code> -- 华东(上海园区)</li>
<li><code>cosgz</code> -- 华南(广州园区)</li>
<li><code>cossgp</code> -- 新加坡园区</li>
</ul>
<h2 id="-cos-sdk-for-node-js">使用 COS SDK for Node.js</h2>
<blockquote>
<p>任务时间：30min ~ 60min</p>
</blockquote>
<h3 id="-">准备工作</h3>
<p>上面介绍了使用浏览器上传文件到 COS 并得到 CDN 地址的方式, 下面介绍如何通过 SDK 的方式使用 COS. 在这之前, 我们需要做一些准备工作</p>
<h4 id="-">创建相关目录</h4>
<pre><code>mkdir -p /data/upload
</code></pre><h4 id="-">准备相关的文件</h4>
<p>执行下面的命令, 在 <em>/data</em> 目录下创建一个名叫<code>upload_example.txt</code>的文件</p>
<pre><code>echo 'Hello World' &gt; /data/upload/upload_example.txt
</code></pre><h4 id="-appid-secret_id-secret_key-">查看 appid, secret_id 和 secret_key 信息</h4>
<ul>
<li>如果您 Bucket 的默认域名中包含125开头的 APPID，请使用“API密钥”， <em>点击前往</em></li>
<li>如果您 Bucket 的默认域名中包含100开头的项目 ID，请使用“项目密钥”， <em>点击前往</em></li>
</ul>
<h4 id="-bucket-">查看 bucket 属于哪个园区</h4>
<ul>
<li>打开 <em>bucket列表</em></li>
<li>在列表中点击将要上传的 bucket, 进入详情页</li>
<li>选择 <code>基础配置</code> tab</li>
<li>查看 <code>基本信息</code> - <code>所属地区</code></li>
<li>华北地区对应 <code>tj</code>, 华南地区对应 <code>gz</code>, 华东地区对应 <code>sh</code></li>
</ul>
<h4 id="-node-js-">安装 Node.js 环境</h4>
<pre><code>curl --silent --location https://rpm.nodesource.com/setup_8.x | sudo bash -
yum install -y nodejs npm
</code></pre><h4 id="-npm-cos-node-js-sdk">使用 npm 安装 COS 的 Node.js SDK</h4>
<pre><code>cd /data &amp;&amp; npm i cos-nodejs-sdk-v5 --save
</code></pre><p>如果下载速度较慢, 可以考虑更换 npm 源来安装</p>
<pre><code>cd /data &amp;&amp; npm i cos-nodejs-sdk-v5 --save --registry=https://registry.npm.taobao.org
</code></pre><h3 id="-sdk-cos">使用 SDK 上传文件到 COS</h3>
<p>在 <em>/data</em> 目录下创建 cos_nodejs_upload.js 文件, 并编辑内容如下</p>
<h5 id="-data-cos_nodejs_upload-js">示例代码：/data/cos_nodejs_upload.js</h5>
<pre><code class="lang-javascript">var COS = require('cos-nodejs-sdk-v5');
var cos = new COS({
    AppId: '',     // 替换为你的appid
    SecretId: '',  // 替换为你的SecretId
    SecretKey: '', // 替换为你的SecretKey
});

cos.sliceUploadFile({
    Bucket: '', // 替换为你的Bucket名称
    Region: 'cn-north', // 设置COS所在的区域，对应关系: 华南-&gt;cn-south, 华东-&gt;cn-east, 华北-&gt;cn-north
    Key: 'upload_example_nodejs.txt', // 设置上传到cos后的文件的名称
    FilePath: './upload/upload_example.txt' // 设置要上传的本地文件
}, function (err, data) {
    if (!err) {
        console.log(data);
    } else {
        console.log(err);
    }
});
</code></pre>
<p>将你的 Appid, SecretId, SecretKey, Bucket名称, COS所在的区域, 要上传的本地文件, 上传到cos后的文件的名称 填入后执行</p>
<pre><code>cd /data &amp;&amp; node cos_nodejs_upload.js
</code></pre><p>如果调用成功, 会返回类似如下结构的信息:</p>
<pre><code>---------------- upload file -----------------
file name : upload_example_nodejs.txt
Async uploading...-----  1
---------------- upload complete -----------------
{ Location: 'detect-1253675457.cn-east.myqcloud.com/upload_example_nodejs.txt',
  Bucket: 'detect',
  Key: 'upload_example_nodejs.txt',
  ETag: '"d9883e94617e5c7d60f115710b4ef7cd"' }
</code></pre><p>在 <em>bucket列表</em> 中选择你刚才填入的 bucket 并查看文件列表, 会发现新增了 <code>upload_sample_nodejs.txt</code> 文件, 证明确实已经成功上传到 COS</p>
<p>如果调用失败, 会返回类似如下的信息:</p>
<pre><code>{ 
    statusCode: 404,
    error: { 
        Code: 'NoSuchBucket',
        Message: 'The specified bucket does not exist.',
        Resource: 'deect-1253675457.cn-east.myqcloud.com/upload_example_nodejs.txt',
        RequestId: 'NTkyZmQ0MjdfOTJhMDY4XzUwZGNfODY0ZjY=',
        TraceId: 'OGVmYzZiMmQzYjA2OWNhODk0NTRkMTBiOWVmMDAxODc0OWRkZjk0ZDM1NmI1M2E2MTRlY2MzZDhmNmI5MWI1OTY4OGQ5OWY4YWFhNjAzOTkyNDJhZmQyOTk1YWVmOW
FlOTBkODY2ZjFjYTkwZDU4M2YyYmQ4MWMwNzhkOGY0ZTE=' 
    } 
}
</code></pre><p>结合 <em>cos错误码说明</em> 和<code>message</code>字段, 您可以知道发生错误的原因</p>
<h3 id="-sdk-cos-">使用 SDK 删除 COS 文件</h3>
<p>COS 提供通过 SDK 删除文件的能力</p>
<p>在 <em>/data</em> 目录下创建 cos_nodejs_delete.js 文件, 并编辑内容如下</p>
<h5 id="-data-cos_nodejs_delete-js">示例代码：/data/cos_nodejs_delete.js</h5>
<pre><code class="lang-javascript">var COS = require('cos-nodejs-sdk-v5');
var cos = new COS({
    AppId: '',     // 替换为你的appid
    SecretId: '',  // 替换为你的SecretId
    SecretKey: '', // 替换为你的SecretKey
});

cos.deleteObject({
    Bucket: '', // 替换为你的Bucket名称
    Region: 'cn-north', // 设置COS所在的区域，对应关系: 华南-&gt;cn-south, 华东-&gt;cn-east, 华北-&gt;cn-north
    Key: 'upload_example_nodejs.txt', // 替换为要删除的cos文件的名称
}, function(err, data) {
    if (err) {
        console.log(err);
    } else {
        console.log(data);
    }
});
</code></pre>
<p>将你的 Appid, SecretId, SecretKey, Bucket名称, COS所在的区域, 要删除的cos文件的名称 填入后执行</p>
<pre><code>cd /data &amp;&amp; node /data/cos_nodejs_delete.js
</code></pre><p>如果调用成功, 会返回类似如下结构的信息:</p>
<pre><code>{ DeleteObjectSuccess: true }
</code></pre><p>在 <em>bucket列表</em> 中选择你刚才填入的 bucket 并查看文件列表, 会发现<code>upload_sample_nodejs.txt</code>文件已经不存在了, 证明文件删除成功</p>
<h3 id="-sdk-bucket-object">使用 SDK 列出 bucket 下的所有 Object</h3>
<p>在 <em>/data</em> 目录下创建 cos_nodejs_list.js 文件, 并编辑内容如下</p>
<h5 id="-data-cos_nodejs_list-js">示例代码：/data/cos_nodejs_list.js</h5>
<pre><code class="lang-javascript">var COS = require('cos-nodejs-sdk-v5');
var cos = new COS({
    AppId: '',     // 替换为你的appid
    SecretId: '',  // 替换为你的SecretId
    SecretKey: '', // 替换为你的SecretKey
});

cos.getBucket({
    Bucket: '', // 替换为你的Bucket名称
    Region: 'cn-north', // 设置COS所在的区域，对应关系: 华南-&gt;cn-south, 华东-&gt;cn-east, 华北-&gt;cn-north
}, function(err, data) {
    if(err) {
        console.log(err);
    } else {
        console.log(data);
    }
});
</code></pre>
<p>将你的 Appid, SecretId, SecretKey, Bucket名称, COS所在的区域 填入后执行</p>
<pre><code>cd /data &amp;&amp; node /data/cos_nodejs_list.js
</code></pre><p>如果调用成功, 会返回类似如下结构的信息:</p>
<pre><code>{
    Name: 'detect',
    Prefix: '',
    Marker: '',
    MaxKeys: '1000',
    IsTruncated: 'false',
    Contents: [
        {
            Key: 'sample_file_move.txt',
            LastModified: '2017-06-01T06: 56: 05.000Z',
            ETag: '"648a6a6ffffdaa0badb23b8baf90b6168dd16b3a"',
            Size: '12',
            Owner: [Object],
            StorageClass: 'Standard'
        },
        {
            Key: 'sample_folder/',
            LastModified: '2017-06-01T07: 05: 04.000Z',
            ETag: '""',
            Size: '0',
            Owner: [Object],
            StorageClass: 'Standard'
        },
        {
            Key: 'sample_folder_php/',
            LastModified: '2017-06-01T08: 08: 10.000Z',
            ETag: '""',
            Size: '0',
            Owner: [Object],
            StorageClass: 'Standard'
        },
        {
            Key: 'upload_sample_php_move.txt',
            LastModified: '2017-06-01T08: 02: 17.000Z',
            ETag: '"648a6a6ffffdaa0badb23b8baf90b6168dd16b3a"',
            Size: '12',
            Owner: [Object],
            StorageClass: 'Standard'
        }
    ],
    CommonPrefixes: []
}
</code></pre><p>点击查看 <em>SDK for Node.js 文档</em> 可以了解详细的返回字段对应信息</p>
<h3 id="-">实验完成</h3>
<p>恭喜您已经完成了 CDN 与 COS for Node.js 的学习</p>
</div>