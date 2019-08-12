# 1、tomcat8.5优化配置
JAVA_OPTS="-server -Xms1024M -Xmx1024M -Xss512k -XX:+AggressiveOpts -XX:+UseBiasedLocking -XX:+DisableExplicitGC -XX:MaxTenuringThreshold=15 -XX:+UseConcMarkSweepGC -XX:+UseParNewGC   -XX:+CMSParallelRemarkEnabled -XX:LargePageSizeInBytes=128m -XX:+UseFastAccessorMethods -XX:+UseCMSInitiatingOccupancyOnly -Djava.awt.headless=true"

# 2、Tomcat连接参数的优化，主要是针对吞吐量做优化
```markdown
<Connector port="8080"
protocol="org.apache.coyote.http11.Http11NioProtocol"
maxHttpHeaderSize="8192"
maxThreads="1000"
minSpareThreads="100"
maxSpareThreads="1000"
minProcessors="100"
maxProcessors="1000"
enableLookups="false"
compression="on"
compressionMinSize="1024"
noCompressionUserAgents="gozilla, traviata"
compressableMimeType="text/html,text/xml,text/javascript,text/css,text/plain"
connectionTimeout="25000"
URIEncoding="utf-8"
acceptCount="1000"
redirectPort="8443"
disableUploadTimeout="true" URIEncoding="UTF-8" />
```
# 3、隐藏tomcat本版号
```bash
cd apache-tomcat-x.x.x/lib/
unzip catalina.jar
```

* 改成如下

```bash
vim org/apache/catalina/util/ServerInfo.properties
server.info=Apache Tomcat
server.number=0.0.0.0
server.built=Nov 7 2019 20:05:27 UTC
```
* 将修改后的信息压缩回jar包

```bash
jar uvf catalina.jar org/apache/catalina/util/ServerInfo.properties
```
