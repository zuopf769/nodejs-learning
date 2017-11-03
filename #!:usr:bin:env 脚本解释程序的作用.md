#  #!/usr/bin/env 脚本解释程序的作用


## 1. 如何指定脚本的解释程序

在linux的一些bash的脚本，需在开头一行指定脚本的解释程序，如：


```
#!/usr/bin/env python

```

再如：

```
#!/usr/bin/env perl 
#!/usr/bin/env zimbu 
#!/usr/bin/env ruby 

```

但是有时候也用

```
#!/usr/bin/python 
#!/usr/bin/perl 

```

## 2. env到底有什么用？何时用这个呢？ 

脚本用env启动的原因，是因为脚本解释器在linux中可能被安装于不同的目录，env可以在系统的PATH目录中查找。同时，env还规定一些系统环境变量。 


如我系统里env程序执行后打印结果：

```
zuopengfei@localhost  ~  env
TERM_SESSION_ID=w0t2p0:441B42A8-34D3-44AC-8AD6-E8FE7CB4B82A
SSH_AUTH_SOCK=/private/tmp/com.apple.launchd.G2bkXnULlv/Listeners
Apple_PubSub_Socket_Render=/private/tmp/com.apple.launchd.NvPWQjuJJs/Render
COLORFGBG=12;8
ITERM_PROFILE=Default
XPC_FLAGS=0x0
LANG=zh_CN.UTF-8
PWD=/Users/zuopengfei
SHELL=/bin/zsh
TERM_PROGRAM_VERSION=3.1.4
TERM_PROGRAM=iTerm.app
PATH=/Users/zuopengfei/.nvm/versions/node/v6.11.3/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
COLORTERM=truecolor
TERM=xterm-256color
HOME=/Users/zuopengfei
TMPDIR=/var/folders/vl/gd4fj9_d5g3b7x864s1fzvf40000gn/T/
USER=zuopengfei
XPC_SERVICE_NAME=0
LOGNAME=zuopengfei
__CF_USER_TEXT_ENCODING=0x1F5:0x19:0x34
ITERM_SESSION_ID=w0t2p0:441B42A8-34D3-44AC-8AD6-E8FE7CB4B82A
SHLVL=1
OLDPWD=/Users/zuopengfei
ZSH=/Users/zuopengfei/.oh-my-zsh
PAGER=less
LESS=-R
LSCOLORS=Gxfxcxdxbxegedabagacad
LC_CTYPE=zh_CN.UTF-8
NVM_DIR=/Users/zuopengfei/.nvm
NVM_CD_FLAGS=-q
NVM_NODEJS_ORG_MIRROR=https://nodejs.org/dist
NVM_IOJS_ORG_MIRROR=https://iojs.org/dist
MANPATH=/Users/zuopengfei/.nvm/versions/node/v6.11.3/share/man:/usr/local/share/man:/usr/share/man:/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.13.sdk/usr/share/man:/Applications/Xcode.app/Contents/Developer/usr/share/man:/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/share/man
NVM_PATH=/Users/zuopengfei/.nvm/versions/node/v6.11.3/lib/node
NVM_BIN=/Users/zuopengfei/.nvm/versions/node/v6.11.3/bin
_=/usr/bin/env
```

可以用env来执行程序：

执行python

```
 ✘ zuopengfei@localhost  ~  env python
Python 2.7.10 (default, Feb  7 2017, 00:08:15)
[GCC 4.2.1 Compatible Apple LLVM 8.0.0 (clang-800.0.34)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>
 
```

也可以执行node，进入到node的命令行执行环境

```
 zuopengfei@localhost  ~  env node
>

```

而如果直接将解释器路径写死在脚本里，可能在某些系统就会存在找不到解释器的兼容性问题。有时候我们执行一些脚本时就碰到这种情况。


## 3. 其他参数

Probably the most common use of env is to find the correct interpreter 
for a script, when the interpreter may be in different directories on 
different systems. The following example will find the `perl’ interpreter by searching through the directories specified by PATH.


```
#!/usr/bin/env perl 

```


One limitation of that example is that it assumes the user's value for 
 PATH is set to a value which will find the interpreter you want to execute.  
 
 The -P option can be used to make sure a specific list of directo- 
 ries is used in the search for utility.  Note that the -S option is also 
 required for this example to work correctly. 
 
 
 ```
  #!/usr/bin/env -S -P/usr/local/bin:/usr/bin perl 
  
 ```
 
 The above finds `perl' only if it is in /usr/local/bin or /usr/bin.  That 
 could be combined with the present value of PATH, to provide more flexi- 
 bility.  Note that spaces are not required between the -S and -P options: 

 #!/usr/bin/env -S-P/usr/local/bin:/usr/bin:${PATH} perl 
 
 
这种写法主要是为了让你的程序在不同的系统上都能适用。 
不管你的perl是在/usr/bin/perl还是/usr/local/bin/perl，#!/usr/bin/env perl会自动的在你的用户PATH变量中所定义的目录中寻找perl来执行的。

还可以加上-P参数来指定一些目录去寻找perl这个程序，


```
#!/usr/bin/env -S -P/usr/local/bin:/usr/bin perl的作用就是在/usr/local/bin和/usr/bin目录下寻找perl。 
```
为了让程序更加的有可扩展性，可以写成

```
#!/usr/bin/env -S-P/usr/local/bin:/usr/bin:${PATH} perl，那么它除了在这两个目录寻找之外，还会在PATH变量中定义的目录中寻找。
```
 
 ### 4. 参考资料
 
 + [LINUX上的SHEBANG符号(#!)](http://smilejay.com/2012/03/linux_shebang/)
