<div class="lab-edi-doc"><h1 id="-cos-cdn-for-php">体验 COS 与 CDN 服务 for PHP</h1>
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
<h2 id="-cos-sdk-for-php">使用 COS SDK for PHP</h2>
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
</code></pre><h4 id="-appid-secret_id-secret_key-">查看appid, secret_id 和 secret_key 信息</h4>
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
<h4 id="-git-php-">安装 Git 与 PHP 环境</h4>
<pre><code>yum install -y git php php-common php-devel
</code></pre><h4 id="-cos-php-sdk">安装 COS 的 PHP SDK</h4>
<pre><code>git clone https://github.com/tencentyun/cos-php-sdk-v4 /data/cos-php-sdk-v4
</code></pre><h3 id="-sdk-">修改 SDK 配置</h3>
<p>修改 <em>/data/cos-php-sdk-v4/qcloudcos/conf.php</em> 内的 APPID、SECRET_ID、SECRET_KEY 为您的配置。</p>
<h3 id="-sdk-cos">使用 SDK 上传文件到 COS</h3>
<p>在 <em>/data</em> 目录下创建 cos_php_upload.php 文件, 并编辑内容如下</p>
<h5 id="-data-cos_php_upload-php">示例代码：/data/cos_php_upload.php</h5>
<pre><code class="lang-php">&lt;?php
// 包含cos-php-sdk-v4/include.php文件
require('./cos-php-sdk-v4/include.php');
use qcloudcos\Cosapi;

// 设置COS所在的区域，对应关系如下：
//     华南  -&gt; gz
//     华中  -&gt; sh
//     华北  -&gt; tj
Cosapi::setRegion('');

$bucket = '';                            // 填写你的bucket名
$src = './upload/upload_example.txt';    // 填写要上传到 cos 的文件(含地址), 如 ./upload/upload_example.txt
$dst = 'upload_sample_php.txt';          // 填写上传到 cos 后的文件名称, 如 upload_sample_php.txt

// 上传文件
$ret = Cosapi::upload($bucket, $src, $dst);
var_dump($ret);
</code></pre>
<p>将你的 bucket 所属的地区码, bucket 名, 需要上传的文件, 上传后的文件名 依次填入后执行</p>
<pre><code>cd /data &amp;&amp; php /data/cos_php_upload.php
</code></pre><p>如果调用成功, 会返回类似如下结构的信息:</p>
<pre><code>array(4) {
  ["code"]=&gt;
  int(0)
  ["message"]=&gt;
  string(7) "SUCCESS"
  ["request_id"]=&gt;
  string(36) "NTkyZmM2NzFfOGVhMDY4XzgzMmVfODU1Yjc="
  ["data"]=&gt;
  array(5) {
    ["access_url"]=&gt;
    string(64) "http://detect-1253675457.file.myqcloud.com/upload_sample_php.txt"
    ["resource_path"]=&gt;
    string(40) "/1253675457/detect/upload_sample_php.txt"
    ["source_url"]=&gt;
    string(65) "http://detect-1253675457.cossh.myqcloud.com/upload_sample_php.txt"
    ["url"]=&gt;
    string(76) "http://sh.file.myqcloud.com/files/v2/1253675457/detect/upload_sample_php.txt"
    ["vid"]=&gt;
    string(42) "23c8c295f62f8639cd4dc5cda26b1b4b1496303217"
  }
}
</code></pre><p><em>点击这里</em> 了解返回的各个字段的含义</p>
<p>在 <em>bucket列表</em> 中选择你刚才填入的 bucket 并查看文件列表, 会发现新增了 <code>upload_sample_php.txt</code> 文件, 证明确实已经成功上传到 COS</p>
<p>如果调用失败, 会返回类似如下的信息:</p>
<pre><code>array(3) {
  ["code"]=&gt;
  int(-62)
  ["message"]=&gt;
  string(22) "ERROR_PROXY_AUTH_APPID"
  ["request_id"]=&gt;
  string(36) "NTkyZmM2ZjhfOGRhMDY4XzViMzdfODc4Njg="
}
</code></pre><p>结合 <em>cos错误码说明</em> 和<code>message</code>字段, 您可以知道发生错误的原因</p>
<h3 id="-sdk-cos-">使用 SDK 移动 COS 文件</h3>
<p>COS 提供通过 SDK, 移动同一 bucket 下的文件位置的能力</p>
<p>在 <em>/data</em> 目录下创建 cos_php_move.php 文件, 并编辑内容如下</p>
<h5 id="-data-cos_php_move-php">示例代码：/data/cos_php_move.php</h5>
<pre><code class="lang-php">&lt;?php
// 包含cos-php-sdk-v4/include.php文件
require('./cos-php-sdk-v4/include.php');
use qcloudcos\Cosapi;

// 设置COS所在的区域，对应关系如下：
//     华南  -&gt; gz
//     华中  -&gt; sh
//     华北  -&gt; tj
Cosapi::setRegion('gz');

$bucket = '';                   // 填写你的bucket名
$src = '/upload_sample_php.txt'; // 填写要移动的 cos 文件的名称, 如 /upload_sample_php.txt
$dst = '';                      // 填写移动后 cos 文件的名称, 如 /upload_sample_php_move.txt

// 上传文件
$ret = Cosapi::moveFile($bucket, $src, $dst);
var_dump($ret);
</code></pre>
<p>将你的 bucket 所属的地区码, bucket 名, 需要移动的文件名, 移动后的文件名 依次填入后执行</p>
<pre><code>cd /data &amp;&amp; php /data/cos_php_move.php
</code></pre><p>如果调用成功, 会返回类似如下结构的信息:</p>
<pre><code>array(3) {
  ["code"]=&gt;
  int(0)
  ["message"]=&gt;
  string(7) "SUCCESS"
  ["request_id"]=&gt;
  string(36) "NTkyZmNhMDlfOGVhMDY4XzgzMzdfODViMDI="
}
</code></pre><p>在 <em>bucket列表</em> 中选择你刚才填入的 bucket 并查看文件列表, 会发现<code>upload_sample_php.txt</code>文件名字变成了 <code>sample_file_move_php.txt</code> 文件, 证明文件移动成功</p>
<h3 id="-sdk-cos-">使用 SDK 创建 COS 目录</h3>
<p>COS 提供通过 SDK, 在 bucket 下创建目录的能力</p>
<p>在 <em>/data</em> 目录下创建 cos_php_create_folder.py 文件, 并编辑内容如下</p>
<h5 id="-data-cos_php_create_folder-php">示例代码：/data/cos_php_create_folder.php</h5>
<pre><code class="lang-php">&lt;?php
// 包含cos-php-sdk-v4/include.php文件
require('./cos-php-sdk-v4/include.php');
use qcloudcos\Cosapi;

// 设置COS所在的区域，对应关系如下：
//     华南  -&gt; gz
//     华中  -&gt; sh
//     华北  -&gt; tj
Cosapi::setRegion('gz');

$bucket = '';                   // 填写你的bucket名
$folderName = '/sample_folder_php'; // 填写要创建的目录的名字

// 上传文件
$ret = Cosapi::createFolder($bucket, $folderName);
var_dump($ret);
</code></pre>
<p>将你的 bucket 所属的地区码, bucket 名, 要创建的目录的名字 依次填入后执行</p>
<pre><code>cd /data &amp;&amp; php /data/cos_php_create_folder.php
</code></pre><p>如果调用成功, 会返回类似如下结构的信息:</p>
<pre><code>array(4) {
  ["code"]=&gt;
  int(0)
  ["message"]=&gt;
  string(7) "SUCCESS"
  ["request_id"]=&gt;
  string(32) "NTkyZmNiNmFfZjFlNjlfYmVhXzgxNzRl"
  ["data"]=&gt;
  array(1) {
    ["ctime"]=&gt;
    int(1496304490)
  }
}
</code></pre><p>在 <em>bucket列表</em> 中选择你刚才填入的 bucket 并查看文件列表, 会发现新增了一个<code>sample_folder_php</code>目录, 证明目录创建成功</p>
<h3 id="-">实验完成</h3>
<p>恭喜您已经完成了 CDN 与 COS for PHP 的学习</p>
</div>