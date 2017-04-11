# 一分钟起步
如果你想在一分钟内上手Zan PHP框架，我们推荐你试试我们的脚手架工具：zan-installer。


## 安装
首先在使用zan-installer之前你需要安装它，推荐的安装方式是通过 [Composer](http://getcomposer.org) 进行安装。zan-installer托管在 [Composer官方源](packagist.org) 的地址是： [youzan/zan-installer](https://packagist.org/packages/youzan/zan-installer) 。

```bash
# 如果你没有安装 Composer，你还得先安装它
curl -sS https://getcomposer.org/installer | php
```
Composer安装相关详见 [Composer Getting Started](https://getcomposer.org/doc/00-intro.md)


接下来让我们用 Composer 来安装 zan-installer 吧！
```bash
composer global require youzan/zan-installer
```

如果安装很慢，可以换成国内的镜像源，再安装
```bash
composer config -g repo.packagist composer https://packagist.phpcomposer.com
```

## 更新
你可以通过如下命令更新 zan-installer：

```bash
composer global update
```


## 使用
先确认一下PATH里是否包含了~/.composer/vendor/bin。
如果没有，先将这个路径加入到PATH里，修改.bashrc或者.zshrc
```bash
export PATH="$PATH:~/.composer/vendor/bin"
```

zan-installer安装完成之后，你就可以在命令行中，输入 `zan` 使用它了！

```bash
~/Sites zan↵
   __    __
  /\ \  /\ \
  \ `\`\\/'/ ___   __  __  ____      __      ___
   `\ `\ /' / __`\/\ \/\ \/\_ ,`\  /'__`\  /' _ `\
     `\ \ \/\ \L\ \ \ \_\ \/_/  /_/\ \L\.\_/\ \/\ \
       \ \_\ \____/\ \____/ /\____\ \__/.\_\ \_\ \_\
        \/_/\/___/  \/___/  \/____/\/__/\/_/\/_/\/_/
Create a new ZanPhp application.
Which type application would you create? (use <space> to select)
❯ ● HTTP
  ○ TCP
Your application name: (ex: zanphp-demo) demo
Your application namespace: (ex: zanphp/zanhttp) youzan/demo
Please input a output directory:
~/Sites
Downloading the source code archive ...
Extracting archive ...
Congratulations, your application has been generated to the 
following directory.
~/Sites/demo/
See ~/Sites/demo/README.md for information on how to run.
Composer installing...
Loading composer repositories with package information
Updating dependencies (including require-dev)
  - Installing zanphp/zan (dev-master o1o0x2x4)
    Cloning b6d8d443a7a3545a3d1796b39e54fcbc2d276a10

Writing lock file
Generating autoload files
```






