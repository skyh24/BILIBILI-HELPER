<div align="center"> 
<h1 align="center">
BILIBILI-HELPER
</h1>

[![GitHub stars](https://img.shields.io/github/stars/JunzhouLiu/BILIBILI-HELPER?style=flat-square)](https://github.com/JunzhouLiu/BILIBILI-HELPER/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/JunzhouLiu/BILIBILI-HELPER?style=flat-square)](https://github.com/JunzhouLiu/BILIBILI-HELPER/network)
[![GitHub issues](https://img.shields.io/github/issues/JunzhouLiu/BILIBILI-HELPER?style=flat-square)](https://github.com/JunzhouLiu/BILIBILI-HELPER/issues)
[![GitHub license](https://img.shields.io/github/license/JunzhouLiu/BILIBILI-HELPER?style=flat-square)](https://github.com/JunzhouLiu/BILIBILI-HELPER/blob/main/LICENSE) 
[![GitHub All Releases](https://img.shields.io/github/downloads/JunzhouLiu/BILIBILI-HELPER/total?style=flat-square)](https://github.com/JunzhouLiu/BILIBILI-HELPER/releases)
[![GitHub contributors](https://img.shields.io/github/contributors/JunzhouLiu/BILIBILI-HELPER?style=flat-square)](https://github.com/JunzhouLiu/BILIBILI-HELPER/graphs/contributors)
![GitHub release (latest SemVer)](https://img.shields.io/github/v/release/JunzhouLiu/BILIBILI-HELPER?style=flat-square)

</div>

# Skyh工具简介 

这是一个利用GitHub Action定时任务实现哔哩哔哩（Bilibili）每日自动投币，点赞，分享视频，直播签到，银瓜子兑换硬币，漫画每日签到，简单配置即可每日轻松获取65经验值，快来和我一起成为Lv6吧~~~~ 

**如果觉得好用，顺手点个Star吧 ❤**

**仓库地址:[JunzhouLiu/BILIBILI-HELPER](https://github.com/JunzhouLiu/BILIBILI-HELPER)**

## 功能列表
* [x] 每天上午8点10分自动开始任务。*【运行时间可自定义】*
* [x] 哔哩哔哩漫画每日自动签到 。
* [x] 每日自动从热门视频中随机观看1个视频，分享一个视频。
* [x] 每日从热门视频中选取5个进行智能投币 *【如果投币不能获得经验，默认不投币】*
* [x] 投币支持下次一定啦，可自定义每日投币数量。*【如果检测到你已经投过币了，则不会投币】*
* [x] 大会员月底使用快到期的B币券，给自己充电，一点也不会浪费哦，默认开启。*【可配置】*
* [x] 大会员月初1号自动领取每月5张B币券和福利。
* [x] 每日哔哩哔哩直播自动签到，领取签到奖励。*【直播你可以不看，但是奖励咱们一定要领】*

......

[点击快速开始使用](#快速开始使用)


# 目录
- [工具简介](#工具简介)
  - [功能列表](#功能列表)
- [目录](#目录)
- [使用说明](#使用说明)
  - [一、Actions定时任务（推荐）](#一actions定时任务推荐)
    - [配置自定义功能](#配置自定义功能)
    - [查看运行日志](#查看运行日志)
  - [二、使用Linux Crontab方式](#二使用linux-crontab方式)
    - [步骤](#步骤)
    - [运行效果](#运行效果)
  - [三、使用Windows10](#三使用windows10)
    - [步骤](#步骤-1)
- [微信订阅通知](#微信订阅通知)
  - [订阅执行结果](#订阅执行结果)
  - [订阅版本更新](#订阅版本更新)
- [快速更新](#快速更新)
  - [关于项目更新频率](#关于项目更新频率)
  - [使用Github Actions 自动同步源仓库代码](#使用github-actions-自动同步源仓库代码)
  - [手动拉取最新代码](#手动拉取最新代码)
- [常见问题](#常见问题)
  - [关于Action定时任务不执行的问题](#关于action定时任务不执行的问题)
- [API参考列表](#api参考列表)


# 使用说明
## 一、Actions定时任务（推荐）
1. **fork本项目**
2. **获取Bilibili Cookies**
- 浏览器打开并登录[bilibili网站](https://www.bilibili.com/)
- 按F12打开 “开发者工具” 找到应用程序/Application -> 存储-> Cookies
- 找到`bili_jct`,`SESSDATA`,`DEDEUSERID`三项，并复制值，创建对应的GitHub Secrets。

![图示](docs/IMG/20201012001307.png)

3. **点击项目 Seeting->Secrets->New Secrets 添加以下3个Secrets。** 
   
| Name       | Value          |
| ---------- | -------------- |
| DEDEUSERID | 从Cookie中获取 |
| SESSDATA   | 从Cookie中获取 |
| BILI_JCT   | 从Cookie中获取 |

![图示](docs/IMG/20201013210000.png)

4. **开启Actions并触发每日自动执行**
   
**Github Actions默认处于关闭状态，还大家请手动开启Actions，执行一次工作流，验证是否可以正常工作。**

![图示](docs/IMG/workflow_dispatch.png)

**Fork仓库后，GitHub默认不自动执行Actions任务，请修改`./github/trigger.json`文件,将`trigger`的值改为`1`，这样每天就会自动执行定时任务了。**

```patch
{
- "trigger": 0
+ "trigger": 1
}
```

如果需要修改每日任务执行的时间，请修改`.github/workflows/auto_task_bilili.yml`，在第12行左右位置找到下如下配置。

```yml
  schedule:
    - cron: '30 10 * * *'
    # cron表达式，Actions时区是UTC时间，所以下午18点要往前推8个小时。
    # 示例： 每天晚上22点30执行 '30 14 * * *'
```


本工具的Actions自动构建配置了缓存，平均运行时间在`20s`左右。

*如果收到了GitHub Action的错误邮件，请检查Cookies是不是失效了，用户主动清除浏览器缓存，会导致`BILI_JCT`和`DEDEUSERID`失效*

### 配置自定义功能

**配置文件位于`src/main/resources/config.json`**

参数示意

| Key                | Value         | 说明                                                     |
| ------------------ | ------------- | -------------------------------------------------------- |
| numberOfCoins      | [0,5]         | 每日投币数量,默认5                                       |
| selectLike         | [0,1]         | 投币时是否点赞，默认0, 0:否 1:是                         |
| ~~watchAndShare~~  | ~~[0,1]~~     | ~~观看时是否分享~~                                       |
| monthEndAutoCharge | [false,true]  | 年度大会员月底是否用B币券给自己充电，默认`true`          |
| devicePlatform     | [ios,android] | 手机端漫画签到时的平台，建议选择你设备的平台 ，默认`ios` |

*投币数量代码做了处理，如果本日投币不能获得经验了，则不会投币，每天只投能获得经验的硬币。假设你设置每日投币3个，早上7点你自己投了2个硬币，则十点半时，程序只会投1个）*

### 查看运行日志 

*展开`Build With Maven`通过`DEBUG`标签快速过滤日志，查看运行状态*  

[Actions运行日志详细查看教程](https://github.com/JunzhouLiu/BILIBILI-HELPER/issues/21)

[日志示例](https://github.com/JunzhouLiu/BILIBILI-HELPER/runs/1256484004?check_suite_focus=true#step:4:5069)

![图示](docs/IMG/debug1.png)
![图示](docs/IMG/debug2.png)


## 二、使用Linux Crontab方式

### 步骤
点击[BILIBILI-HELPER/release](https://github.com/JunzhouLiu/BILIBILI-HELPER/releases)，下载已发布的版本，上传至Liunx服务器。

1. `crontab -l`
```bash
root@iZuf642f8w148fwdcpq169Z:~# crontab -l
.......
# m h  dom mon dow   command
0 0 1,15 * * /home/./acme.sh-master/acme.sh --renew-all >>/var/log/cron.log 2>&1 &
0 0 1,15 * * nginx -s reload >>/var/log/cron.log 2>&1 &
```
2. `corntab -e`,编辑crontab任务，退出保存即可。后面跟的三个参数为哔哩哔哩Cookies参数。
```bash
# m h  dom mon dow   command
0 0 1,15 * * /home/./acme.sh-master/acme.sh --renew-all >>/var/log/cron.log 2>&1 &
0 0 1,15 * * nginx -s reload >>/var/log/cron.log 2>&1 &
30 10 * * * java -jar /home/BILIBILI-HELP.jar DEDEUSERID SESSDATA BILI_JCT >>/var/log/cron.log 2>&1 &
```

### 运行效果  
![图示](docs/IMG/liunxImg.png)


## 三、使用Windows10
### 步骤
1. 点击[BILIBILI-HELPER/release](https://github.com/JunzhouLiu/BILIBILI-HELPER/releases)，下载已发布的版本。解压，在解压后的目录打开`Powershell` 需要装有Java运行环境。
   
2. 执行`java -jar /home/BILIBILI-HELP.jar DEDEUSERID SESSDATA BILI_JCT `

![图示](docs/IMG/powershell.png)


# 微信订阅通知
## 订阅执行结果

1. 前往[sc.ftqq.com](http://sc.ftqq.com/3.version)创建账号，并且生成一个Key。将其增加到Github Secrets中，变量名为`SERVERPUSHKEY`，值为你创建账号获得的key,可在[查看Key](http://sc.ftqq.com/?c=code),查看你的key。
2. 绑定微信账号，开启微信推送。[绑定微信](http://sc.ftqq.com/?c=wechat&a=bind)

![图示](docs/IMG/serverpush.png)
3. 推送效果展示

![图示](docs/IMG/serverpush1.jpg)
![图示](docs/IMG/serverpush2.jpg)


## 订阅版本更新

微信扫描以下二维码订阅版本更新通知

![图示](docs/IMG/suborcode.png)


# 快速更新

## 关于项目更新频率
目前处于快速迭代阶段，建议通过以下两种方式从本仓库拉取最新代码。


## 使用Github Actions 自动同步源仓库代码

 该方案来自 `@happy888888` `#PR6` ，由于源仓库`config.json`文件的更改会覆盖用户自己的`config.json`配置文件，所以暂时没有合并到`main`分支。
 
 **使用自定义功能的朋友慎用该方法，建议使用手动拉取的方式，手动解决代码冲突** 

在`./github/workflows`目录下创建`auto_merge.yml`文件，内容如下

```yml
name: auto_merge

on:
  workflow_dispatch:
  schedule:
    - cron: 0 2 * * fri
    # cron表达式,每周五10点执行一次，UTC时间，使用北京时间请+8可按照需求自定义。  

jobs:
  merge:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout
      uses: actions/checkout@v2
      with:
        ref: main
        fetch-depth: 0
        lfs: true

    - name: Set git identity
      run : |
        git config --global user.email "41898282+github-actions[bot]@users.noreply.github.com"
        git config --global user.name "github-actions[bot]"
    - name: Load upstream commits
      run: |
        git update-index --assume-unchanged ./src/main/resources/config.json
        git pull https://github.com/JunzhouLiu/BILIBILI-HELPER.git --log --no-commit
    - name: Apply commit changes
      run: |
        if [ -f ./.git/MERGE_MSG ]; then
        mkdir ./tmp && cp ./.git/MERGE_MSG ./tmp/message
        sed -i "1c [bot] AutoMerging: merge all upstream's changes:" ./tmp/message
        sed -i '/^\#.*/d' ./tmp/message
        git commit --file="./tmp/message"
        else
        echo "There is no merge commits."
        fi
    - name: Push Commits
      env:
        DOWNSTREAM_BRANCH: main
        TZ: Asia/Shanghai
      run: git push origin $DOWNSTREAM_BRANCH
```

## 手动拉取最新代码

1. 通过`git remote -v`查看是否有源头仓库的别名和地址。

例如这里`origin`就是你自己的仓库，`upstream`是你`fork`的源头仓库。
```bash
$ git remote -v
origin  https://github.com/JunzhouLiu/cxmooc-tools.git (fetch)
origin  https://github.com/JunzhouLiu/cxmooc-tools.git (push)
upstream        https://github.com/CodFrm/cxmooc-tools.git (fetch)
upstream        https://github.com/CodFrm/cxmooc-tools.git (push)

```

2. fork仓库后，将你的仓库拉到本地，如果没有源头仓库，则添加源头仓库
```bash
git remote add upstream https://github.com/JunzhouLiu/BILIBILI-HELPER.git
```

3. 更新上游仓库main分支的代码（pull操作实际上是 `fetch+merge`）

```bash
git pull upstream main
```

4. 将从源头仓库更新后的代码推送到你自己的github仓库

```bash
git push origin main 
```
5. 这样你就能快速的从我的仓库拉取最新的代码，并更新到你自己的仓库里了。自定义配置的同学，要注意`config.json` 不要被我的文件覆盖了。 

# 常见问题

## 关于Action定时任务不执行的问题

**Fork仓库后，GitHub默认不自动执行Actions任务，请修改`./github/trigger.json`文件,将`trigger`的值改为`1`，这样每天就会自动执行定时任务了。**

```patch
{
- "trigger": 0
+ "trigger": 1
}
```

# API参考列表

- [SocialSisterYi/bilibili-API-collect](https://github.com/SocialSisterYi/bilibili-API-collect)
- [happy888888/BiliExp](https://github.com/happy888888/BiliExp)
