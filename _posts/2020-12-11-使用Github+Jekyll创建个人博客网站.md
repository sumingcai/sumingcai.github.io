---
layout: post
title:  "用Github+Jekyll搭建个人博客"
author: "sumcai"
header-style: text 
tags: [Jekyll, blog, 工具]
---

在信息爆炸的当今社会，技术发展日新月异，不时刻保持学习很快就会跟不上时代步伐。我很久之前就希望有一个自己的个人网站来收集平时看到的一些好的文章、教程，把这些零碎的信息都变成自己的收藏，当下一次需要查阅时，不至于穷死苦想在哪看过。如果有了自己的网站，加上分类和搜索，能快速查到自己想找的内容，那该多么令人兴奋呀！经过一段时间的摸索，找到了一个免费而且便捷的方式搭建个人博客，具备分类和查找功能，而且还免费、稳健可靠，那就是通过 Github/Gitee + jekyll 搭建。

# 准备工作
考虑到国内的网速问题，我们使用 Gitee 作为平台。实现过程的思路是：在本地编写jekyll网站源码，上传至Gitee，由Gitee生成并托管整个博客网站。

- [Gitee账号](https://gitee.com)
- [安装Ruby](http://www.ruby-lang.org/zh_cn/)
- [安装jekyll](https://www.cnblogs.com/mingyue5826/p/11533978.html)

## 环境部署

### 安装文件下载

我的开发环境是win7 64位，下载如下安装程序：
- [rubyinstaller-devkit-2.7.1-1-x64.exe](https://download.csdn.net/download/smcnjyddx0623/13457483?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522160723504119215668841683%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fdownload.%2522%257D&request_id=160723504119215668841683&biz_id=1&utm_medium=distribute.pc_search_result.none-task-download-2~download~first_rank_v2~rank_dl_default-1-13457483.pc_v2_rank_dl_default&utm_term=rubyinstaller-devkit-2.7.1%20windows64%E4%BD%8D%E5%AE%89%E8%A3%85%E5%8C%85&spm=1018.2118.3001.4451)
  
- [DevKit-mingw64-64-4.7.2-20130224-1432-sfx.exe](https://rubyinstaller.org/downloads/archives/)

### 安装Ruby
运行安装`rubyinstaller-devkit-2.7.1-1-x64.exe`，所有的选项都勾选上，最后的MSYS2一定要安装。

`DevKit-mingw64-64-4.7.2-20130224-1432-sfx.exe`运行解压到一个单独目录，打开cmd，进入到解压目录，执行命令：

```ruby
ruby dk.rb init    #初始化
ruby dk.rb review  # 审查（非必须）
ruby dk.rb install  # 安装
gem -v  # 查看gem是否正常安装
```

### 安装jekyll

- 先切换到国内的镜像源：

    ```gem
    gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/
    gem sources -l
    ```

- 安装Jekyll
  
    ```gem
    gem install jekyll
    ```

- 安装jekyll-paginate
  
    ```gem
    gem install jekyll-paginate
    jekyll -v
    ```

- 安装 Bundler
  
  ```gem
  gem install bundler
  ```

- 查看安装版本

    ```gem
    jekyll --version
    ```

### 创建博客

```ruby
jekyll new blog     #创建新项目
cd blog
jekyll server       #运行服务器
```

上面的`jekyll new blog`可能会很慢，使用`jekyll new blog --skip-bundle`跳过bundle环节，然后修改新blog目录中的Gemfile，将第一行改成`source "https://gems.ruby-china.com"`，再运行`bundle install`即可完成项目创建。

访问测试：http://127.0.0.1:4000/
![](/assets/2020-12-06-16-25-44.png)

---



## 扩展功能

### valine评论功能

[Valine](https://valine.js.org)，是一款快速、简洁且高效的无后端评论系统，基于LeanCloud搭建，对Jekyll的博客是支持的。

#### 获取 APP ID 和 APP KEY

1. [点击这里登录或注册Leancloud](https://leancloud.cn/dashboard/login.html#/signup)

2. [点这里创建应用](https://leancloud.cn/dashboard/applist.html#/newapp)，应用名看个人喜好。
![006qrazegy1fkwo2fpoetj30h40coaak](/assets/006qrazegy1fkwo2fpoetj30h40coaak.jpg)

3. 选择左下角的设置>应用Key
![006qrazegy1fkwo6w2b6uj30xe0etjt4](/assets/006qrazegy1fkwo6w2b6uj30xe0etjt4.jpg)



#### _config.yaml配置

```yaml
valine_comment: 
enable: true
leancloud_appid: Cd26RI8uft3yIkXRTKTvVWe7-gzGzoHsz
leancloud_appkey: UVDInqUyF8cl6EaRA58snH7h
placeholder: Say something...
```


#### 新增评论页面

新增`_includes/valine_comment.html`

```html
<!-- <script src="https://cdnjs.loli.net/ajax/libs/jquery/3.2.1/jquery.min.js"></script> -->
<script src="//cdn1.lncld.net/static/js/3.0.4/av-min.js"></script>
<script src="//unpkg.com/valine/dist/Valine.min.js"></script>
<script>
new Valine({
el: '#comments',
app_id: '{{ site.valine_comment.leancloud_appid }}',   //网站配置文件_config.yml
app_key: '{{ site.valine_comment.leancloud_appkey }}', //网站配置文件_config.yml
placeholder:'{{ site.valine_comment.placeholder }}'
});</script>
```



#### 增加评论功能代码

在需要添加评论框的文件中增加：

```html
{% if site.valine_comment.enable %}
<!-- 使用 valine 评论框 start -->
<div id="comments">
{% include valine_comment.html %}
</div>
<!-- 使用 valine 评论框 end -->
{% endif %}
```

​      


参考：
[windows安装jekyll步骤及问题](https://blog.csdn.net/mouday/article/details/79300135)
[windows安装jekyll](https://www.cnblogs.com/mingyue5826/p/11533978.html)