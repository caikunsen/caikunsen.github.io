---
title: Hexo添加PDF插件功能
date: 2024-01-22 11:25:00
updated: 2024-01-22 11:25:00
tags:
  - Hexo
---
1、在根目录下执行命令
```
npm install --save hexo-pdf
```
2、`source`文件夹下创建pdf的文件夹，一定要以pdf命名的文件夹，后续pdf
文件都放这里面。然后在`_post`文件夹中的xxx.md直接使用 {% pdf /pdf/xxxx.pdf %}
例如：教程.pdf 放在pdf文件夹。`_post`文件夹下的md文件，教程.md的内容只写：{% pdf /pdf/教程.pdf %}
