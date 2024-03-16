---
title: hexo远端部署
tags:
  - hexo
  - nginx
  - git
layout: post
---

# 创建hexo项目

![image-20240315212549119](../img/hexo%E8%BF%9C%E7%AB%AF%E9%83%A8%E7%BD%B2.assets/image-20240315212549119.png)

`test` 就是准备好用来初始化 `hexo` 项目的根目录

## 步骤

1. 进入 `test` 执行 `hexo init`

   不出意外的话, `test` 路径下会出现以下内容

![image-20240315213900716](../img/hexo%E8%BF%9C%E7%AB%AF%E9%83%A8%E7%BD%B2.assets/image-20240315213900716.png)

​		这就是一个初始的 **hexo** 项目了

2. 进入 `test` 执行 `git init`

   将 `test` 路径初始化为一个 `git` 项目, 目的是方便后续的远端同步



# 安装theme

[关于 `butterfly` 主题的官方安装教程](https://butterfly.js.org/posts/21cfbf15/)

进入 `test\themes` 路径下执行 `git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git`

>  此时会将主题项目从 github 上克隆下来

成功后, `test\themes` 内容如下

![image-20240315214900875](../img/hexo%E8%BF%9C%E7%AB%AF%E9%83%A8%E7%BD%B2.assets/image-20240315214900875.png)

关于如何个性化配置主题, 这里不做记录

## 注意事项

+ `test` 根目录下, `.config.yml` 中 `themes` 参数需要配置为 `test\themes` 路径下, 你的主题所在的文件名这里就是 `hexo-theme-butterfly`

![image-20240315214958424](../img/hexo%E8%BF%9C%E7%AB%AF%E9%83%A8%E7%BD%B2.assets/image-20240315214958424.png)

+ `butterfly` 需要单独的解析 `.pug` 的插件, 在 `test` 下执行 `npm install hexo-renderer-pug hexo-renderer-stylus --save`



# 配置deploy插件

[hexo-deployer-git  官方教程](https://hexo.io/zh-cn/docs/one-command-deployment.html)

回到 `test` 根目录, 进入 `git bash` 执行 `npm install hexo-deployer-git --save`

## Git

1. 安装 [hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git)。

```bash
$ npm install hexo-deployer-git --save
```

2. 修改 `.config.yml` 配置

```yml
deploy:
  type: git
  repo: <repository url> #https://bitbucket.org/JohnSmith/johnsmith.bitbucket.io
  branch: [branch]
  message: [message]
```

| 参数      | 描述                                                         | 默认                                                         |
| :-------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| `repo`    | 库（Repository）地址                                         |                                                              |
| `branch`  | 分支名称                                                     | `gh-pages` (GitHub) `coding-pages` (Coding.net) `master` (others) |
| `message` | 自定义提交信息                                               | `Site updated: {{ now('YYYY-MM-DD HH:mm:ss') }}`)            |
| `token`   | 可选的令牌值，用于认证 repo。用 `$` 作为前缀从而从环境变量中读取令牌 |                                                              |



# 同步远端仓库

1. 修改 `.config.yml` 配置

   + 需要在 **github** 上创建名为 `test.github.io`, 这是专门用来推送 `hexo g` 生成的静态网页

   ```yml
   deploy:
     type: git
     branch: master
     repo: git@github.com:qwqcoder/test.github.io.git
   ```

2. 创建一个 **git** 仓库 `test_push.git`, 用于推送整个 `hexo` 项目(也就是 `test` 路径的所有内容)

   **步骤**:

   + `git add .`
   + `git add remote test https://github.com/qwqcoder/test_push.git`
   + `git push -u test master`

   ![image-20240315221448604](../img/hexo%E8%BF%9C%E7%AB%AF%E9%83%A8%E7%BD%B2.assets/image-20240315221448604.png)

3. **action** 自动话部署

   + 给 `test_push.git` 添加一个工作流配置文件

   + [参考教程](https://sanonz.github.io/2020/deploy-a-hexo-blog-from-github-actions/)

     ```yml
     name: CI
     
     on:
       push:
         branches:
           - master
     
     env: 
       GIT_USER: qwqcoder
       GIT_EMAIL: 1958448979@qq.com
       THEME_REPO: qwqcoder/blog_theme # blog_theme 是我用来专门存放主题项目的仓库
       THEME_BRANCH: master
       DEPLOY_REPO: qwqcoder/test.github.io
       DEPLOY_BRANCH: master
     
     jobs:
       build:
         name: Build on node ${{ matrix.node_version }} and ${{ matrix.os }}
         runs-on: ubuntu-latest
         strategy:
           matrix:
             os: [ubuntu-latest]
             node_version: [16.x]
     
         steps:
         # 将当前所在repository拉取到github的虚拟机容器工作目录下
           - name: Checkout
             uses: actions/checkout@v4
     
         # 因为hexo使用的theme主题是一个单独的git仓库项目, 给blog_push仓库push更新时会忽略其中嵌套的repo仓库
         # 所以事实上blog_push.git中是没有theme相关文件的, 需要额外的拉取, 同理放入容器的工作目录下
           - name: Checkout theme repo
             uses: actions/checkout@v4
             with:
               repository: ${{ env.THEME_REPO }}
               ref: ${{ env.THEME_BRANCH }}
               path: themes/hexo-theme-butterfly # 这里指定了拉取之后存放的路径
     
         # 安装node.js, 配置node环境
           - name: Use Node.js ${{ matrix.node_version }}
             uses: actions/setup-node@v4
             with:
               node-version: ${{ matrix.node_version }}
     
           - name: Configuration environment
             env:
               HEXO_DEPLOY_PRI: ${{secrets.HEXO_DEPLOY_PRI}}
             run: |
               sudo timedatectl set-timezone "Asia/Shanghai"
               mkdir -p ~/.ssh/
               echo "$HEXO_DEPLOY_PRI" > ~/.ssh/id_rsa
               chmod 600 ~/.ssh/id_rsa
               # 避免访问github出现问题, 添加到known_hosts
               ssh-keyscan github.com >> ~/.ssh/known_hosts
               ssh-keyscan 121.36.61.23 >> ~/.ssh/known_hosts
               git config --global user.name $GIT_USER
               git config --global user.email $GIT_EMAIL
               # cp _config.butterfly.yml themes/hexo-theme-butterfly/_config.yml
     
           - name: Install dependencies
             run: |
               npm install
     
           - name: Deploy hexo
             run: |
               npm run build
               npm run deploy
     
           # 推送到远端服务器
           - name: rsync deployments
             uses: burnett01/rsync-deployments@4.1
             with:
               # 这里是 rsync 的参数 switches: -avzh --delete --exclude="" --include="" --filter=""
               switches: -avzh --delete
               path: .deploy_git/  # action容器工作目录内的路径地址
               remote_path: /var/www/myblog # 远端服务器的路径地址
               remote_host: 121.36.61.23 # 服务器 ip
               remote_port: 22
               remote_user: root
               remote_key: ${{ secrets.SSH_PRIVATE_KEY }} # 一定配置密钥登录
     ```

     这段代码是一个 GitHub Actions 工作流程，主要用于配置环境并设置相关参数，包括：

     1. **使用 Node.js 版本：**
        - 使用 `actions/setup-node` 动作来设置 Node.js 环境。
        - `${{ matrix.node_version }}` 是一个矩阵构建中定义的变量，用于指定要使用的 Node.js 版本。

     2. **配置环境变量：**
        - 使用 `env` 关键字设置环境变量。
        - `HEXO_DEPLOY_PRI` 是一个环境变量，其值来自 GitHub Secrets 中的 `HEXO_DEPLOY_PRI` 密钥。
        
     3. **运行命令：**
        - 使用 `run` 关键字执行一系列命令。
        - `sudo timedatectl set-timezone "Asia/Shanghai"` 设置系统时区为亚洲/上海时区。
        - `mkdir -p ~/.ssh/` 创建 SSH 目录，用于存放 SSH 密钥和相关文件。
        - `echo "$HEXO_DEPLOY_PRI" > ~/.ssh/id_rsa` 将名为 `HEXO_DEPLOY_PRI` 的密钥内容写入 `~/.ssh/id_rsa` 文件中。
        - `chmod 600 ~/.ssh/id_rsa` 修改 `~/.ssh/id_rsa` 文件的权限，确保只有当前用户可以读写该文件。
        - `ssh-keyscan github.com >> ~/.ssh/known_hosts` 和 `ssh-keyscan 121.36.61.23 >> ~/.ssh/known_hosts` 用于将远程主机的公钥添加到 `~/.ssh/known_hosts` 文件中，以避免 SSH 连接时的警告或确认提示。
        - `git config --global user.name $GIT_USER` 和 `git config --global user.email $GIT_EMAIL` 用于设置全局 Git 用户名和邮箱。

     4. **注意事项**

        + 确保在服务器上生成一个密钥对, 将**公钥**添加到 `github SSH` 文件中, **私钥**添加到 `test\test_push.git` 仓库的 `secret` 变量中, 这里将**私钥**命名为 `SSH_PRIVATE_KEY`

          ![image-20240316004003064](../img/hexo%E8%BF%9C%E7%AB%AF%E9%83%A8%E7%BD%B2.assets/image-20240316004003064.png)

          ![image-20240316092137214](../img/hexo%E8%BF%9C%E7%AB%AF%E9%83%A8%E7%BD%B2.assets/image-20240316092137214.png)



