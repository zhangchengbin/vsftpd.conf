<div class="lab-edi-doc"><h1 id="-jupyter-notebook-centos-">玩转 Jupyter Notebook （CentOS）</h1>
<h2 id="-jupyter-notebook">安装 Jupyter Notebook</h2>
<blockquote>
<p>任务时间：20min</p>
</blockquote>
<h3 id="jupyter-notebook-">Jupyter Notebook 简介</h3>
<p><strong>Jupyter Notebook</strong> 是一个开源的 Web 应用程序，可以用来创建和共享包含动态代码、方程式、可视化及解释性文本的文档。</p>
<p>其应用于包括：数据整理与转换，数值模拟，统计建模，机器学习等等。</p>
<p>更多信息请见 <a target="_blank" href="http://jupyter.org/" title="null">官网</a> 。</p>
<h3 id="-python-">检查 Python 环境</h3>
<p>CentOS 7.2 中默认集成了 Python 2.7，可以通过下面命令检查 Python 版本：</p>
<pre><code>python --version
</code></pre><h3 id="-pip">安装 pip</h3>
<p>pip 是一个 Python 包管理工具，我们使用 yum 命令来安装该工具：</p>
<pre><code>yum -y install python-pip
</code></pre><p>使用下面命令升级 pip 到最新版本：</p>
<pre><code>pip install --upgrade pip
</code></pre><h3 id="-">安装相关依赖</h3>
<p>安装 Jupyter 过程中还需要其他一些依赖，我们使用以下命令安装他们：</p>
<pre><code>yum -y groupinstall "Development Tools"
yum -y install python-devel
</code></pre><h3 id="-">配置虚拟环境</h3>
<h4 id="-virtualenv">安装 virtualenv</h4>
<p>我们将为 Jupyter 创建一个独立的虚拟环境，与系统自带的 Python 隔离开来。为此，先安装 virtualenv 库：</p>
<pre><code>pip install virtualenv
</code></pre><h4 id="-">创建虚拟环境</h4>
<p>创建一个专门的虚拟环境，并直接激活进入该环境：</p>
<pre><code>virtualenv venv
source venv/bin/activate
</code></pre><h3 id="-pip-jupyter">使用 pip 安装 Jupyter</h3>
<p>我们使用 pip 命令安装 Jupyter：</p>
<pre><code>pip install jupyter
</code></pre><h2 id="-jupyter-notebook">配置 Jupyter Notebook</h2>
<blockquote>
<p>任务时间：10min</p>
</blockquote>
<h3 id="-">建立项目目录</h3>
<p>我们先为 Jupyter 相关文件准备一个目录：</p>
<pre><code>mkdir /data/jupyter
cd /data/jupyter
</code></pre><p>再建立一个目录作为 Jupyter 运行的根目录：</p>
<pre><code>mkdir /data/jupyter/root
</code></pre><h3 id="-">准备密码密文</h3>
<p>由于我们将以需要密码验证的模式启动 Jupyter，所以我们要预先生成所需的密码对应的密文。</p>
<h4 id="-">生成密文</h4>
<p>使用下面的命令，创建一个密文的密码：</p>
<pre><code>python -c "import IPython;print IPython.lib.passwd()"
</code></pre><p>执行后需要输入并确认密码，然后程序会返回一个 <code>'sha1:...'</code> 的密文，我们接下来将会用到它。</p>
<h3 id="-">修改配置</h3>
<h4 id="-">生成配置文件</h4>
<p>我们使用 <code>--generate-config</code> 来参数生成默认配置文件：</p>
<pre><code>jupyter notebook --generate-config --allow-root
</code></pre><p>生成的配置文件在 <code>/root/.jupyter/</code> 目录下，可以<em>点此编辑配置</em>。</p>
<h4 id="-">修改配置</h4>
<p>然后在配置文件最下方加入以下配置：</p>
<pre><code>c.NotebookApp.ip = '*'
c.NotebookApp.allow_root = True
c.NotebookApp.open_browser = False
c.NotebookApp.port = 8888
c.NotebookApp.password = u'刚才生成的密文(sha:...)'
c.ContentsManager.root_dir = '/data/jupyter/root'
</code></pre><p>其中：</p>
<ul>
<li><code>c.NotebookApp.password</code> 请将上一步中密文填入此项，包括 sha: 部分。</li>
</ul>
<p>你也可以直接配置或使用 <code>Nginx</code> 将服务代理到 80 或 443 端口。</p>
<h2 id="-jupyter-notebook">启动 Jupyter Notebook</h2>
<blockquote>
<p>任务时间：10min</p>
</blockquote>
<h3 id="-">直接启动</h3>
<p>使用以下指令启动 Jupyter Notebook：</p>
<pre><code>jupyter notebook
</code></pre><p>此时，访问 <a target="_blank" href="http://&lt;您的 CVM IP 地址&gt;:8888" title="null">http://&lt;您的 CVM IP 地址&gt;:8888</a> 即可进入 Jupyter 首页。</p>
<h3 id="-notebook">创建 Notebook</h3>
<ul>
<li>进入<code>【首页】</code>首先需要输入前面步骤中设置的密码。</li>
<li>然后点击右侧的<code>【 new 】</code>，选择 Python2 新建一个 notebook，这时跳转至编辑界面。</li>
<li>现在我们可以看到 <em>/data/jupyter/root/</em> 目录中出现了一个 <code>Untitled.ipynb</code> 文件，这就是我们刚刚新建的 Notebook 文件。我们建立的所有 Notebook 都将默认以该类型的文件格式保存。 </li>
</ul>
<h3 id="-">后台运行</h3>
<p>直接以 <code>jupyter notebook</code> 命令启动 Jupyter 的方式在连接断开时将会中断，所以我们需要让 Jupyter 服务在后台常驻。</p>
<p>先按下 <code>Ctrl + C</code> 并输入 <code>y</code> 停止 Jupyter 服务，然后执行以下命令：</p>
<pre><code>nohup jupyter notebook &gt; /data/jupyter/jupyter.log 2&gt;&amp;1 &amp;
</code></pre><p>该命令将使得 Jupyter 在后台运行，并将日志写在 <em>/data/jupyter/jupyter.log</em> 文件中。</p>
<h3 id="-notebook">准备后续步骤的 Notebook</h3>
<p>为了后面实验中实验室的步骤检查器能够更好的工作，此时我们使用以下命令预先创建几份 ipynb 文件：</p>
<pre><code>cd /data/jupyter/root
cp Untitled.ipynb first.ipynb
cp Untitled.ipynb matplotlib.ipynb
cp Untitled.ipynb tensorflow.ipynb
rm -f Untitled.ipynb
</code></pre><h2 id="-jupyter-notebook">使用 Jupyter Notebook</h2>
<blockquote>
<p>任务时间：30min</p>
</blockquote>
<ul>
<li><strong>接下来的步骤中如遇到步骤检查未通过，请按下 <code>Ctrl + S</code> 保存，等待步骤检查器确认。</strong></li>
</ul>
<h3 id="-">编辑界面</h3>
<p><strong>点击<a target="_blank" href="http://&lt;您的 CVM IP 地址&gt;:8888/notebooks/first.ipynb" title="null">这里</a>打开 first.ipynb 编辑界面。</strong></p>
<p><strong>Jupyter Notebook</strong> 的编辑界面主要由 <strong>工具栏</strong> 和 <strong>内容编辑区</strong> 构成。</p>
<p>下方编辑区，由 <strong>Cell</strong> 组成。每个 notebook 由多个 <strong>Cell</strong> 构成，每个 <strong>Cell</strong> 都可以有不同的用途。</p>
<h3 id="code-cell">Code Cell</h3>
<p>新建的 notebook 中包含一个代码 <strong>Cell</strong>（Code Cell），以 <code>[ ]</code> 开头，在该类型的 <strong>Cell</strong> 中，可以输入任意代码并执行。如输入：</p>
<pre><code>1 + 1
</code></pre><p>然后按下 <code>Shift + Enter</code> 键， <strong>Cell</strong> 中代码就会被执行，光标也会移动至下个新 <strong>Cell</strong> 中。我们接着输入：</p>
<pre><code>print('Hello Jupyter')
</code></pre><p>再次按下 <code>Shift + Enter</code> ，可以看到这次没有出现 <code>Out[..]</code> 这样的文字。这是因为我们只打印出来了某些值，而没有返回任何的值。</p>
<ul>
<li><strong>按下 <code>Ctrl + S</code> 保存，等待步骤检查器确认。</strong></li>
</ul>
<h3 id="heading-cell-">Heading Cell *</h3>
<p>新版本中已经没有独立的 <code>Heading Cell</code>，现在标题被整合在 <code>Markdown Cell</code> 之中。</p>
<p>如果我们想在顶部添加一个的标题。选中第一个 <strong>Cell</strong>，然后点击 <code>Insert -&gt; Insert Cell Above</code>。</p>
<p>你会发现，文档顶部马上就出现了一个新的 <strong>Cell</strong>。点击在工具栏中 <strong>Cell</strong> 类型（默认为 Code），将其变成 Markdown。接着在 <strong>Cell</strong> 中写下：</p>
<pre><code># My First Notebook
</code></pre><p>然后按下 <code>Shift + Enter</code> 键，便可以看到生成了一行一级标题。</p>
<ul>
<li><strong>与 Markdown 语法相同，使用多个<code>#</code>将改变标题级别。</strong></li>
</ul>
<h3 id="markdown-cell">Markdown Cell</h3>
<p>上一步中我们已经尝试了使用了 <code>Markdown Cell</code>。在该 Cell 中，除标题外其他语法同样支持。比如，我们在一个新的 Cell 中插入以下文本：</p>
<pre><code>This is a **table**:

| Name | Value |
|:----:|:-----:|
|    A |     1 |
|    B |     2 |
|    C |     3 |
</code></pre><p>然后按下 <code>Shift + Enter</code>，即可渲染出相应内容。</p>
<h4 id="-html">高级用法 - HTML</h4>
<p><code>Markdown Cell</code> 中同样接受 HTML 代码。这样，你就可以实现更加丰富的样式及结构、添加图片等等。</p>
<p>例如，如果想在 notebook 中添加 Jupyter 的 logo，并且添加 2px 的黑色边框，放置在单元格左侧，可以这样编写：</p>
<pre><code>&lt;img src="http://jupyter.org/assets/nav_logo.svg" style="border: 2px solid black; float:left" /&gt;
</code></pre><p>然后按下 <code>Shift + Enter</code>，即可渲染出图片。</p>
<h4 id="-latex">高级用法 - LaTex</h4>
<p>Markdown Cell 还支持 LaTex 语法。在 Cell 中插入以下文本：</p>
<pre><code>$$int_0^{+infty} x^2 dx$$
</code></pre><p>同样按下 <code>Shift + Enter</code>，即可渲染出公式。</p>
<h3 id="-">导出</h3>
<p>notebook 支持导出导出为 HTML、Markdown、PDF 等多种格式。</p>
<p>如点击 <code>File -&gt; Download as -&gt; HTML(.html)</code>，即可下载到 HTML 版本的 notebook。</p>
<h4 id="-pdf">导出 PDF</h4>
<p>其中，导出 PDF 需要其他包的支持，我们需要使用以下命令安装这些依赖：</p>
<pre><code>yum -y install pandoc texlive-*
</code></pre><ul>
<li><strong>注：直接导出 PDF 时 Jupyter 可能会忽略一些 Cell，建议先导出为 HTML，然后使用浏览器将其转为 PDF。</strong></li>
</ul>
<h2 id="-matplotlib-">集成 Matplotlib（可选）</h2>
<blockquote>
<p>任务时间：30min</p>
</blockquote>
<p><strong>Matplotlib</strong> 是 Python 中最常用的可视化工具之一，可以非常方便地创建许多类型的 2D 图表和基本的 3D 图表。</p>
<h3 id="-matplotlib">安装 Matplotlib</h3>
<p>我们使用 pip 来安装 Matplotlib：</p>
<pre><code>pip install matplotlib
</code></pre><h3 id="-matplotlib">测试 Matplotlib</h3>
<p>我们使用另一个 notebook （matplotlib.ipynb）来测试 Matplotlib。</p>
<p><strong>点击<a target="_blank" href="http://&lt;您的 CVM IP 地址&gt;:8888/notebooks/matplotlib.ipynb" title="null">这里</a>打开 matplotlib.ipynb 编辑界面。</strong></p>
<h4 id="-">魔法命令</h4>
<p>在第一个 <strong>Cell</strong> 中，我们插入并执行：</p>
<pre><code>%matplotlib inline
</code></pre><p>这是指定 matplotlib 图表的显示方式的魔法命令。inline 表示将图表嵌入到 notebook 中。</p>
<h4 id="-">测试</h4>
<ul>
<li>关于 Matplotlib 的使用请移步其<a target="_blank" href="http://matplotlib.org/" title="null">官网</a>。</li>
</ul>
<p>在接下来 <strong>Cell</strong> 中，我们插入几个官方示例测试：</p>
<p>1.<a target="_blank" href="http://matplotlib.org/examples/style_sheets/plot_bmh.html" title="null">plot_bmh</a>：</p>
<h5 id="-plot_bmh-py">示例代码：/plot_bmh.py</h5>
<pre><code>from numpy.random import beta
import matplotlib.pyplot as plt


plt.style.use('bmh')


def plot_beta_hist(ax, a, b):
    ax.hist(beta(a, b, size=10000), histtype="stepfilled",
            bins=25, alpha=0.8, normed=True)


fig, ax = plt.subplots()
plot_beta_hist(ax, 10, 10)
plot_beta_hist(ax, 4, 12)
plot_beta_hist(ax, 50, 12)
plot_beta_hist(ax, 6, 55)
ax.set_title("'bmh' style sheet")

plt.show()
</code></pre><p><code>Shift + Enter</code> 执行 <strong>Cell</strong>，即可看到绘制出的图像。</p>
<p>2.<a target="_blank" href="http://matplotlib.org/examples/pyplots/whats_new_99_mplot3d.html" title="null">whats_new_99_mplot3d</a>：</p>
<h5 id="-whats_new_99_mplot3d-py">示例代码：/whats_new_99_mplot3d.py</h5>
<pre><code>import random

import numpy as np
import matplotlib.pyplot as plt
from matplotlib import cm
from mpl_toolkits.mplot3d import Axes3D

X = np.arange(-5, 5, 0.25)
Y = np.arange(-5, 5, 0.25)
X, Y = np.meshgrid(X, Y)
R = np.sqrt(X**2 + Y**2)
Z = np.sin(R)

fig = plt.figure()
ax = Axes3D(fig)
ax.plot_surface(X, Y, Z, rstride=1, cstride=1, cmap=cm.viridis)

plt.show()
</code></pre><p>同样执行 <strong>Cell</strong>，即可看到绘制出的图像。</p>
<h3 id="-">动手试试</h3>
<p>最后，我们来尝试绘制一个<strong>二次函数</strong>图像，你可以自行实现，也可以参考下面代码：</p>
<h5 id="-my-py">示例代码：/my.py</h5>
<pre><code>import matplotlib.pyplot as plt
import numpy as np

x = np.arange(-10, 11)
y = x**2

plt.plot(x, y)
plt.show()
</code></pre><h2 id="-tensorflow-">搭配 TensorFlow（可选）</h2>
<blockquote>
<p>任务时间：30min</p>
</blockquote>
<p><strong>TensorFlow™</strong> 是一个采用数据流图，用于数值计算的开源软件库。它灵活的架构让你可以在多种平台上展开计算，例如台式计算机中的一个或多个CPU（或GPU），服务器，移动设备等等。</p>
<p><strong>TensorFlow</strong> 最初由 Google 大脑小组的研究员和工程师们开发出来，用于机器学习和深度神经网络方面的研究，但这个系统的通用性使其也可广泛用于其他计算领域。</p>
<h3 id="-tensorflow">安装 TensorFlow</h3>
<p>我们使用 pip 安装相关依赖及 Tensorflow</p>
<pre><code>pip install protobuf
pip install tensorflow
</code></pre><h3 id="-tensorflow">测试 TensorFlow</h3>
<ul>
<li>关于 TensorFlow 的使用请移步其<a target="_blank" href="https://cloud.tencent.com" title="null">官网</a>，这里只是测试其在 Jupiter 中是否可用。</li>
</ul>
<p><strong>点击<a target="_blank" href="http://&lt;您的 CVM IP 地址&gt;:8888/notebooks/tensorflow.ipynb" title="null">这里</a>打开 tensorflow.ipynb 编辑界面。</strong></p>
<p>在 <strong>Cell</strong> 中加入以下代码（整理自<a target="_blank" href="https://www.tensorflow.org/get_started/mnist/beginners" title="null">官网 MNIST 教程</a>）：</p>
<h5 id="-tensorflow-py">示例代码：/tensorflow.py</h5>
<pre><code>from tensorflow.examples.tutorials.mnist import input_data
import tensorflow as tf

# The MNIST Data
mnist = input_data.read_data_sets("MNIST_data/", one_hot=True)

# Regression
x = tf.placeholder(tf.float32, [None, 784])
W = tf.Variable(tf.zeros([784, 10]))
b = tf.Variable(tf.zeros([10]))
y = tf.nn.softmax(tf.matmul(x, W) + b)

# Training
y_ = tf.placeholder(tf.float32, [None, 10])
cross_entropy = tf.reduce_mean(-tf.reduce_sum(y_ * tf.log(y), reduction_indices=[1]))
train_step = tf.train.GradientDescentOptimizer(0.05).minimize(cross_entropy)

sess = tf.InteractiveSession()

tf.global_variables_initializer().run()

for _ in range(1000):
    batch_xs, batch_ys = mnist.train.next_batch(100)
    sess.run(train_step, feed_dict={x: batch_xs, y_: batch_ys})

# Evaluating
correct_prediction = tf.equal(tf.argmax(y,1), tf.argmax(y_,1))
accuracy = tf.reduce_mean(tf.cast(correct_prediction, tf.float32))

print(sess.run(accuracy, feed_dict={x: mnist.test.images, y_: mnist.test.labels}))
</code></pre><p>按下 <code>Shift + Enter</code>，学习过程结束后可以看到输出了准确率（92% 左右）。</p>
<h2 id="-">自由体验</h2>
<blockquote>
<p>任务时间：20min</p>
</blockquote>
<p>接下来你可以自由体验搭建起的云端 Jupyter Notebook。</p>
<h3 id="-">完成实验</h3>
<p>恭喜！您已经成功搭建起了一个云端的 Jupyter Notebook 环境。你可以选择保留已经运行的服务，继续进行 Jupyter Notebook 的使用。</p>
</div>