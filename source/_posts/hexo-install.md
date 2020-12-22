---
title: Hexo安装设置与备份
---
# 安装环境
  1、这里针对Hexo，我们安装node12.20，过高版本的node会有问题。
  2、git
# 安装Hexo
  ``` bash
    npm install -g hexo-cli
  ```
# 初始化Hexo
在电脑的某个地方新建一个名为hexo的文件夹（名字可以随便取），比如我的是/home/tinker/Vscode/hexo，由于这个文件夹将来就作为你存放代码的地方，所以最好不要随便放。
  ``` bash
    hexo init #初始化Hexo
hexo g # 生成
hexo s # 启动服务
  ```
执行以上命令之后，hexo就会在public文件夹生成相关html文件，这些文件将来都是要提交到github去的。
hexo s是开启本地预览服务，打开浏览器访问 http://localhost:4000 即可看到内容，很多人会碰到浏览器一直在转圈但是就是加载不出来的问题，一般情况下是因为端口占用的缘故，因为4000这个端口太常见了。

# 安装主题
主题这里大家可以根据自己的喜好，从网上或者自己去编写你想要的主题，把主题文件放在themes文件中。根据主题作者的教程修改主题配置完成更换主题的过程，这里我主要记录一下自己更换主题的步骤，不同的主题，步骤不尽相同，所以在这里大家一定要根据自己下载的主题文件进行修改，切记生搬硬套！！！

这里感谢 观月真由理@amehime设计的主题shoka，十分好看。这里是地址：https://github.com/amehime/hexo-theme-shoka
中文wiki地址：https://shoka.lostyu.me/computer-science/note/theme-shoka-doc

把clone下来的主题放入themes中，从example文件拷贝_config.yml和_config.shoka.yml到hex文件下进行修改！
这里请参照作者的中文wiki安装必要插件！！！，不然可能有些功能会导致缺失。

Theme Shoka 依赖以下 Hexo 插件
|  插件名称    | npm地址  |  功能  | 依赖程度  |
|  :----:     | :----:  | :----:| :----:   |
|hexo-renderer-multi-markdown-it  | 链接 |
| 单元格  | 单元格 |

# 特别说明
本文主题安装说明均来自： Ruri Shimotsuki @優萌初華
链接地址： https://shoka.lostyu.me/computer-science/note/theme-shoka-doc/dependents
再次感谢優萌初華提供的主题和使用安装说明！
