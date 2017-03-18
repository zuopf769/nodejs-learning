# linux 下部署nodejs（两种方式）

## 关于在node在linux的部署我认为主要有三种方式，

```
1. 第一种就是自己下载源码，手动编译二进制，即是部署过程。

2. 第二种方法 直接下载二进制文件解压即可。

3. 第三种方式，使用yum install node或者apt-get install node安装（在linux下 貌似默认源中没有node的程序，这种方式有缺点，安装后的程序版本可能不是最新版的，不推荐这种方式安装）

```

### 源码编译安装(安装时速度极其慢，不推荐)
源码编译安装可以参考这个[微博](http://blog.csdn.net/zhaoweitco/article/details/12677089)

### 二进制文件的部署安装（安装速度快，只需配置环境变量即可）

1. 首先下载NodeJS的二进制文件，可以从[淘宝镜像](https://npm.taobao.org/mirrors/node) 下载相应版本即可

2. 下载后将安装包移动到要安装到的文件夹下，根据个人喜好设置即可。这里我放在了/home/kun/mysofltware/ 下面,依次执行如下命令，可看到
   ```
   cd  /home/kun/mysofltware/
   ls
   ```
   ![](http://images.cnitblog.com/blog/171505/201402/210913034875851.png)
   
   解压到当前文件夹下运行 
   tar zxvf node-v0.10.26-linux-x64.tar.gz

   进入解压后的目录bin目录下，执行ls会看到两个文件node,npm. 然后执行./node -v ，如果显示出版本号说明我们下载的程序包是没有问题的。依次运行如下三条命令
   
   ```
   cd node-v0.10.26-linux-x64/bin
   ls
   ./node -v
   ```
    ![](http://images.cnitblog.com/blog/171505/201402/210917347493824.png)
 
    因为 /home/kun/mysofltware/node-v0.10.26-linux-x64/bin这个目录是不在环境变量中的，所以只能到该目录下才能node的程序。如果在其他的目录下执行node命令的话 ，必须通过绝对路径访问才可以的。

    如果要在任意目录可以访问的话，需要将node 所在的目录，添加PATH环境变量里面，或者通过软连接的形式将node和npm链接到系统默认的PATH目录下的一个，以下别介绍

　　#### 软连接方式

在终端执行echo $PATH可以获取PATH变量包含的内容，系统默认的PATH环境变量包括/usr/local/bin:/usr/bin:/bin:/usr/local/sbin:/usr/sbin:/sbin: ，冒号为分隔符。所以我们可以将node和npm链接到/usr/local/bin 目录下如下执行

1
2
ln -s /home/kun/mysofltware/node-v0.10.26-linux-x64/bin/node /usr/local/bin/node
ln -s /home/kun/mysofltware/node-v0.10.26-linux-x64/bin/npm /usr/local/bin/npm
　　通过如此，就可以访问Node了，同时node部署也已经完毕。

 

环境变量配置。

在node目录下执行pwd 获取node所在的目录，要把这个目录添加到PATH环境变量



执行su 输入密码切换到root用户。

vi /etc/profile


(如果不熟悉vi的，centos还有个方便的类似记事本的东东。gedit执行gedit /etc/profile可以打开进行编辑）


在vi 环境下 点击 i 进入插入状态，在export PATH的上一行添加如下内容 (环境变量中的内容 是以冒号分割的)
PATH=$PATH:/home/kun/mysofltware/node-v0.10.26-linux-x64/bin
 

编辑完成后按Esc键 然后输入 :wq 按回车保存退出。



 

 

退出vi ，执行

source /etc/profile 可以是变量生效，

然后执行 echo $PATH ，看看输出内容是否包含自己添加的内容



然后到任意目录下去执行一次执行node -v   npm -v 



 

ok 搞定了。

 

 需要注意的是,在我的安装过程中，通过source /etc/profile，只是让变量临时生效了，如果此时我在开一个终端的 话运行node会提示找不到命令，这个问题 重启或者注销之后得到了解决，我记得之前玩Ubuntu的时候 是没有这个问题的。看来linux知识还是欠缺啊。


