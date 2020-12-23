---
title: Hexo安装设置与备份
categories:
- [Hexo]
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

# 备份
