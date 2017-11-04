
## 背景

在使用 Nodejs 过程中，有很多包都支持全局安装，提供一个命令，然后在命令行我们就可以完成一些任务。有时候我们也需要开发这样的命令工具。在Node.js 中发现弄个命令行工具特别轻松，我来学习如何使用 Node.js 生成自己的command命令，在未来的项目中方便自己。

但是本文是基础知识普及文章，着重讲解开发命令行工具中两个非常重要的概念package.json的bin属性和npm link。


## package.json的bin属性


helloworld.js代码如下：

```
#!/usr/bin/env node

console.log('hello world');
```

上面的 #!/usr/bin/env node 被成为 shebang ，表示用后面的路径所示的程序来执行当前文件夹。



package.json中的bin选项用来指定各个内部命令对应的可执行文件的位置。

```
"bin": {
  "helloworld": "./helloworld.js"
},
```

上面代码指定，helloworld 命令对应的可执行文件为当前目录下的 helloworld.js。Npm会寻找这个文件，在node_modules/.bin/目录下建立符号链接。


在上面的例子中，./helloworld.js会建立符号链接npm_modules/.bin/helloworld。由于node_modules/.bin/目录会在运行时加入系统的PATH变量，因此在运行npm时，就可以不带路径，直接通过命令来调用这些脚本。

下面我们看如何建立符号链接。


## npm link


### 1. 全局模式链接


在在package.json文件路径下，执行npm link

```
zuopengfei@zuopengfeideMacBook-Pro  ~/work/demo/commad  npm link
npm WARN commad@1.0.0 No description
npm WARN commad@1.0.0 No repository field.
/Users/zuopengfei/.nvm/versions/node/v6.11.3/bin/helloworld -> /Users/zuopengfei/.nvm/versions/node/v6.11.3/lib/node_modules/commad/helloworld.js
/Users/zuopengfei/.nvm/versions/node/v6.11.3/lib/node_modules/commad -> /Users/zuopengfei/work/demo/commad
```

此时说明已经成功了。

上面两句话表示什么？

创建连接到`/Users/zuopengfei/.nvm/versions/node/v6.11.3/lib/node_modules`

那么`commad`的模块将被链接到`$PREFIX/lib/node_modules`下，就像我的机器上`$PREFIX` 指到 `/Users/zuopengfei/.nvm/versions/node/v6.11.3` ，那么`/Users/zuopengfei/.nvm/versions/node/v6.11.3/bin/helloworld`将会链接到`/Users/zuopengfei/.nvm/versions/node/v6.11.3/lib/node_modules/commad/helloworld.js`下。执行脚本`bin/helloworld.js`被链接到` /Users/zuopengfei/.nvm/versions/node/v6.11.3/lib/node_modules/commad/helloworld.js`上。

其实上面是把`helloworld`链接到全局模式下。
> 更加正确的解释是npm link 会自动添加全局的 symbolic link ，然后就可以使用自己的命令了。

### 2. 局部链接

假设我们开发了一个模块叫test，然后我们在test-example里引用这个模块 ，每次test模块的变动我们都需要反映到 test-example 模块里。不要担心，有了 npm link 命令一切变的非常容易。

接下来我们需要把 test 引用到 test-example 项目中来：

cd ~/work/node/test-example # 进入test-example模块目录
npm link test # 把全局模式的模块链接到本地
npm link test 命令会去 $PREFIX/lib/node_modules 目录下查找名叫 test 的模块，找到这个模块后把 $PREFIX/lib/node_modules/test 的目录链接到 ~/work/node/test-example/node_modules/test 这个目录上来。

现在任何 test 模块上的改动都会直接映射到 test-example 上来。


再比如假设我们开发很多应用，每个应用都用到 Coffee-script ：

npm install coffee-script -g # 全局模式下安装coffee-script
cd ~/work/node/test # 进入开发目录
npm link coffee-script # 把全局模式的coffee-script模块链接到本地的node_modules下
cd ../test-example # 进入另外的一个开发目录
npm link coffee-script # 把全局模式的coffee-script模块链接到本地
npm update coffee-script -g # 更新全局模式的coffee-script，所有link过去的项目同时更新了。











