<div class="lab-edi-doc"><h1 id="tensorflow-">TensorFlow - 深度学习破解验证码</h1>
<h2 id="-">简介</h2>
<blockquote>
<p>任务时间：时间未知</p>
</blockquote>
<p>验证码主要用于防刷，传统的验证码识别算法一般需要把验证码分割为单个字符，然后逐个识别，如果字符之间相互重叠，传统的算法就然并卵了，本文采用cnn对验证码进行整体识别。通过本文的学习，大家可以学到几点：1.captcha库生成验证码；2.如何将验证码识别问题转化为分类问题；3.可以训练自己的验证码识别模型。</p>
<h3 id="-captcha-">安装 captcha 库</h3>
<pre><code>sudo pip install captcha
</code></pre><h2 id="-">生成验证码训练数据</h2>
<blockquote>
<p>任务时间：时间未知</p>
</blockquote>
<p>所有的模型训练，数据是王道，本文采用 captcha 库生成验证码，captcha 可以生成语音和图片验证码，我们采用生成图片验证码功能，验证码是由数字、大写字母、小写字母组成（当然你也可以根据自己的需求调整，比如添加一些特殊字符），长度为 4，所以总共有 62^4 种组合验证码。</p>
<h3 id="-">验证码生成器</h3>
<p>采用 python 中生成器方式来生成我们的训练数据，这样的好处是，不需要提前生成大量的数据，训练过程中生成数据，并且可以无限生成数据。</p>
<p><strong>示例代码：</strong></p>
<p>现在您可以在 <em>/home/ubuntu</em> 目录下<em>创建源文件 generate_captcha.py</em>，内容可参考：</p>
<h5 id="-home-ubuntu-generate_captcha-py">示例代码：/home/ubuntu/generate_captcha.py</h5>
<pre><code>#!/usr/bin/python
# -*- coding: utf-8 -*

from captcha.image import ImageCaptcha
from PIL import Image
import numpy as np
import random
import string

class generateCaptcha():
    def __init__(self,
                 width = 160,#验证码图片的宽
                 height = 60,#验证码图片的高
                 char_num = 4,#验证码字符个数
                 characters = string.digits + string.ascii_uppercase + string.ascii_lowercase):#验证码组成，数字+大写字母+小写字母
        self.width = width
        self.height = height
        self.char_num = char_num
        self.characters = characters
        self.classes = len(characters)

    def gen_captcha(self,batch_size = 50):
        X = np.zeros([batch_size,self.height,self.width,1])
        img = np.zeros((self.height,self.width),dtype=np.uint8)
        Y = np.zeros([batch_size,self.char_num,self.classes])
        image = ImageCaptcha(width = self.width,height = self.height)

        while True:
            for i in range(batch_size):
                captcha_str = ''.join(random.sample(self.characters,self.char_num))
                img = image.generate_image(captcha_str).convert('L')
                img = np.array(img.getdata())
                X[i] = np.reshape(img,[self.height,self.width,1])/255.0
                for j,ch in enumerate(captcha_str):
                    Y[i,j,self.characters.find(ch)] = 1
            Y = np.reshape(Y,(batch_size,self.char_num*self.classes))
            yield X,Y

    def decode_captcha(self,y):
        y = np.reshape(y,(len(y),self.char_num,self.classes))
        return ''.join(self.characters[x] for x in np.argmax(y,axis = 2)[0,:])

    def get_parameter(self):
        return self.width,self.height,self.char_num,self.characters,self.classes

    def gen_test_captcha(self):
        image = ImageCaptcha(width = self.width,height = self.height)
        captcha_str = ''.join(random.sample(self.characters,self.char_num))
        img = image.generate_image(captcha_str)
        img.save(captcha_str + '.jpg')
</code></pre><p><strong>然后执行:</strong></p>
<pre><code>cd /home/ubuntu;
python
import generate_captcha
g = generate_captcha.generateCaptcha()
g.gen_test_captcha()
</code></pre><p><strong>执行结果：</strong></p>
<p>在 <em>/home/ubuntu</em> 目录下查看生成的验证码，jpg 格式的图片可以点击查看。</p>
<h2 id="-">验证码识别模型</h2>
<blockquote>
<p>任务时间：时间未知</p>
</blockquote>
<p>将验证码识别问题转化为分类问题，总共 62^4 种类型，采用 4 个 one-hot 编码分别表示 4 个字符取值。</p>
<h3 id="cnn-">cnn 验证码识别模型</h3>
<p>3 层隐藏层、2 层全连接层，对每层都进行 dropout。input——&gt;conv——&gt;pool——&gt;dropout——&gt;conv——&gt;pool——&gt;dropout——&gt;conv——&gt;pool——&gt;dropout——&gt;fully connected layer——&gt;dropout——&gt;fully connected layer——&gt;output</p>
<p><strong>示例代码：</strong></p>
<p>现在您可以在 <em>/home/ubuntu</em> 目录下<em>创建源文件 captcha_model.py</em>，内容可参考：</p>
<h5 id="-home-ubuntu-captcha_model-py">示例代码：/home/ubuntu/captcha_model.py</h5>
<pre><code>#!/usr/bin/python
# -*- coding: utf-8 -*

import tensorflow as tf
import math

class captchaModel():
    def __init__(self,
                 width = 160,
                 height = 60,
                 char_num = 4,
                 classes = 62):
        self.width = width
        self.height = height
        self.char_num = char_num
        self.classes = classes

    def conv2d(self,x, W):
        return tf.nn.conv2d(x, W, strides=[1, 1, 1, 1], padding='SAME')

    def max_pool_2x2(self,x):
        return tf.nn.max_pool(x, ksize=[1, 2, 2, 1],
                              strides=[1, 2, 2, 1], padding='SAME')

    def weight_variable(self,shape):
        initial = tf.truncated_normal(shape, stddev=0.1)
        return tf.Variable(initial)

    def bias_variable(self,shape):
        initial = tf.constant(0.1, shape=shape)
        return tf.Variable(initial)

    def create_model(self,x_images,keep_prob):
        #first layer
        w_conv1 = self.weight_variable([5, 5, 1, 32])
        b_conv1 = self.bias_variable([32])
        h_conv1 = tf.nn.relu(tf.nn.bias_add(self.conv2d(x_images, w_conv1), b_conv1))
        h_pool1 = self.max_pool_2x2(h_conv1)
        h_dropout1 = tf.nn.dropout(h_pool1,keep_prob)
        conv_width = math.ceil(self.width/2)
        conv_height = math.ceil(self.height/2)

        #second layer
        w_conv2 = self.weight_variable([5, 5, 32, 64])
        b_conv2 = self.bias_variable([64])
        h_conv2 = tf.nn.relu(tf.nn.bias_add(self.conv2d(h_dropout1, w_conv2), b_conv2))
        h_pool2 = self.max_pool_2x2(h_conv2)
        h_dropout2 = tf.nn.dropout(h_pool2,keep_prob)
        conv_width = math.ceil(conv_width/2)
        conv_height = math.ceil(conv_height/2)

        #third layer
        w_conv3 = self.weight_variable([5, 5, 64, 64])
        b_conv3 = self.bias_variable([64])
        h_conv3 = tf.nn.relu(tf.nn.bias_add(self.conv2d(h_dropout2, w_conv3), b_conv3))
        h_pool3 = self.max_pool_2x2(h_conv3)
        h_dropout3 = tf.nn.dropout(h_pool3,keep_prob)
        conv_width = math.ceil(conv_width/2)
        conv_height = math.ceil(conv_height/2)

        #first fully layer
        conv_width = int(conv_width)
        conv_height = int(conv_height)
        w_fc1 = self.weight_variable([64*conv_width*conv_height,1024])
        b_fc1 = self.bias_variable([1024])
        h_dropout3_flat = tf.reshape(h_dropout3,[-1,64*conv_width*conv_height])
        h_fc1 = tf.nn.relu(tf.nn.bias_add(tf.matmul(h_dropout3_flat, w_fc1), b_fc1))
        h_fc1_drop = tf.nn.dropout(h_fc1, keep_prob)

        #second fully layer
        w_fc2 = self.weight_variable([1024,self.char_num*self.classes])
        b_fc2 = self.bias_variable([self.char_num*self.classes])
        y_conv = tf.add(tf.matmul(h_fc1_drop, w_fc2), b_fc2)

        return y_conv
</code></pre><h3 id="-cnn-">训练 cnn 验证码识别模型</h3>
<p>每批次采用 64 个训练样本，每 100 次循环采用 100 个测试样本检查识别准确度，当准确度大于 99% 时，训练结束，采用 GPU 需要 5-6 个小时左右，CPU 大概需要 20 个小时左右。</p>
<p>注：作为实验，你可以通过调整 <code>train_captcha.py</code> 文件中 <code>if acc &gt; 0.99:</code> 代码行的准确度节省训练时间(比如将 0.99 为 0.01)；同时，我们已经通过长时间的训练得到了一个训练集，可以通过如下命令将训练集下载到本地。</p>
<pre><code>wget http://tensorflow-1253902462.cosgz.myqcloud.com/captcha/capcha_model.zip
unzip capcha_model.zip
</code></pre><p>现在您可以在 <em>/home/ubuntu</em> 目录下<em>创建源文件 train_captcha.py</em>，内容可参考：</p>
<h5 id="-home-ubuntu-train_captcha-py">示例代码：/home/ubuntu/train_captcha.py</h5>
<pre><code>#!/usr/bin/python

import tensorflow as tf
import numpy as np
import string
import generate_captcha
import captcha_model

if __name__ == '__main__':
    captcha = generate_captcha.generateCaptcha()
    width,height,char_num,characters,classes = captcha.get_parameter()

    x = tf.placeholder(tf.float32, [None, height,width,1])
    y_ = tf.placeholder(tf.float32, [None, char_num*classes])
    keep_prob = tf.placeholder(tf.float32)

    model = captcha_model.captchaModel(width,height,char_num,classes)
    y_conv = model.create_model(x,keep_prob)
    cross_entropy = tf.reduce_mean(tf.nn.sigmoid_cross_entropy_with_logits(labels=y_,logits=y_conv))
    train_step = tf.train.AdamOptimizer(1e-4).minimize(cross_entropy)

    predict = tf.reshape(y_conv, [-1,char_num, classes])
    real = tf.reshape(y_,[-1,char_num, classes])
    correct_prediction = tf.equal(tf.argmax(predict,2), tf.argmax(real,2))
    correct_prediction = tf.cast(correct_prediction, tf.float32)
    accuracy = tf.reduce_mean(correct_prediction)

    saver = tf.train.Saver()
    with tf.Session() as sess:
        sess.run(tf.global_variables_initializer())
        step = 0
        while True:
            batch_x,batch_y = next(captcha.gen_captcha(64))
            _,loss = sess.run([train_step,cross_entropy],feed_dict={x: batch_x, y_: batch_y, keep_prob: 0.75})
            print ('step:%d,loss:%f' % (step,loss))
            if step % 100 == 0:
                batch_x_test,batch_y_test = next(captcha.gen_captcha(100))
                acc = sess.run(accuracy, feed_dict={x: batch_x_test, y_: batch_y_test, keep_prob: 1.})
                print ('###############################################step:%d,accuracy:%f' % (step,acc))
                if acc &gt; 0.99:
                    saver.save(sess,"capcha_model.ckpt")
                    break
            step += 1
</code></pre><p><strong>然后执行:</strong></p>
<pre><code>cd /home/ubuntu;
python train_captcha.py
</code></pre><p><strong>执行结果：</strong></p>
<pre><code>step:75173,loss:0.010555
step:75174,loss:0.009410
step:75175,loss:0.009978
step:75176,loss:0.008089
step:75177,loss:0.009949
step:75178,loss:0.010126
step:75179,loss:0.009584
step:75180,loss:0.012272
step:75181,loss:0.010157
step:75182,loss:0.009529
step:75183,loss:0.007636
step:75184,loss:0.009058
step:75185,loss:0.010061
step:75186,loss:0.009941
step:75187,loss:0.009339
step:75188,loss:0.009685
step:75189,loss:0.009879
step:75190,loss:0.007799
step:75191,loss:0.010866
step:75192,loss:0.009838
step:75193,loss:0.010931
step:75194,loss:0.012859
step:75195,loss:0.008747
step:75196,loss:0.009147
step:75197,loss:0.009351
step:75198,loss:0.009746
step:75199,loss:0.010014
step:75200,loss:0.009024
###############################################step:75200,accuracy:0.992500
</code></pre><h3 id="-cnn-">测试 cnn 验证码识别模型</h3>
<p><strong>示例代码：</strong></p>
<p>现在您可以在 <em>/home/ubuntu</em> 目录下<em>创建源文件 predict_captcha.py</em>，内容可参考：</p>
<h5 id="-home-ubuntu-predict_captcha-py">示例代码：/home/ubuntu/predict_captcha.py</h5>
<pre><code>#!/usr/bin/python

from PIL import Image, ImageFilter
import tensorflow as tf
import numpy as np
import string
import sys
import generate_captcha
import captcha_model

if __name__ == '__main__':
    captcha = generate_captcha.generateCaptcha()
    width,height,char_num,characters,classes = captcha.get_parameter()

    gray_image = Image.open(sys.argv[1]).convert('L')
    img = np.array(gray_image.getdata())
    test_x = np.reshape(img,[height,width,1])/255.0
    x = tf.placeholder(tf.float32, [None, height,width,1])
    keep_prob = tf.placeholder(tf.float32)

    model = captcha_model.captchaModel(width,height,char_num,classes)
    y_conv = model.create_model(x,keep_prob)
    predict = tf.argmax(tf.reshape(y_conv, [-1,char_num, classes]),2)
    init_op = tf.global_variables_initializer()
    saver = tf.train.Saver()
    gpu_options = tf.GPUOptions(per_process_gpu_memory_fraction=0.95)
    with tf.Session(config=tf.ConfigProto(log_device_placement=False,gpu_options=gpu_options)) as sess:
        sess.run(init_op)
        saver.restore(sess, "capcha_model.ckpt")
        pre_list =  sess.run(predict,feed_dict={x: [test_x], keep_prob: 1})
        for i in pre_list:
            s = ''
            for j in i:
                s += characters[j]
            print s
</code></pre><p><strong>然后执行:</strong></p>
<pre><code>cd /home/ubuntu;
python predict_captcha.py Kz2J.jpg
</code></pre><p><strong>执行结果：</strong></p>
<pre><code>Kz2J
</code></pre><p>注:因为实验时间的限制，你可能调整了准确度导致执行结果不符合预期，属于正常情况。</p>
<p>在训练时间足够长的情况下，你可以采用验证码生成器生成测试数据，cnn 训练出来的验证码识别模型还是很强大的，大小写的 z 都可以区分，甚至有时候人都无法区分，该模型也可以正确的识别。</p>
<h2 id="-">完成</h2>
<blockquote>
<p>任务时间：时间未知</p>
</blockquote>
<h3 id="-">恭喜，您已完成本实验内容</h3>
<p>您可进行更多 TensorFlow 的系列教程：</p>
<ul>
<li><em>TensorFlow - 相关 API</em></li>
<li><em>TensorFlow - 线性回归</em></li>
</ul>
<p>关于 TensorFlow 的更多资料可参考 <em>TensorFlow 官网 </em>。</p>
</div>