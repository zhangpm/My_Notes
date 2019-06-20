---
title: Hexo + GitHub 个人主页
---
## 1.安装
1. node.js
2. git,github账号
3. 新建文件夹blog，在文件夹下右键-git bash
4. 安装hexo：npm install -g hexo-cli
5. 输入：hexo init; npm install； npm install hexo-deployer-git --save
6. 进入github网站，新建repository,"name"填写“用户名.github.io”，例如“pengmiao.github.io”
7. 生成repository，复制ssh或http地址
8. 到blog文件夹下找到_config.yml,打开，在最后修改和添加：
```yml
deploy:
  type: git
  repo: git@github.com:zhangpm/pengmiao.github.io.git
  branch: master
```
8. 即部署到git, **重要**
```hexo g -d```,
9.  打开网页：https://zhangpm.github.io/
## 2. 更新和添加博客
1. 在文件夹下G:\git\blog\source\_posts 添加markdown文件
2. 部署博客“hexo g -d”

## 3. 更换主题
1. Hexo官网，themes，找主题
2. 在blog目录下按照安装说明操作,一般会修改blog下的config（不是主题下）
3. 在所用主题的目录下找config.yml，修改图片和名字

## 4. 渲染数学公式
https://www.jianshu.com/p/7ab21c7f0674

## 5. 图片处理
1. CMD连接文件夹：mklink G:\git\blog\source\_posts\pic G:\git\blog\source\pic
2. markdown中写入/pic/图片，本机取\_posts\pic，远程取\source\pic，内容一样，均可显示
