

# 安装 git

- [Git - Downloading Package (git-scm.com)](https://git-scm.com/download/win)
- 直接下一步就可以了，安装完桌面有 git bash 的图标

# 安装 node.js

- [下载 | Node.js (nodejs.org)](https://nodejs.org/zh-cn/download)

- 直接下一步就可以了

- 查看 node 和 npm 是否安装完成

    ```markdown
    #检查 node.js
    node -v
    #检查 npm
    npm -v
    ```

# 安装 cnpm

```
npm install -g cnpm --registry=https://registry.npm.taobao.org
检查 cnpm 是否安装成功
cnpm -v
```

# 安装 Hexo

- 用 cnpm 安装 hexo

    ```
    cnpm install -g hexo-cli
    
    检查是否安装成功
    hexo -v
    
    
    ```

- blog 初始化

    - 新建个目录 ，blog
    - 打开 git bash
    - 进入到 blog 目录
    - 在git bash 输命令 ：hexo init 
    - 初始化成功： 出现 INFO Start blogging with hexo！
    - 输入命令 ：  hexo  s
    - 在浏览器输入localhost:4000查看

# 部署blog

- gitee 注册

- 新建仓库

- 仓库地址：https://gitee.com/pjlong/blog

    - 简易的命令行入门教程:

        ```
        Git 全局设置:
        
        git config --global user.name "pjlong"
        git config --global user.email "321518146@qq.com"
        创建 git 仓库:
        
        mkdir blog
        cd blog
        git init 
        touch README.md
        git add README.md
        git commit -m "first commit"
        git remote add origin https://gitee.com/pjlong/blog.git
        git push -u origin "master"
        已有仓库?
        
        cd existing_git_repo
        git remote add origin https://gitee.com/pjlong/blog.git
        git push -u origin "master"
        ```

- 安装 git 插件

    ```
    cnpm install --save hexo-deployer-git
    ```

- 设置远程仓库

    - 打开 `_config.yml`

    - 设置 url

        ```
        # URL
        ## Set your site url here. For example, if you use GitHub Page, set url as 'https://username.github.io/project'
        url: https://gitee.com/pjlong/blog ##创建 gitee的用户名
        root: /blog/
        ```
    
- 设置仓库地址
  
    ```
    ​```
    # Deployment
    ## Docs: https://hexo.io/docs/one-command-deployment
    deploy:
      type: git
      repo: https://gitee.com/pjlong/blog
      branch: master
    ```
    ```
    
    ```
    
- 部署到远端
  
        ```
        hexo d
        
        INFO Deploy done: git表示推送成功
    ```
    
    ```
    
- 开启静态页面服务
  
        ```
        进入码云新建的仓库，开启 Gitee Pages
    ```
    
    - hexo部署到gitee，网页没有样式
    
    ```
        # URL
        # url 这里 要设置成 gitee 仓库的网址
        # root 使用网址最后一个/
        url: https://gitee.com/name/blog
        root: /blog/
        ```
    
# 参考

​    

    [(205条消息) 在Gitee搭建属于自己的博客_用gitee搭建博客_jiuqi_玖柒的博客-CSDN博客](https://blog.csdn.net/qq_46036214/article/details/110137239)
    
    [Hexo+Next主题搭建个人博客+优化全过程（完整详细版） - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/618864711)
    
    [(205条消息) hexo史上最全搭建教程_zjufangzh的博客-CSDN博客](https://blog.csdn.net/sinat_37781304/article/details/82729029)
    
    [(205条消息) hexo史上最全搭建教程_zjufangzh的博客-CSDN博客](https://blog.csdn.net/sinat_37781304/article/details/82729029)
    
    [(205条消息) Hexo为文章设置目录与标签的方法_hexo 文章目录_Half_A的博客-CSDN博客](https://blog.csdn.net/weixin_44543463/article/details/119738094#:~:text=Hexo为文章设置目录与标签的方法 1 1. 创建目录页 在网站根目录下执行以下代码。 hexo new page,title%3A 这里是文章的标题 date%3A 这里是发表时间，如：2021-08-15 08%3A15%3A16 description%3A 这里填写摘要。 )


​    

