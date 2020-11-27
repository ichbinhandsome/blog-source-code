---
title: 解决搭建博客中tags和categories页面404
date: 2020-08-05 19:05:38
tags: hexo
categories: hexo
---
#### 问题：
使用 `hexo + next-theme + Github Page`搭建博客在收尾阶段碰到一点小问题，菜单栏的`Tags`和 `Categories` 链接无法正常显示，而是直接跳转到404页面。

#### 解决方法：

<!-- more -->

* hexo 默认 `source` 文件中是没有 `tags` 和 `categories` 对应的page文件的，我们需要先手动进行创建，使用命令行:
```bash
   hexo new page categories
   hexo new page tags
```
* 接下来设置在`source` 目录下的`tags\index.md` 和 `categories\index.md` 文件。 在文件最上方的 `front-matter`（顶部代码片段）中加上对应的`type` 类型。 比如对于`tags\index.md` 加上``` type: tags```,  对于`categories\index.md`加上 ```type: categories```。
<br/>

* 测试： 使用 ```hexo new xxx``` 新建一个博客文章，在文件最上方的 `front-matter`（顶部代码片段）中加上对应的`tags` 和 `categories`, 比如：
    ```js
    tags: hello world
    categories: hello world
    ```
<br/>

* 使用 ``` hexo g ```重新生成静态文件，再重新部署 ``` hexo d```，打开博客，可以看到菜单中 `Tags` 和 `Categories` 成功生成。 



