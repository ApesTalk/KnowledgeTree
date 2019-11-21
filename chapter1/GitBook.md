# GitBook常用操作

- 初始化书籍目录
```
gitbook init
```

会生成README.md和SUMMARY.md文件。SUMMARY.md 是书籍的目录结构，gitbook会根据SUMMARY.md中的配置生成对应目录结构。

- 编译书籍
```
gitbook serve
```
``gitbook serve`` 命令实际上会首先调用 ``gitbook build`` 编译书籍，完成以后会打开一个 web 服务器，监听在本地的 4000 端口。编译后就可以用 ``http://127.0.0.1:4000/`` 预览效果了。

参考：

- [GitBook 简明教程](http://www.chengweiyang.cn/)
- [GitBook搭建](https://liangjunrong.github.io/other-library/Markdown-Websites/GitBook/GitBook-study.html)

