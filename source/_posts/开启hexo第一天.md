---
title: 开启hexo第一天
date: 2020-03-02 16:33:22
tags: [hexo, 博客]
categories: Hexo
---
## 基本流程
### 1，安装git和node
git已经安装过了。
用brewhome安装node：
```bash
brew install node
```
安装成功。

### 2，安装hexo
```bash
npm install -g hexo
```
报错：“error Cannot convert name to ASCII”，网上查说是icu的版本问题。
解决方法：更新brewhome, 重新安装node
```bash
brew update
```
中间又遇到brew update的问题，因为访问不了github，报400错误。
解决方法：git配置了代理，取消掉代理。（公司网络问题基本上都是代理的锅）

### 3，测试hexo
```
hexo init # 当前路径下生成blog目录
cd blog
npm install # 安装依赖包
hexo generate # 生成静态页面=hexo g
hexo server # 启动服务=hexo s
```
服务能起来，说明装好了。这个服务是本地作为服务器，只是用来测试。我用github的个人仓库作为服务器。

### 4，github配置
github新建repo，名为username.github.io，该工程路径为：https://github.com/username/username.github.io.git

### 5，hexo和github关联配置
```bash
cd blog
vim _config.yml # 修改站点配置
```
修改的项包括：
```bash
deploy:
  type: git
  repo: https://github.com/username/username.github.io.git
  branch: master
```

### 6, 安装git部署插件
```bash
npm install hexo-deployer-git --save
```

### 7，静态页面部署：
```bash
hexo clean # 清理缓存和静态结果。——否则修改可能不起作用。
hexo g # 生成
hexo deploy # 部署=hexo d, 生成的静态页面会推送到github上
#上面两句可以合成为：hexo -g d或者 hexo -d g
```
可以通过域名：username.github.io访问我们到博客。

## next主题配置
### 配置文档
 比较流行next主题,参考[http://theme-next.iissnan.com/getting-started.html](http://theme-next.iissnan.com/getting-started.html)配置。

### 主要问题
#### 1，categories和tags等页面404，需要配置：
新建相关page：
```
cd blog
hexo new page tags
vi source/tags/index.md
```
修改如下：
```bash
---
title: tags
date: 2020-03-02 15:23:37
type: tags
---
```
然后，去掉主题配置文件menu对应注释
```bash
menu:
  home: / || home
  about: /about/ || user
  tags: /tags/ || tags
  categories: /categories/ || th
  archives: /archives/ || archive
```
然后重启，**一定先要hexo clean*

#### 关于latex公式不能正常显示
首先，在next主题配置文件中打开math相关配置。
然后，如果仍然有不不兼容的情况，主要可能有以下问题：
* x_0要写成{x}_{0}，避免下划线与斜体冲突；
* 出现连续两个"{"的时候，两个"{"之间要有空格，或者前后用"{% raw %}"和"{% endraw %}"包裹起来。
