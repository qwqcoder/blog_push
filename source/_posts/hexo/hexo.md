---
title: blog-name
date: 
tags: 
	- tag1
	- tag2 # 一定注意空格
description: description
cover: 封面地址
top_img: 个性化顶部图
abbrlink: ab21860c
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
  tags: 
  	- tag1
  	- tag2 # 一定注意空格
  description: description
  cover: 封面地址
  top_img: 个性化顶部图
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

  

# 优化博客链接配置

`Hexo` 文章链接默认的生成规则是：`:year/:month/:day/:title`，是按照年、月、日、标题来生成的。

这样一来, 当我们修改了文章的日期或者标题, 链接很可能就失效了, 特别是文章标题包含中文时, 被转译为 URL 编码后, 链接就特别长😣😣😣

**解决方案**

+ 安装插件

  ```bash
  npm install hexo-abbrlink --save
  ```

+ 修改 `_config.yml` 配置文件

  ```yml
  # permalink: :year/:month/:day/:title/
  permalink: :year/:month/:day/:abbrlink.html
  ```




# 个性化blog图片

+ 修改主题路径下的 `_config.yml` 配置文件

```yml
cover:
  # display the cover or not (是否顯示文章封面)
  index_enable: true
  aside_enable: true
  archives_enable: true
  # the position of cover in home page (封面顯示的位置)
  # left/right/both
  position: right
  # When cover is not set, the default cover is displayed (當沒有設置cover時，默認的封面顯示)
  default_cover:
    # - https://i.loli.net/2020/05/01/gkihqEjXxJ5UZ1C.jpg
```

| 参数            | 解释                                                         |
| --------------- | ------------------------------------------------------------ |
| index_enable    | 主页是否显示文章封面                                         |
| aside_enable    | 侧栏是否显示                                                 |
| archives_enable | 归档页面是否显示文章封面                                     |
| position        | 主页卡片文章封面显示位置<br />- left：全部显示左边<br />- right：全部显示右边<br />- both：左右交替 |
| default_cover   | 默认的图片配置                                               |

```yml	
default_cover:
  - https://jsd.012700.xyz/gh/jerryc127/CDN@latest/cover/default_bg.png
  - https://jsd.012700.xyz/gh/jerryc127/CDN@latest/cover/default_bg2.png
  - https://jsd.012700.xyz/gh/jerryc127/CDN@latest/cover/default_bg3.png
```



# 待办

+ 检索功能
+ 评论功能