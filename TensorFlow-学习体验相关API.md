<div class="lab-edi-doc"><h1 id="tensorflow-api">TensorFlow — 相关 API</h1>
<h2 id="tensorflow-">TensorFlow 相关函数理解</h2>
<blockquote>
<p>任务时间：时间未知</p>
</blockquote>
<h3 id="tf-truncated_normal">tf.truncated_normal</h3>
<pre><code>truncated_normal(
    shape,
    mean=0.0,
    stddev=1.0,
    dtype=tf.float32,
    seed=None,
    name=None
)
</code></pre><h4 id="-">功能说明：</h4>
<p>产生截断正态分布随机数，取值范围为<code>[mean - 2 * stddev, mean + 2 * stddev]</code>。</p>
<h4 id="-">参数列表：</h4>
<table>
<thead>
<tr>
<th style="text-align:left">参数名</th>
<th style="text-align:left">必选</th>
<th style="text-align:left">类型</th>
<th>说明</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left">shape</td>
<td style="text-align:left">是</td>
<td style="text-align:left">1 维整形张量或array</td>
<td>输出张量的维度</td>
</tr>
<tr>
<td style="text-align:left">mean</td>
<td style="text-align:left">否</td>
<td style="text-align:left">0 维张量或数值</td>
<td>均值</td>
</tr>
<tr>
<td style="text-align:left">stddev</td>
<td style="text-align:left">否</td>
<td style="text-align:left">0 维张量或数值</td>
<td>标准差</td>
</tr>
<tr>
<td style="text-align:left">dtype</td>
<td style="text-align:left">否</td>
<td style="text-align:left">dtype</td>
<td>输出类型</td>
</tr>
<tr>
<td style="text-align:left">seed</td>
<td style="text-align:left">否</td>
<td style="text-align:left">数值</td>
<td>随机种子，若 seed 赋值，每次产生相同随机数</td>
</tr>
<tr>
<td style="text-align:left">name</td>
<td style="text-align:left">否</td>
<td style="text-align:left">string</td>
<td>运算名称</td>
</tr>
</tbody>
</table>
<h4 id="-">示例代码：</h4>
<p>现在您可以在 <em>/home/ubuntu</em> 目录下<em>创建源文件 truncated_normal.py</em>：</p>
<h5 id="-home-ubuntu-truncated_normal-py">示例代码：/home/ubuntu/truncated_normal.py</h5>
<pre><code>#!/usr/bin/python

import tensorflow as tf
initial = tf.truncated_normal(shape=[3,3], mean=0, stddev=1)
print tf.Session().run(initial)
</code></pre><p>然后执行:</p>
<pre><code>python /home/ubuntu/truncated_normal.py
</code></pre><h4 id="-">执行结果：</h4>
<p>将得到一个取值范围 [-2, 2] 的 3 * 3 矩阵，您也可以尝试修改源代码看看输出结果有什么变化？</p>
<h3 id="tf-constant">tf.constant</h3>
<pre><code>constant(
    value,
    dtype=None,
    shape=None,
    name='Const',
    verify_shape=False
)
</code></pre><h4 id="-">功能说明：</h4>
<p>根据 value 的值生成一个 shape 维度的常量张量</p>
<h4 id="-">参数列表：</h4>
<table>
<thead>
<tr>
<th style="text-align:left">参数名</th>
<th style="text-align:left">必选</th>
<th style="text-align:left">类型</th>
<th>说明</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left">value</td>
<td style="text-align:left">是</td>
<td style="text-align:left">常量数值或者 list</td>
<td>输出张量的值</td>
</tr>
<tr>
<td style="text-align:left">dtype</td>
<td style="text-align:left">否</td>
<td style="text-align:left">dtype</td>
<td>输出张量元素类型</td>
</tr>
<tr>
<td style="text-align:left">shape</td>
<td style="text-align:left">否</td>
<td style="text-align:left">1 维整形张量或 array</td>
<td>输出张量的维度</td>
</tr>
<tr>
<td style="text-align:left">name</td>
<td style="text-align:left">否</td>
<td style="text-align:left">string</td>
<td>张量名称</td>
</tr>
<tr>
<td style="text-align:left">verify_shape</td>
<td style="text-align:left">否</td>
<td style="text-align:left">Boolean</td>
<td>检测 shape 是否和 value 的 shape 一致，若为 Fasle，不一致时，会用最后一个元素将 shape 补全</td>
</tr>
</tbody>
</table>
<h4 id="-">示例代码：</h4>
<p>现在您可以在 <em>/home/ubuntu</em> 目录下<em>创建源文件 constant.py</em>，内容可参考：</p>
<h5 id="-home-ubuntu-constant-py">示例代码：/home/ubuntu/constant.py</h5>
<pre><code>#!/usr/bin/python

import tensorflow as tf
import numpy as np
a = tf.constant([1,2,3,4,5,6],shape=[2,3])
b = tf.constant(-1,shape=[3,2])
c = tf.matmul(a,b)

e = tf.constant(np.arange(1,13,dtype=np.int32),shape=[2,2,3])
f = tf.constant(np.arange(13,25,dtype=np.int32),shape=[2,3,2])
g = tf.matmul(e,f)
with tf.Session() as sess:
    print sess.run(a)
    print ("##################################")
    print sess.run(b)
    print ("##################################")
    print sess.run(c)
    print ("##################################")
    print sess.run(e)
    print ("##################################")
    print sess.run(f)
    print ("##################################")
    print sess.run(g)
</code></pre><p>然后执行:</p>
<pre><code>python /home/ubuntu/constant.py
</code></pre><h4 id="-">执行结果：</h4>
<ul>
<li>a: 2x3 维张量；</li>
<li>b: 3x2 维张量；</li>
<li>c: 2x2 维张量；</li>
<li>e: 2x2x3 维张量；</li>
<li>f: 2x3x2 维张量；</li>
<li>g: 2x2x2 维张量。</li>
</ul>
<p>您也可以尝试修改源代码看看输出结果有什么变化？</p>
<h3 id="tf-placeholder">tf.placeholder</h3>
<pre><code>placeholder(
    dtype,
    shape=None,
    name=None
)
</code></pre><h4 id="-">功能说明：</h4>
<p>是一种占位符，在执行时候需要为其提供数据</p>
<h4 id="-">参数列表：</h4>
<table>
<thead>
<tr>
<th style="text-align:left">参数名</th>
<th style="text-align:left">必选</th>
<th style="text-align:left">类型</th>
<th>说明</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left">dtype</td>
<td style="text-align:left">是</td>
<td style="text-align:left">dtype</td>
<td>占位符数据类型</td>
</tr>
<tr>
<td style="text-align:left">shape</td>
<td style="text-align:left">否</td>
<td style="text-align:left">1 维整形张量或 array</td>
<td>占位符维度</td>
</tr>
<tr>
<td style="text-align:left">name</td>
<td style="text-align:left">否</td>
<td style="text-align:left">string</td>
<td>占位符名称</td>
</tr>
</tbody>
</table>
<h4 id="-">示例代码：</h4>
<p>现在您可以在 <em>/home/ubuntu</em> 目录下<em>创建源文件 placeholder.py</em>，内容可参考：</p>
<h5 id="-home-ubuntu-placeholder-py">示例代码：/home/ubuntu/placeholder.py</h5>
<pre><code>#!/usr/bin/python

import tensorflow as tf
import numpy as np

x = tf.placeholder(tf.float32,[None,10])
y = tf.matmul(x,x)
with tf.Session() as sess:
    rand_array = np.random.rand(10,10)
    print sess.run(y,feed_dict={x:rand_array})
</code></pre><p>然后执行:</p>
<pre><code>python /home/ubuntu/placeholder.py
</code></pre><h4 id="-">执行结果：</h4>
<p>输出一个 10x10 维的张量。您也可以尝试修改源代码看看输出结果有什么变化？</p>
<h3 id="tf-nn-bias_add">tf.nn.bias_add</h3>
<pre><code>bias_add(
    value,
    bias,
    data_format=None,
    name=None
)
</code></pre><h4 id="-">功能说明：</h4>
<p>将偏差项 bias 加到 value 上面，可以看做是 tf.add 的一个特例，其中 bias 必须是一维的，并且维度和 value 的最后一维相同，数据类型必须和 value 相同。</p>
<h4 id="-">参数列表：</h4>
<table>
<thead>
<tr>
<th style="text-align:left">参数名</th>
<th style="text-align:left">必选</th>
<th style="text-align:left">类型</th>
<th>说明</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left">value</td>
<td style="text-align:left">是</td>
<td style="text-align:left">张量</td>
<td>数据类型为 float, double, int64, int32, uint8, int16, int8, complex64, or complex128</td>
</tr>
<tr>
<td style="text-align:left">bias</td>
<td style="text-align:left">是</td>
<td style="text-align:left">1 维张量</td>
<td>维度必须和 value 最后一维维度相等</td>
</tr>
<tr>
<td style="text-align:left">data_format</td>
<td style="text-align:left">否</td>
<td style="text-align:left">string</td>
<td>数据格式，支持'NHWC'和'NCHW'</td>
</tr>
<tr>
<td style="text-align:left">name</td>
<td style="text-align:left">否</td>
<td style="text-align:left">string</td>
<td>运算名称</td>
</tr>
</tbody>
</table>
<h4 id="-">示例代码：</h4>
<p>现在您可以在 <em>/home/ubuntu</em> 目录下<em>创建源文件 bias_add.py</em>，内容可参考：</p>
<h5 id="-home-ubuntu-bias_add-py">示例代码：/home/ubuntu/bias_add.py</h5>
<pre><code>#!/usr/bin/python

import tensorflow as tf
import numpy as np

a = tf.constant([[1.0, 2.0],[1.0, 2.0],[1.0, 2.0]])
b = tf.constant([2.0,1.0])
c = tf.constant([1.0])
sess = tf.Session()
print sess.run(tf.nn.bias_add(a, b)) 
#print sess.run(tf.nn.bias_add(a,c)) error
print ("##################################")
print sess.run(tf.add(a, b))
print ("##################################")
print sess.run(tf.add(a, c))
</code></pre><p>然后执行:</p>
<pre><code>python /home/ubuntu/bias_add.py
</code></pre><h4 id="-">执行结果：</h4>
<p>3 个 3x2 维张量。您也可以尝试修改源代码看看输出结果有什么变化？</p>
<h3 id="tf-reduce_mean">tf.reduce_mean</h3>
<pre><code>reduce_mean(
    input_tensor,
    axis=None,
    keep_dims=False,
    name=None,
    reduction_indices=None
)
</code></pre><h4 id="-">功能说明：</h4>
<p>计算张量 input_tensor 平均值</p>
<h4 id="-">参数列表：</h4>
<table>
<thead>
<tr>
<th style="text-align:left">参数名</th>
<th style="text-align:left">必选</th>
<th style="text-align:left">类型</th>
<th>说明</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left">input_tensor</td>
<td style="text-align:left">是</td>
<td style="text-align:left">张量</td>
<td>输入待求平均值的张量</td>
</tr>
<tr>
<td style="text-align:left">axis</td>
<td style="text-align:left">否</td>
<td style="text-align:left">None、0、1</td>
<td>None：全局求平均值；0：求每一列平均值；1：求每一行平均值</td>
</tr>
<tr>
<td style="text-align:left">keep_dims</td>
<td style="text-align:left">否</td>
<td style="text-align:left">Boolean</td>
<td>保留原来的维度，降为 1</td>
</tr>
<tr>
<td style="text-align:left">name</td>
<td style="text-align:left">否</td>
<td style="text-align:left">string</td>
<td>运算名称</td>
</tr>
<tr>
<td style="text-align:left">reduction_indices</td>
<td style="text-align:left">否</td>
<td style="text-align:left">None</td>
<td>和axis等价，被弃用</td>
</tr>
</tbody>
</table>
<h4 id="-">示例代码：</h4>
<p>现在您可以在 <em>/home/ubuntu</em> 目录下<em>创建源文件 reduce_mean.py</em>，内容可参考：</p>
<h5 id="-home-ubuntu-reduce_mean-py">示例代码：/home/ubuntu/reduce_mean.py</h5>
<pre><code>#!/usr/bin/python

import tensorflow as tf
import numpy as np

initial = [[1.,1.],[2.,2.]]
x = tf.Variable(initial,dtype=tf.float32)
init_op = tf.global_variables_initializer()
with tf.Session() as sess:
    sess.run(init_op)
    print sess.run(tf.reduce_mean(x))
    print sess.run(tf.reduce_mean(x,0)) #Column
    print sess.run(tf.reduce_mean(x,1)) #row
</code></pre><p>然后执行:</p>
<pre><code>python /home/ubuntu/reduce_mean.py
</code></pre><h4 id="-">执行结果：</h4>
<pre><code>1.5
[ 1.5  1.5]
[ 1.  2.]
</code></pre><p>您也可以尝试修改源代码看看输出结果有什么变化？</p>
<h3 id="tf-squared_difference">tf.squared_difference</h3>
<pre><code>squared_difference(
    x,
    y,
    name=None
)
</code></pre><h4 id="-">功能说明：</h4>
<p>计算张量 x、y 对应元素差平方</p>
<h4 id="-">参数列表：</h4>
<table>
<thead>
<tr>
<th style="text-align:left">参数名</th>
<th style="text-align:left">必选</th>
<th style="text-align:left">类型</th>
<th>说明</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left">x</td>
<td style="text-align:left">是</td>
<td style="text-align:left">张量</td>
<td>是 half, float32, float64, int32, int64, complex64, complex128 其中一种类型</td>
</tr>
<tr>
<td style="text-align:left">y</td>
<td style="text-align:left">是</td>
<td style="text-align:left">张量</td>
<td>是 half, float32, float64, int32, int64, complex64, complex128 其中一种类型</td>
</tr>
<tr>
<td style="text-align:left">name</td>
<td style="text-align:left">否</td>
<td style="text-align:left">string</td>
<td>运算名称</td>
</tr>
</tbody>
</table>
<h4 id="-">示例代码：</h4>
<p>现在您可以在 <em>/home/ubuntu</em> 目录下<em>创建源文件 squared_difference.py</em>，内容可参考：</p>
<h5 id="-home-ubuntu-squared_difference-py">示例代码：/home/ubuntu/squared_difference.py</h5>
<pre><code>#!/usr/bin/python

import tensorflow as tf
import numpy as np

initial_x = [[1.,1.],[2.,2.]]
x = tf.Variable(initial_x,dtype=tf.float32)
initial_y = [[3.,3.],[4.,4.]]
y = tf.Variable(initial_y,dtype=tf.float32)
diff = tf.squared_difference(x,y)
init_op = tf.global_variables_initializer()
with tf.Session() as sess:
    sess.run(init_op)
    print sess.run(diff)
</code></pre><p>然后执行:</p>
<pre><code>python /home/ubuntu/squared_difference.py
</code></pre><h4 id="-">执行结果：</h4>
<pre><code>[[ 4.  4.]
 [ 4.  4.]]
</code></pre><p>您也可以尝试修改源代码看看输出结果有什么变化？</p>
<h3 id="tf-square">tf.square</h3>
<pre><code>square(
    x,
    name=None
)
</code></pre><h4 id="-">功能说明：</h4>
<p>计算张量对应元素平方</p>
<h4 id="-">参数列表：</h4>
<table>
<thead>
<tr>
<th style="text-align:left">参数名</th>
<th style="text-align:left">必选</th>
<th style="text-align:left">类型</th>
<th>说明</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left">x</td>
<td style="text-align:left">是</td>
<td style="text-align:left">张量</td>
<td>是 half, float32, float64, int32, int64, complex64, complex128 其中一种类型</td>
</tr>
<tr>
<td style="text-align:left">name</td>
<td style="text-align:left">否</td>
<td style="text-align:left">string</td>
<td>运算名称</td>
</tr>
</tbody>
</table>
<h4 id="-">示例代码：</h4>
<p>现在您可以在 <em>/home/ubuntu</em> 目录下<em>创建源文件 square.py</em>，内容可参考：</p>
<h5 id="-home-ubuntu-square-py">示例代码：/home/ubuntu/square.py</h5>
<pre><code>#!/usr/bin/python
import tensorflow as tf
import numpy as np

initial_x = [[1.,1.],[2.,2.]]
x = tf.Variable(initial_x,dtype=tf.float32)
x2 = tf.square(x)
init_op = tf.global_variables_initializer()
with tf.Session() as sess:
    sess.run(init_op)
    print sess.run(x2)
</code></pre><p>然后执行:</p>
<pre><code>python /home/ubuntu/square.py
</code></pre><h4 id="-">执行结果：</h4>
<pre><code>[[ 1.  1.]
 [ 4.  4.]]
</code></pre><p>您也可以尝试修改源代码看看输出结果有什么变化？</p>
<h2 id="tensorflow-">TensorFlow 相关类理解</h2>
<blockquote>
<p>任务时间：时间未知</p>
</blockquote>
<h3 id="tf-variable">tf.Variable</h3>
<pre><code>__init__(
    initial_value=None,
    trainable=True,
    collections=None,
    validate_shape=True,
    caching_device=None,
    name=None,
    variable_def=None,
    dtype=None,
    expected_shape=None,
    import_scope=None
)
</code></pre><h4 id="-">功能说明：</h4>
<p>维护图在执行过程中的状态信息，例如神经网络权重值的变化。</p>
<h4 id="-">参数列表：</h4>
<table>
<thead>
<tr>
<th style="text-align:left">参数名</th>
<th style="text-align:left">类型</th>
<th>说明</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left">initial_value</td>
<td style="text-align:left">张量</td>
<td>Variable 类的初始值，这个变量必须指定 shape 信息，否则后面 validate_shape 需设为 False</td>
</tr>
<tr>
<td style="text-align:left">trainable</td>
<td style="text-align:left">Boolean</td>
<td>是否把变量添加到 collection GraphKeys.TRAINABLE_VARIABLES 中（collection 是一种全局存储，不受变量名生存空间影响，一处保存，到处可取）</td>
</tr>
<tr>
<td style="text-align:left">collections</td>
<td style="text-align:left">Graph collections</td>
<td>全局存储，默认是 GraphKeys.GLOBAL_VARIABLES</td>
</tr>
<tr>
<td style="text-align:left">validate_shape</td>
<td style="text-align:left">Boolean</td>
<td>是否允许被未知维度的 initial_value 初始化</td>
</tr>
<tr>
<td style="text-align:left">caching_device</td>
<td style="text-align:left">string</td>
<td>指明哪个 device 用来缓存变量</td>
</tr>
<tr>
<td style="text-align:left">name</td>
<td style="text-align:left">string</td>
<td>变量名</td>
</tr>
<tr>
<td style="text-align:left">dtype</td>
<td style="text-align:left">dtype</td>
<td>如果被设置，初始化的值就会按照这个类型初始化</td>
</tr>
<tr>
<td style="text-align:left">expected_shape</td>
<td style="text-align:left">TensorShape</td>
<td>要是设置了，那么初始的值会是这种维度</td>
</tr>
</tbody>
</table>
<h4 id="-">示例代码：</h4>
<p>现在您可以在 <em>/home/ubuntu</em> 目录下<em>创建源文件 Variable.py</em>，内容可参考：</p>
<h5 id="-home-ubuntu-variable-py">示例代码：/home/ubuntu/Variable.py</h5>
<pre><code>#!/usr/bin/python

import tensorflow as tf
initial = tf.truncated_normal(shape=[10,10],mean=0,stddev=1)
W=tf.Variable(initial)
list = [[1.,1.],[2.,2.]]
X = tf.Variable(list,dtype=tf.float32)
init_op = tf.global_variables_initializer()
with tf.Session() as sess:
    sess.run(init_op)
    print ("##################(1)################")
    print sess.run(W)
    print ("##################(2)################")
    print sess.run(W[:2,:2])
    op = W[:2,:2].assign(22.*tf.ones((2,2)))
    print ("###################(3)###############")
    print sess.run(op)
    print ("###################(4)###############")
    print (W.eval(sess)) #computes and returns the value of this variable
    print ("####################(5)##############")
    print (W.eval())  #Usage with the default session
    print ("#####################(6)#############")
    print W.dtype
    print sess.run(W.initial_value)
    print sess.run(W.op)
    print W.shape
    print ("###################(7)###############")
    print sess.run(X)
</code></pre><p>然后执行:</p>
<pre><code>python /home/ubuntu/Variable.py
</code></pre><h2 id="-">完成</h2>
<blockquote>
<p>任务时间：时间未知</p>
</blockquote>
<h3 id="-">恭喜，您已完成本实验内容</h3>
<p>您可进行更多 TensorFlow 的系列教程：</p>
<ul>
<li><em>TensorFlow - 线性回归</em></li>
<li><em>TensorFlow - 基于 CNN 数字识别</em></li>
</ul>
<p>关于 TensorFlow 的更多资料可参考 <em>TensorFlow 官网 </em>。</p>
</div>