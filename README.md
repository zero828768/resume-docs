# 简介

基于Hexo + 主题，实现简约的在线简历，简历内容可随时修改。而且，可以采用懒人法只维护MarkDown文档，不需要部署本地环境。

# 部署环境
结合 `Github Actions` 功能，其实已经非常简单，我们只需要关注内容修改，提交后将自动渲染发布到外网。

## 生成秘钥对
打开Gitbash，使用以下命令：
```
ssh-keygen -f github-deploy-key
``` 
一路回车，会在命令执行路径下生成两个文件：
1. `github-deploy-key` - 私钥，部署到源代码仓库
2. `github-deploy-key.pub` - 公钥，部署到对外发布仓库

## 部署 `Actions`
在源代码仓库根目录下创建 `.github/workflows/deploy.yml` 文件，目录结构如下：
```
./ (repository)
└── .github
    └── workflows
        └── deploy.yml
```

`deploy.yml` 内容：
```
name: HEXO CI #task name display

on: [push] #git event

jobs:
  build:
    name: Auto deploy website
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [12.x]

    steps:
      - name: Checkout #step1 get code
        uses: actions/checkout@v1 #use existing script
        with:
          submodules: true #Checkout private submodules(themes or something else)

      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v1
        with:
          node-version: ${{ matrix.node-version }}

      - name: Set environment
        env:
          USER_NAME: your_name #git user.name
          USER_EMAIL: your_email  #git user.email
          DEPLOY_KEY: ${{secrets.DEPLOY_KEY}} #git deploy key
        run: |
          mkdir -p ~/.ssh/
          echo "$DEPLOY_KEY" > ~/.ssh/id_rsa
          chmod 600 ~/.ssh/id_rsa
          ssh-keyscan github.com >> ~/.ssh/known_hosts
          git config --global user.name "$USER_NAME"
          git config --global user.email "$USER_EMAIL"

      - name: Install dependencies
        run: |
          npm i -g hexo-cli
          npm i

      - name: Deploy hexo
        run: |
          hexo g -d
```
## 绑定域名「可选」
Github提供了默认的 `xx.github.io` 用户域名，如果不能满足需求，可以自行购买域名绑定：

```
# 示例
echo yiwangmeng.com > src/CNAME
```

以上命令也就是在 `src` 目录下生成一个 `CNAME` 文件，该文件里填上要绑定的自定义域名即可。

# 修改内容
基本的信息配置完成后，我们只需要修改 `src` 目录下的 `index.md` 即可，简历全部内容都在里面，对应英文版则在 `src/en/index.md` ，按照其中模板内容自行修改即可。
```
---
# 语言 （可选）
lang: zh-cn
# 网页关键词和描述
keywords: 简历主题,Hexo主题,简历模板,一网盟
description: 这是一个简约大方的在线简历
# 简历标题
resume_title: 我的简历
# 应聘者姓名
name: 一网盟
# 联系方式
contact:
  - icon: fas fa-globe-asia
    text: 一网盟
    url: https://yiwangmeng.com
  # 邮箱
  - icon: fas fa-envelope
    text: xxx@yiwangmeng.com
    url:
  # 电话号码
  - icon: fas fa-phone-alt
    text: 18812345678
    url: tel:10086
# PDF下载链接
download:
  title: 下载本站源码
  icon: fas fa-download fa-fw
  url: https://yiwangmeng.com
---
//上面的 --- 中间部分有格式要求，请注意是英文冒号，后面至少要带一个空格才可以
{% raw %}   //这种代码不要动，保留即可
<grid>
<avatar><img src="https://cdn.jsdelivr.net/gh/xaoxuu/cdn-assets/avatar/avatar.png"></avatar>
<h1>Hexo 简历主题</h1>
<center>
<a href='/en/'>English</a> | <a href='/'>简体中文</a>
</center>
<br>
</grid>
{% endraw %}


## <i class="fas fa-flag"></i> 开始使用

以下都是可以自由发挥的内容，基本的MarkDown格式。


## <i class="fas fa-user-graduate"></i> 教育背景

**XX大学 X学院 X系 X专业 X年毕业**


## <i class="fas fa-user-tie"></i> 工作经验


#### 2000年 ~ 至今：XX公司

- 主要负责XXX
- 也负责XXX


#### 1900年 ~ 2000年：XX公司

- 主要负责XXX
- 也负责XXX

#### 1800年 ~ 1900年：XX公司

- 主要负责XXX
- 也负责XXX


## <i class="fas fa-award"></i> 精选项目


{% raw %}
<btns rounded>
<a href='https://apps.apple.com/cn/app/heart-mate-pro-hrm-utility/id1463348922?ls=1'>
  <img src='https://cdn.jsdelivr.net/gh/xaoxuu/cdn-assets/proj/heartmate/icon.png'>
  心率管家
</a>
<a href='https://apps.apple.com/cn/app/c%E5%85%BB%E8%80%81/id1458315594'>
  <img src='https://cdn.jsdelivr.net/gh/xaoxuu/cdn-assets/proj/het-cyanglao/icon.png'>
  C养老
</a>
<a href='https://apps.apple.com/cn/app/c-life%E5%85%BB%E8%80%81/id1393937890'>
  <img src='https://cdn.jsdelivr.net/gh/xaoxuu/cdn-assets/proj/het-clife/icon.png'>
  C-Life养老
</a>
<a href='https://apps.apple.com/cn/app/linksmart/id1109303355'>
  <img src='https://cdn.jsdelivr.net/gh/xaoxuu/cdn-assets/proj/ht-linksmart/icon.png'>
  LinkSmart
</a>
<a href='https://apps.apple.com/cn/app/hitfit/id1207738581'>
  <img src='https://cdn.jsdelivr.net/gh/xaoxuu/cdn-assets/proj/ht-hitfit/icon.png'>
  HitFit
</a>
<a href='https://apps.apple.com/cn/app/%E8%85%95%E8%83%BD%E5%8A%A9%E6%89%8B/id1138242219'>
  <img src='https://cdn.jsdelivr.net/gh/xaoxuu/cdn-assets/proj/ht-fiyta/icon.png'>
  飞亚达腕能助手
</a>
</btns><br>
{% endraw %}


### A项目

#### 2000/01 ~ 2019/01：于XX公司开发，团队项目，维护至今

啦啦啦

### B项目

#### 1900/01 ~ 2000/01：于XX公司开发

啦啦啦

### C项目

#### 1800/01 ~ 1900/01：于XX公司开发

啦啦啦

## <i class="fab fa-github"></i> 开源贡献


### Volantis

#### 2017 ~ 至今，一个简约的卡片式Hexo博客主题

- 完全自由的模块化、易于定制化设计
- 移动端优化
- 源码：https://github.com/xaoxuu/hexo-theme-volantis
- 官网：https://volantis.js.org/

### ProHUD

#### 2019/08 ~ 至今，易于定制、接口简单的HUD库

- 使用Swift5编写。
- 包含顶部通知横幅、弹窗、底部操作表三种使用场景的UI控件。
- 易于配置UI从而满足公司各业务线的UI要求，接口调用简单明了。
- 源码：https://github.com/xaoxuu/ProHUD

<fancybox>
<img src='https://cdn.jsdelivr.net/gh/xaoxuu/cdn-assets/proj/prohud/screenshot01.png'>
<img src='https://cdn.jsdelivr.net/gh/xaoxuu/cdn-assets/proj/prohud/screenshot02.png'>
<img src='https://cdn.jsdelivr.net/gh/xaoxuu/cdn-assets/proj/prohud/screenshot03.png'>
<img src='https://cdn.jsdelivr.net/gh/xaoxuu/cdn-assets/proj/prohud/screenshot04.png'>
<img src='https://cdn.jsdelivr.net/gh/xaoxuu/cdn-assets/proj/prohud/screenshot05.png'>
<img src='https://cdn.jsdelivr.net/gh/xaoxuu/cdn-assets/proj/prohud/screenshot06.png'>
<img src='https://cdn.jsdelivr.net/gh/xaoxuu/cdn-assets/proj/prohud/screenshot07.png'>
<img src='https://cdn.jsdelivr.net/gh/xaoxuu/cdn-assets/proj/prohud/screenshot08.png'>
<img src='https://cdn.jsdelivr.net/gh/xaoxuu/cdn-assets/proj/prohud/screenshot09.png'>
<img src='https://cdn.jsdelivr.net/gh/xaoxuu/cdn-assets/proj/prohud/screenshot10.png'>
</fancybox>

## <i class="fas fa-phone-alt"></i> 与我联系

目前状态为：在职，考虑换工作，100年内可到岗。

<i class="fas fa-envelope fa-fw"></i> your email
<i class="fas fa-phone-alt fa-fw"></i> 1xxxxxxxxxx

```