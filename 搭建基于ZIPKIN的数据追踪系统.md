<div class="lab-edi-doc"><h1 id="-zipkin-">搭建基于 ZIPKIN 的数据追踪系统</h1>
<h2 id="-java-">配置 Java 环境</h2>
<blockquote>
<p>任务时间：5min ~ 10min</p>
</blockquote>
<h3 id="-jdk">安装 JDK</h3>
<p>Zipkin 使用 Java8</p>
<pre><code>yum install java-1.8.0-openjdk* -y
</code></pre><p>安装完成后，查看是否安装成功：</p>
<pre><code>java -version
</code></pre><h2 id="-zipkin">安装 Zipkin</h2>
<blockquote>
<p>任务时间：10min ~ 20min</p>
</blockquote>
<h3 id="-">新建目录</h3>
<pre><code>mkdir -p /data/release/zipkin &amp;&amp; cd "$_"
</code></pre><h3 id="-zipkin">下载 Zipkin</h3>
<pre><code>wget -O zipkin.jar 'https://search.maven.org/remote_content?g=io.zipkin.java&amp;a=zipkin-server&amp;v=LATEST&amp;c=exec'
</code></pre><h3 id="-zipkin">启动 Zipkin</h3>
<pre><code>java -jar zipkin.jar
</code></pre><p>Zipkin 默认监听 9411 端口， 使用浏览器访问 <a target="_blank" href="http://&lt;您的 CVM IP 地址&gt;:9411" title="null">http://&lt;您的 CVM IP 地址&gt;:9411</a> 即可看到 Zipkin 自带的图形化界面。<sup>[<a href="#stage-2-step-3-text">访问遇到问题？</a>]</sup></p>
<p><a id="stage-2-step-3-text"></a></p>
<blockquote>
<p>请确认此CVM有外网IP且安全组已开通9411端口</p>
</blockquote>
<h2 id="-mysql-">配置 MySQL 数据持久化方案</h2>
<blockquote>
<p>任务时间：10min ~ 20min</p>
</blockquote>
<p>Zipkin 支持的持久化方案很多，如： Cassandra, MySQL, Elasticsearch。本实验使用 MySQL 5.7 作为数据持久化方案。</p>
<h3 id="-mysql-5-7">安装 MySQL 5.7</h3>
<p>使用 <code>Ctrl + C</code> 退出上个步骤的 Java 进程并下载 rmp 包</p>
<pre><code>wget http://dev.mysql.com/get/mysql57-community-release-el7-9.noarch.rpm
</code></pre><p>安装 rpm 包</p>
<pre><code>rpm -Uvh mysql57-community-release-el7-9.noarch.rpm
</code></pre><p>安装 MySQL</p>
<pre><code>yum install mysql-community-server -y
</code></pre><p>启动 MySQL 服务</p>
<pre><code>systemctl start mysqld.service
</code></pre><h3 id="-mysql-">设置 MySQL 密码</h3>
<p>获取 root 临时密码</p>
<pre><code>grep 'temporary password' /var/log/mysqld.log | awk '{print $NF}'
</code></pre><p>使用上一步的获得的临时密码登入 MySQL</p>
<pre><code>mysql -uroot -p
</code></pre><p>设置 MySQL 账户 root 密码<sup>[<a href="#stage-3-step-2-password">?</a>]</sup></p>
<pre><code>ALTER USER 'root'@'localhost' IDENTIFIED BY 'Xx$Zipkin2017';
</code></pre><p>退出 MySQL, 回到 Bash shell</p>
<pre><code>exit;
</code></pre><p><a id="stage-3-step-2-password"></a></p>
<blockquote>
<p>下面命令中的密码是教程为您自动生成的，为了方便实验的进行，不建议使用其它密码。如果设置其它密码，请把密码记住，在后续的步骤会使用到。</p>
</blockquote>
<h3 id="-zipkin-">初始化 Zipkin 数据库</h3>
<p>编写初始化脚本</p>
<p>请在 <code>/data/release/zipkin</code> 目录下<em>创建 zipkin_init.sql</em>，参考下面的内容。</p>
<h5 id="-data-release-zipkin-zipkin_init-sql">示例代码：/data/release/zipkin/zipkin_init.sql</h5>
<pre><code class="lang-sql">CREATE TABLE IF NOT EXISTS zipkin_spans (
  `trace_id_high` BIGINT NOT NULL DEFAULT 0 COMMENT 'If non zero, this means the trace uses 128 bit traceIds instead of 64 bit',
  `trace_id` BIGINT NOT NULL,
  `id` BIGINT NOT NULL,
  `name` VARCHAR(255) NOT NULL,
  `parent_id` BIGINT,
  `debug` BIT(1),
  `start_ts` BIGINT COMMENT 'Span.timestamp(): epoch micros used for endTs query and to implement TTL',
  `duration` BIGINT COMMENT 'Span.duration(): micros used for minDuration and maxDuration query'
) ENGINE=InnoDB ROW_FORMAT=COMPRESSED CHARACTER SET=utf8 COLLATE utf8_general_ci;

ALTER TABLE zipkin_spans ADD UNIQUE KEY(`trace_id_high`, `trace_id`, `id`) COMMENT 'ignore insert on duplicate';
ALTER TABLE zipkin_spans ADD INDEX(`trace_id_high`, `trace_id`, `id`) COMMENT 'for joining with zipkin_annotations';
ALTER TABLE zipkin_spans ADD INDEX(`trace_id_high`, `trace_id`) COMMENT 'for getTracesByIds';
ALTER TABLE zipkin_spans ADD INDEX(`name`) COMMENT 'for getTraces and getSpanNames';
ALTER TABLE zipkin_spans ADD INDEX(`start_ts`) COMMENT 'for getTraces ordering and range';

CREATE TABLE IF NOT EXISTS zipkin_annotations (
  `trace_id_high` BIGINT NOT NULL DEFAULT 0 COMMENT 'If non zero, this means the trace uses 128 bit traceIds instead of 64 bit',
  `trace_id` BIGINT NOT NULL COMMENT 'coincides with zipkin_spans.trace_id',
  `span_id` BIGINT NOT NULL COMMENT 'coincides with zipkin_spans.id',
  `a_key` VARCHAR(255) NOT NULL COMMENT 'BinaryAnnotation.key or Annotation.value if type == -1',
  `a_value` BLOB COMMENT 'BinaryAnnotation.value(), which must be smaller than 64KB',
  `a_type` INT NOT NULL COMMENT 'BinaryAnnotation.type() or -1 if Annotation',
  `a_timestamp` BIGINT COMMENT 'Used to implement TTL; Annotation.timestamp or zipkin_spans.timestamp',
  `endpoint_ipv4` INT COMMENT 'Null when Binary/Annotation.endpoint is null',
  `endpoint_ipv6` BINARY(16) COMMENT 'Null when Binary/Annotation.endpoint is null, or no IPv6 address',
  `endpoint_port` SMALLINT COMMENT 'Null when Binary/Annotation.endpoint is null',
  `endpoint_service_name` VARCHAR(255) COMMENT 'Null when Binary/Annotation.endpoint is null'
) ENGINE=InnoDB ROW_FORMAT=COMPRESSED CHARACTER SET=utf8 COLLATE utf8_general_ci;

ALTER TABLE zipkin_annotations ADD UNIQUE KEY(`trace_id_high`, `trace_id`, `span_id`, `a_key`, `a_timestamp`) COMMENT 'Ignore insert on duplicate';
ALTER TABLE zipkin_annotations ADD INDEX(`trace_id_high`, `trace_id`, `span_id`) COMMENT 'for joining with zipkin_spans';
ALTER TABLE zipkin_annotations ADD INDEX(`trace_id_high`, `trace_id`) COMMENT 'for getTraces/ByIds';
ALTER TABLE zipkin_annotations ADD INDEX(`endpoint_service_name`) COMMENT 'for getTraces and getServiceNames';
ALTER TABLE zipkin_annotations ADD INDEX(`a_type`) COMMENT 'for getTraces';
ALTER TABLE zipkin_annotations ADD INDEX(`a_key`) COMMENT 'for getTraces';
ALTER TABLE zipkin_annotations ADD INDEX(`trace_id`, `span_id`, `a_key`) COMMENT 'for dependencies job';

CREATE TABLE IF NOT EXISTS zipkin_dependencies (
  `day` DATE NOT NULL,
  `parent` VARCHAR(255) NOT NULL,
  `child` VARCHAR(255) NOT NULL,
  `call_count` BIGINT
) ENGINE=InnoDB ROW_FORMAT=COMPRESSED CHARACTER SET=utf8 COLLATE utf8_general_ci;

ALTER TABLE zipkin_dependencies ADD UNIQUE KEY(`day`, `parent`, `child`);
</code></pre>
<p>登录 Mysql</p>
<pre><code>mysql -u root --password='Xx$Zipkin2017'
</code></pre><p>创建 Zipkin 数据库</p>
<pre><code>create database zipkin;
</code></pre><p>切换数据库</p>
<pre><code>use zipkin;
</code></pre><p>初始化表及索引</p>
<pre><code>source /data/release/zipkin/zipkin_init.sql
</code></pre><p>执行以下命令会看到<code>zipkin_annotations</code>, <code>zipkin_dependencies</code>, <code>zipkin_spans</code> 三张数据表，说明初始化成功了</p>
<pre><code>show tables;
</code></pre><p>退出 MySQL, 回到 Bash shell</p>
<pre><code>exit
</code></pre><h3 id="-zipkin">启动 Zipkin</h3>
<p>注： 此处默认使用实验生成的密码</p>
<pre><code>cd /data/release/zipkin
STORAGE_TYPE=mysql MYSQL_HOST=localhost MYSQL_TCP_PORT=3306 MYSQL_DB=zipkin MYSQL_USER=root MYSQL_PASS='Xx$Zipkin2017' \
nohup java -jar zipkin.jar &amp;
</code></pre><h2 id="-demo">创建具有数据上报能力的Demo</h2>
<blockquote>
<p>任务时间：10min ~ 20min</p>
</blockquote>
<h3 id="-nodejs-">搭建 NodeJS 环境</h3>
<pre><code>curl --silent --location https://rpm.nodesource.com/setup_8.x | sudo bash -
yum install nodejs -y
</code></pre><h3 id="-demo-">创建Demo目录</h3>
<p>创建<em>/data/release/service_a</em>目录</p>
<pre><code>mkdir -p /data/release/service_a &amp;&amp; cd "$_"
</code></pre><h3 id="-npm-">使用 NPM 安装相关依赖</h3>
<p>请在 <code>/data/release/service_a</code> 目录下<em>创建并编辑 package.json</em>，参考下面的内容。</p>
<h5 id="-data-release-service_a-package-json">示例代码：/data/release/service_a/package.json</h5>
<pre><code class="lang-json">{
  "name": "service_a",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {},
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.15.3",
    "zipkin": "^0.7.2",
    "zipkin-instrumentation-express": "^0.7.2",
    "zipkin-transport-http": "^0.7.2"
  }
}
</code></pre>
<p>安装相关依赖</p>
<pre><code>npm install
</code></pre><h3 id="-app-js">创建并编辑 app.js</h3>
<p>请在 <code>/data/release/service_a</code> 目录下<em>创建 app.js</em>，参考下面的内容。</p>
<h5 id="-data-release-service_a-app-js">示例代码：/data/release/service_a/app.js</h5>
<pre><code class="lang-javascript">const express = require('express');
const {Tracer, ExplicitContext, BatchRecorder} = require('zipkin');
const {HttpLogger} = require('zipkin-transport-http');
const zipkinMiddleware = require('zipkin-instrumentation-express').expressMiddleware;

const ctxImpl = new ExplicitContext();
const recorder = new BatchRecorder({
    logger: new HttpLogger( {
        endpoint: 'http://127.0.0.1:9411/api/v1/spans'
    })
});

const tracer = new Tracer({ctxImpl, recorder});

const app = express();

app.use(zipkinMiddleware({
  tracer,
  serviceName: 'service-a'
}));

app.use('/', (req, res, next) =&gt; {
    res.send('hello world');
});

app.listen(3000, () =&gt; {
  console.log('service-a listening on port 3000!')
});
</code></pre>
<h3 id="-">启动服务</h3>
<pre><code>node app.js
</code></pre><p>该服务监听 3000 端口， 使用浏览器访问 <a target="_blank" href="http://&lt;您的 CVM IP 地址&gt;:3000" title="null">http://&lt;您的 CVM IP 地址&gt;:3000</a> 后，看到“hello world” 的文本字样说明服务已经正常工作。<sup>[<a href="#stage-4-step-5-text">访问遇到问题？</a>]</sup></p>
<p><a id="stage-4-step-5-text"></a></p>
<blockquote>
<p>请确认此CVM有外网IP且安全组已开通3000端口</p>
</blockquote>
<h2 id="-">部署完成</h2>
<blockquote>
<p>任务时间：3min ~ 5min</p>
</blockquote>
<h3 id="-">查看采集到的追踪数据</h3>
<p>使用浏览器访问 <a target="_blank" href="http://&lt;您的 CVM IP 地址&gt;:9411" title="null">http://&lt;您的 CVM IP 地址&gt;:9411</a> 即可看到刚才访问产生的追踪数据。</p>
<p>至此，本入门教程已结束，而 Zipkin 的学习只是一个开始，如有兴趣，可尝试搭建一个基于 Kafka + Zookeeper + Elasticsearch 的分布式服务。</p>
<h3 id="-">大功告成</h3>
<p>恭喜您已经完成了搭建基于 ZIPKIN 的数据追踪系统的学习，您可以留用或者<em>购买 Linux 版本的 CVM</em> 继续学习。</p>
</div>