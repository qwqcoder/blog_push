---
tags: hexo
title: hexo
---



# 关于配置文件

在 `hexo` 的根目录创建一个文件 `_config.butterfly.yml`，并把主题目录的 `_config.yml `内容复制到` _config.butterfly.yml` 中。( 注意: 复制的是主題的 `_config.yml` ，而不是 `hexo` 的 `_config.yml`)

> **注意**： 不能把主題目录的 `_config.yml `删掉

注意： 以后只需要在 `_config.butterfly.yml` 进行配置就行。
如果使用了 `_config.butterfly.yml`， 配置主题的 `_config.yml` 将不会有效果。

`Hexo`会自动合并主题中的 `_config.yml` 和 `_config.butterfly.yml` 里的配置，如果存在同名配置，会使用 `_config.butterfly.yml` 的配置，其优先度较高。



# 博客撰写步骤

**第一步**: 创建 `.md` 文件

+ 方法一: `cd` 进入 `hexo` 根目录, 在 `Git Bash Here` 中执行命令: `hexo new 'blog-name'` , 此时 `hexo` 会在 `\source\_posts` 下生成名为 `blog-name` 的 `.md` 文件, 用这个命令可以自动生成文件, 其中包含默认内容如下:

  ```yaml
  title: blog-name
  date: 创建时间
  tags: 标签
  ```

+ 方法二: 也可以手动创建 `.md` 文件

**第二步**: 编写 `md` 文档内容并保存

+ 使用 `markdown` 语法, 撰写博客内容即可

**第三步**: 清理然后生成, 然后推送到远端仓库

+ `hexo` 根目录下进入 `bash` 终端, 依次输入以下命令:

  ```shell
  hexo clean
  hexo g
  hexo d
  ```

  

## 背景图片设置

+ 在 `themes\hexo-theme-matery\source\css\matery.css` 路径下,  找到 `body` 修改样式如下

  ```css
  body {
      /* background-color: #eaeaea; */
      background: linear-gradient(60deg, rgba(255, 165, 150, 0.5) 5%, rgba(0, 228, 255, 0.35)) 0% 0% / cover, url("https://ae01.alicdn.com/kf/H18a4b998752a4ae68b8e85d432a5aef0l.png"), url("https://ae01.alicdn.com/kf/H21b5f6b8496141a1979a33666e1074d9x.jpg") 0px 0px;
      background-attachment: fixed;
      margin: 0;
      color: #7F95D1;
  }
  ```

# 目录栏设置

+ 目录栏的样式位于 `themes\hexo-theme-matery\layout\_partial\post-detail-toc.ejs` , 修改如下:

  ```css
  .toc-widget {
        width: 345px;
        padding-left: 20px;
        background-color: rgb(255, 255, 255,0.7);
        border-radius: 10px;
        box-shadow: 0 10px 35px 2px rgba(0, 0, 0, .15), 0 5px 15px rgba(0, 0, 0, .07), 0 2px 5px -5px rgba(0, 0, 0, .1) !important;
    }
  ```

  

# 待办

- [ ] 友情链接