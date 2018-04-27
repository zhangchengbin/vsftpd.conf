<div class="lab-edi-doc"><h1 id="-by-php-sdk">体验万象优图 by PHP SDK</h1>
<h2 id="-">准备工作</h2>
<blockquote>
<p>任务时间：30min ~ 60min</p>
</blockquote>
<h3 id="-">实名认证</h3>
<p>在<em>使用万象优图</em>前，您需要实名认证。如果您已经实名认证，可以直接跳过这一步。</p>
<p><em>前往实名认证</em>，本实验使用万象优图可能产生小额费用。</p>
<h3 id="-">获取密钥信息</h3>
<p>前往 <em>密钥管理</em> 页面获取你的 APPID，SecretId 和 SecretKey 信息，这些信息将会在调用万象优图的接口时候用到。</p>
<p>如果你还没有创建过密钥，可以在该页面点击 <code>+新建密钥</code> 按钮创建一个</p>
<h3 id="-bucket">创建 Bucket</h3>
<p>Bucket 用于存储使用万象优图时候用到的图片。<em>点击这里</em>前往腾讯云控制台 <code>万象优图 - Bucket管理</code> 页面创建一个 Bucket 并记住名称，其他选项默认即可。</p>
<h3 id="-">配置使用环境</h3>
<h4 id="-git-php">安装 Git 与 PHP</h4>
<pre><code>yum install -y git php php-common php-devel
</code></pre><h4 id="-">创建测试要用到的图片</h4>
<p>创建 /data/img 目录用于存放图片</p>
<pre><code>mkdir -p /data/img
</code></pre><p>您可以随意上传一张测试用的图片到此服务器的 <em>/data/img</em> 目录，或者直接使用实验室提供的如下图片:</p>
<p><img src="https://imgcache.qq.com/open_proj/proj_qcloud_v2/gateway/event/pc/ci-identify/css/img/demo/demo_10.jpg" alt="使用实验室提供的图片"></p>
<p>使用下面的命令将此图片保存到 <em>/data/img</em> 目录。</p>
<pre><code>wget https://imgcache.qq.com/open_proj/proj_qcloud_v2/gateway/event/pc/ci-identify/css/img/demo/demo_10.jpg -O /data/img/demo.jpg
</code></pre><h4 id="-sdk-for-php">安装 万象优图 SDK for PHP</h4>
<pre><code>git clone https://github.com/tencentyun/image-php-sdk-v2.0 /data/image-php-sdk
</code></pre><h2 id="-api">使用万象优图的鉴黄API</h2>
<blockquote>
<p>任务时间：时间未知</p>
</blockquote>
<h3 id="-">编写调用代码</h3>
<p>在 <em>/data</em> 目录下创建 ci_detect_porn_php.php 文件, 内容如下:</p>
<h5 id="-data-ci_detect_porn_php-php">示例代码：/data/ci_detect_porn_php.php</h5>
<pre><code class="lang-php">&lt;?php
require_once __DIR__ . '/image-php-sdk/index.php';
use QcloudImage\CIClient;

$client = new CIClient('你的APP_ID', '你的SECRET_ID', '你的SECRET_KEY', '你的BUCKET名称');
$client-&gt;setTimeout(30);

var_dump($client-&gt;pornDetect(
    array('files'=&gt;array('./img/demo.jpg')) // 可将此处鉴别的图片替换成自己要鉴定的图片
    ));
</code></pre>
<h3 id="-">执行并查看结果</h3>
<p>将我们在<code>准备工作</code>这部分准备的AppId, secret_id 等信息填入代码中, <sup>[<a href="#stage-2-step-2-save-init">Ctrl + S</a>]</sup> 保存后, 执行以下命令来运行编写好的 PHP 代码</p>
<pre><code>cd /data &amp;&amp; php ci_detect_porn_php.php
</code></pre><p>如果调用成功, 会返回类似如下的信息:</p>
<pre><code>string(216) "{
    "result_list": [
        {
            "code": 0,
            "message": "success",
            "filename": "/data/img/demo.jpg",
            "data": {
                "result": 0,
                "forbid_status": 0,
                "confidence": 26.683,
                "hot_score": 99.657,
                "normal_score": 0.342,
                "porn_score": 0.001
            }
        }
    ],
    "http_code": 200
}"
</code></pre><p>其中返回字段数据代表的意义如下:</p>
<ul>
<li><code>result</code>: 供参考的识别结果，0正常，1黄图，2疑似图片</li>
<li><code>confidence</code>: 识别为黄图的置信度，范围0-100；是normal_score, hot_score, porn_score的综合评分</li>
<li><code>normal_score</code>: 图片为正常图片的评分</li>
<li><code>hot_score</code>: 图片为性感图片的评分</li>
<li><code>porn_score</code>: 图片为色情图片的评分</li>
<li><code>forbid_status</code>: 封禁状态，0表示正常，1表示图片已被封禁（只有存储在万象优图的图片才会被封禁）</li>
</ul>
<p><em>点击这里</em>可以查看更详细的信息。</p>
<p>如果调用失败, 会返回类似如下的信息:</p>
<pre><code>string(52) "{"code":14,"message":"sign no pass","http_code":401}"
</code></pre><p>结合错误码说明和<code>message</code>字段, 您可以知道发生错误的原因</p>
<p>点击查看 <em>万象优图错误码说明</em></p>
<p><a id="stage-2-step-2-save-init"></a></p>
<blockquote>
<p>Mac 用户请按键盘 <code>Cmd + S</code> 进行保存</p>
</blockquote>
<h2 id="-">使用万象优图提取图片标签</h2>
<blockquote>
<p>任务时间：时间未知</p>
</blockquote>
<p>万象优图提供从图片中提取相关标签的能力</p>
<h3 id="-">编写调用代码</h3>
<p>在 <em>/data</em> 目录下创建 ci_extra_tag_php.php 文件, 内容如下:</p>
<h5 id="-data-ci_extra_tag_php-php">示例代码：/data/ci_extra_tag_php.php</h5>
<pre><code class="lang-php">&lt;?php
require_once __DIR__ . '/image-php-sdk/index.php';
use QcloudImage\CIClient;

$client = new CIClient('你的APP_ID', '你的SECRET_ID', '你的SECRET_KEY', '你的BUCKET名称');
$client-&gt;setTimeout(30);

var_dump($client-&gt;tagDetect(
    array('file'=&gt;'./img/demo.jpg') // 可将此处鉴别的图片替换成自己要鉴定的图片
    ));
</code></pre>
<h3 id="-">执行并查看结果</h3>
<p>将我们在<code>准备工作</code>这部分准备的AppId, secret_id 等信息填入代码中, <sup>[<a href="#stage-3-step-2-save-init">Ctrl + S</a>]</sup> 保存后, 执行以下命令来运行编写好的 PHP 代码</p>
<pre><code>cd /data &amp;&amp; php ci_extra_tag_php.php
</code></pre><p>如果调用成功, 会返回类似如下的信息:</p>
<pre><code>string(205)"{
    "code": 0,
    "message": "success",
    "tags": [
        {
            "tag_name": "写真照",
            "tag_confidence": 18
        },
        {
            "tag_name": "女孩",
            "tag_confidence": 48
        },
        {
            "tag_name": "手脚",
            "tag_confidence": 27
        }
    ],
    "http_code": 200
}"
</code></pre><p>其中返回字段数据代表的意义如下:</p>
<ul>
<li><code>tag_name</code> 匹配的标签</li>
<li><code>tag_confidence</code> 标签的匹配度</li>
</ul>
<p><a id="stage-3-step-2-save-init"></a></p>
<blockquote>
<p>Mac 用户请按键盘 <code>Cmd + S</code> 进行保存</p>
</blockquote>
<h2 id="-">使用万象优图识别身份证</h2>
<blockquote>
<p>任务时间：时间未知</p>
</blockquote>
<h3 id="-">编写调用代码</h3>
<p>在 <em>/data</em> 目录下创建 ci_recon_idcard_php.php 文件, 内容如下:</p>
<h5 id="-data-ci_recon_idcard_php-php">示例代码：/data/ci_recon_idcard_php.php</h5>
<pre><code class="lang-php">&lt;?php
require_once __DIR__ . '/image-php-sdk/index.php';
use QcloudImage\CIClient;

$client = new CIClient('你的APP_ID', '你的SECRET_ID', '你的SECRET_KEY', '你的BUCKET名称');
$client-&gt;setTimeout(30);

# 您可以替换下面的身份证照片为自己的, 参数 0 代表识别的是身份证的正面, 1 代表识别的是身份证的背面
var_dump($client-&gt;idcardDetect(
    array('urls'=&gt; array('http://imgs.focus.cn/upload/sz/5876/a_58758051.jpg')), 0));
</code></pre>
<h3 id="-">执行并查看结果</h3>
<p>将我们在<code>准备工作</code>这部分准备的AppId, secret_id 等信息填入代码中, <sup>[<a href="#stage-4-step-2-save-init">Ctrl + S</a>]</sup> 保存后, 执行以下命令来运行编写好的 PHP 代码</p>
<pre><code>cd /data &amp;&amp; php ci_recon_idcard_php.php
</code></pre><p>如果调用成功, 会返回类似如下的信息:</p>
<pre><code>string(609) "{
    "result_list": [
        {
            "code": 0,
            "message": "OK",
            "url": "http://imgs.focus.cn/upload/sz/5876/a_58758051.jpg",
            "data": {
                "name": "李时杰",
                "sex": "男",
                "nation": "汉",
                "birth": "1988/4/26",
                "address": "湖北省荆州市荆州区南环路199号",
                "id": "420822198804266119",
                "name_confidence_all": [95, 98, 99],
                "sex_confidence_all": [56],
                "nation_confidence_all": [37],
                "birth_confidence_all": [63, 56, 57, 60],
                "address_confidence_all": [19, 47, 33, 18, 24, 36, 23, 20, 37, 21, 20, 10, 6, 3, 0, 40],
                "id_confidence_all": [58, 59, 77, 54, 61, 58, 63, 56, 57, 53, 73, 53, 60, 56, 53, 59, 56, 58]
            }
        }
    ],
    "http_code": 200
}"
</code></pre><p>其中返回字段数据代表的意义如下:</p>
<ul>
<li><code>name</code> 姓名</li>
<li><code>sex</code> 性别</li>
<li><code>nation</code> 民族</li>
<li><code>birth</code> 出生日期</li>
<li><code>address</code> 地址</li>
<li><code>id</code> 身份证号</li>
<li><code>name_confidence_all</code> 证件姓名置信度</li>
<li><code>sex_confidence_all</code> 性别置信度</li>
<li><code>nation_confidence_all</code> 民族置信度</li>
<li><code>birth_confidence_all</code> 出生日期置信度</li>
<li><code>address_confidence_all</code> 地址置信度</li>
<li><code>id_confidence_all</code> 身份证号置信度</li>
</ul>
<p>置信度的值为区间在[0,100]的整数</p>
<p><em>点击这里</em>可以查看更详细的信息。</p>
<p><a id="stage-4-step-2-save-init"></a></p>
<blockquote>
<p>Mac 用户请按键盘 <code>Cmd + S</code> 进行保存</p>
</blockquote>
<h3 id="-">完成实验</h3>
<p>恭喜！您已经成功完成了<code>使用万象优图 SDK for PHP</code> 的实验任务。</p>
</div>