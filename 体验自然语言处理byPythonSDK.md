<div class="lab-edi-doc"><h1 id="-by-python-sdk">体验自然语言处理 by Python SDK</h1>
<h2 id="-">准备工作</h2>
<blockquote>
<p>任务时间：20min ~ 30min</p>
</blockquote>
<h3 id="-">获取免费额度</h3>
<p>文智为新手提供了免费的额度，访问 <em>文智应用管理</em> 可以领取<sup>[<a href="#stage-1-step-1-out-of-quota">新手专享包</a>]</sup>。</p>
<p>如果您账户中没有足够的文智 API 调用次包额度，将无法正常完成本实验。</p>
<p><a id="stage-1-step-1-out-of-quota"></a></p>
<blockquote>
<p>如果您已使用完免费额度，可以直接购买商业套餐。</p>
</blockquote>
<h3 id="-secretid-secretkey">获取 SecretId 和 SecretKey</h3>
<p>前往 <em>密钥管理</em> 页面获取你的 SecretId 和 SecretKey 信息，这些信息将会在调用接口的时候用到。</p>
<p>如果你还没有创建过密钥，可以在该页面点击 <code>+新建密钥</code> 按钮创建一个。</p>
<h3 id="-">创建相关目录</h3>
<p>在根目录下创建 <code>data</code> 目录，之后操作中相关的代码均放置在此目录下(注：若目录已存在则直接跳过本步骤)。</p>
<pre><code>mkdir /data
</code></pre><h3 id="-git-python-">安装 Git 工具和 Python 环境</h3>
<pre><code>yum install -y git python-pip
</code></pre><h3 id="-qcloudapi-sdk-python">安装 qcloudapi-sdk-python</h3>
<p>安装依赖包 requests</p>
<pre><code>pip install requests
</code></pre><p>安装 qcloudapi-sdk-python</p>
<pre><code>cd /data &amp;&amp; git clone https://github.com/QcloudApi/qcloudapi-sdk-python
</code></pre><h2 id="-sdk-">使用 SDK 体验文智的自然语言处理</h2>
<blockquote>
<p>任务时间：30min ~ 40min</p>
</blockquote>
<h3 id="-">编写代码</h3>
<p>在 /data/qcloudapi-sdk-python 下创建 <em>wenzhi.py</em> 文件，<sup>[<a href="#stage-2-step-1-save-init">Ctrl + S</a>]</sup> 保存，内容如下(注：将 <code>SecretId</code> 和 <code>SecretKey</code> 字段修改为对应取值):</p>
<h5 id="-data-qcloudapi-sdk-python-wenzhi-py">示例代码：/data/qcloudapi-sdk-python/wenzhi.py</h5>
<pre><code class="lang-python">#!/usr/bin/python
# -*- coding: utf-8 -*-

# 引入云API入口模块
from src.QcloudApi.qcloudapi import QcloudApi

module = 'wenzhi'

'''
action 对应接口的接口名，请参考wiki文档上对应接口的接口名
'''
action = 'TextSentiment'

config = {
    'secretId': '之前取得的 secretId', 
    'secretKey': '之前取得的 secretKey', 
    'Region': 'gz',
    'method': 'POST'
}

'''
params 请求参数，请参考wiki文档上对应接口的说明
'''
params = {"content": "李亚鹏挺王菲：加油！孩他娘。"}

try:
    service = QcloudApi(module, config)

    # 生成请求的URL，不发起请求
    print service.generateUrl(action, params)
    # 调用接口，发起请求
    print service.call(action, params)
except Exception, e:
    print 'exception:', e
</code></pre>
<p><a id="stage-2-step-1-save-init"></a></p>
<blockquote>
<p>Mac 用户请按键盘 <code>Cmd + S</code> 进行保存</p>
</blockquote>
<h3 id="-">体验文智的自然语言处理</h3>
<p>执行以下命令，就可以得到对 "李亚鹏挺王菲：加油！孩儿他娘。" 这句话的情感分析结果。</p>
<pre><code>cd /data/qcloudapi-sdk-python &amp;&amp; python wenzhi.py
</code></pre><p>得到类似如下的结果， 证明调用成功。</p>
<pre><code>{"code":0,"message":"","codeDesc":"Success","positive":0.99481022357941,"negative":0.0051898001693189}
</code></pre><p>各字段的含义如下：</p>
<p>positive    正面情感概率</p>
<p>negative    负面情感概率</p>
<p>code        0表示成功，非0表示失败</p>
<p>message        失败时候的错误信息，成功则无该字段</p>
<p>文智的更多相关接口和文档， 请访问 <em>文智-文档中心</em> 获取更多信息。</p>
<h3 id="-">大功告成</h3>
<p>恭喜您已经完成了体验自然语言处理 by Python SDK 的学习。</p>
</div>