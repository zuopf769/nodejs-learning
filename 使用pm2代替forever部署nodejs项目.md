
[pm2官方文档](http://pm2.keymetrics.io/docs/usage/quick-start/)

简单教程

首先需要安装pm2：

npm install -g pm2
运行：

pm2 start app.js
初次安装并运行，会有一个高大上的界面：


高大上的界面
直接我们介绍过forever，那么pm2与forever相比较有哪些高大上的功能呢？我们看一下对比表格：

Feature	Forever	PM2
Keep Alive	✔	✔
Coffeescript	✔	
Log aggregation		✔
API		✔
Terminal monitoring		✔
Clustering		✔
JSON configuration		✔
我们可以很直观的看出，pm2相比较Forever，功能更加强大一些。

查看运行状态

我们可以通过简单的命令查看应用的运行状态：

pm2 list
效果如下：


运行状态
ANodeBlog应用正在运行，pid为31480，并且占用内存为89.113 MB。
追踪资源运行情况

pm2 monit
会看到应用资源的实时运行情况


实时运行情况
查看应用详细部署状态

如果我们想要查看一个应用详细的运行状态，比如ANodeBlog的状态，可以运行：

pm2 describe 3
“3”是指App Id。
结果如下：


详细运行状态
查看日志

pm2 logs
系统会打印出详细的logs。

重启应用

pm2 restart appId
停止应用

想要终止应用，只需要运行：

pm2 stop app.js
强健的API

在项目中运行：

pm2 web
然后浏览器访问http://localhost:9615 你会有惊喜！

预定义运行配置文件

我们可以预定义一个配置文件，然后制定运行这个配置文件，比如我们定义一个文件process.json，内容如下：

{
  "apps": [
    {
      "name": "ANodeBlog",
      "script": "bin/www",
      "watch": "../",
      "log_date_format": "YYYY-MM-DD HH:mm Z"
    }
  ]
}
然后可以通过

pm2 start process.json
运行这个App。

总结

常用命令总结如下：

安装pm2
npm install -g pm2
启动应用
pm2 start app.js
列出所有应用
pm2 list
查看资源消耗
pm2 monit
查看某一个应用状态
pm2 describe [app id]
查看所有日志
pm2 logs
重启应用
pm2 restart [app id]
停止应用
pm2 stop [app id]
开启api访问
pm2 web
更多pm2内容请参考官方文档：http://pm2.keymetrics.io/docs/usage/quick-start
