# @Travis-CI && @GithubAction && @Coding && @Vercel,你帮我下载文件行吗QvQ

## 前言

此项目是一个**鰯鲫**下午在学校用下载Github下载PicGo时的无力吐槽。

网传加速Github的项目有很多种，包括但不限于：

- Gitee加速克隆 【缺点：账号在异地登陆很有可能风控需要手机验证码（然而人在学校没有手机】
- cnpm加速 【缺点：无法下载Release，且移动网络下载速度鸡贼】
- CloudFlareWorkers加速 【缺点：电信网络下下载速度比百度网盘稍微好些，而且丢包严重】
- jsdelivr加速 【缺点：无法下载>20MB的文件，并且官方屏蔽exe下载】
- 改host 【一句话，麻烦（拒绝任何反驳qwq】

此项目均解决以上问题，但是仍有缺点：

- 第一次配置略麻烦
- 每下载一次文件需要修改一次Github
- 若大文件下载分包较多麻烦

## 开始

### Travis-CI（平均53s

#### 第一次配置

1. Star✨此项目 ~~【可选~~
2. Fork此仓库
3. 修改 `.travis.yml` 14行单引号括起来的链接地址，将其设置为你所需要下载的链接。

> **注意，其中不得包含特殊字符如\&**

> 实际上我可以把它写在travis-ci的变量里，可以避免公布下载链接，但是个人测试，github访问速度比travis-ci快多了，而且travis-ci登陆还是要Github...

4. 进入你的Github个人资料，[新建一个token](https://github.com/settings/tokens) ，权限至少repo读写、profile读【建议全勾】，生成token并复制，妥善保管。

5. 进入 <https://travis-ci.org> 或 <https://travis-ci.com>，用github账号登入，进入dashboard-setting，设置环境变量：变量名字: `GH_TOKEN` ，变量值: 你生成的TOKEN，tigger此项目。

6. 大约一分钟，这个项目就会变绿，如果没有，请检查日志；成功后进入仓库，选择 `gh-pages` 分支，你大概会看到以下文件：



```

gh-pages
  - dl.tar.bz2.aa
  - dl.tar.bz2.ab
  - dl.tar.bz2.ac
  - dl.tar.bz2.ad
  
```

~~原来下载文件在 `re\` 文件夹下~~，为避免Github发现有二进制文件并发送警告邮件，我们在部署前删除了`/re`文件夹。但是jsdelivr禁止下载exe文件且不得大于20MB，所以我们将其分割为20MB的包，下载时请使用以下链接：

```
https://cdn.jsdelivr.net/gh/用户名/仓库名字@gh-pages/分块名字
```

以本仓库为例，下载以下所有链接：

```
https://cdn.jsdelivr.net/gh/ChenYFan-Tester/travis-ci-help-me-download@gh-pages/dl.tar.bz2.aa
https://cdn.jsdelivr.net/gh/ChenYFan-Tester/travis-ci-help-me-download@gh-pages/dl.tar.bz2.ab
https://cdn.jsdelivr.net/gh/ChenYFan-Tester/travis-ci-help-me-download@gh-pages/dl.tar.bz2.ac
https://cdn.jsdelivr.net/gh/ChenYFan-Tester/travis-ci-help-me-download@gh-pages/dl.tar.bz2.ad
https://cdn.jsdelivr.net/gh/ChenYFan-Tester/travis-ci-help-me-download@gh-pages/end.bat
```

剪切以下文件至新建文件夹，运行 `end.bat` 合并所有分块，解压 `dl.tar.bz2` 即可

### 以后...

1. 改 `.travis.yml` 14行单引号括起来的链接地址，将其设置为你所需要下载的链接。
2. 为绕开jsdelivr缓存，请修改16行、17行默认的名字 `dl`【可选】
3. 进入github，gh-pages分支等待一分钟即可

## Coding部署版（不清楚

> 此方法避免分块麻烦

感谢[Colsrch](https://github.com/Colsrch) 提供Coding版本及readme

## 详见Coding仓库：[Coding帮我下个文件](https://jxsfgz-cat.coding.net/public/github-release-download/github-release-download/git/files)

### 配置方法

0. Star✨此项目 ~~【可选~~
1. 新建一个仓库
2. 将 `/Coding` 文件夹中的`Jenkinsfile`文件写入仓库，文件名不变
3. 新建构建计划，选择自定义构建计划，选择代码库为Coding，选择该仓库，配置来源选择 `使用代码库中的Jenkinsfile` ，节点池选择 `硅谷-美国`。
4. 修改触发规则为推送到master自动执行。

### 食用方法

1. 申请一个令牌
2. 添加默认字符串变量`CODING_KEY`变量内容为：`令牌用户名:令牌密钥`
3. 修改仓库Jenkinsfile文件中的克隆地址为该仓库地址。
4. 修改需要下载的下载链接
5. 保存就可以啦，然后就会自动开始下载。
6. 最后前往`download`分支下载你所需的这个文件就可以了，别忘了下载完成后删除download分支哦。
7. 之后食用直接修改下载链接即可，别忘了每次下载完成后要删除download分支哦

## Github-Action版本（平均耗时46s

感谢[Flexiston](https://github.com/Flexiston),同样支持GithubAction部署，原理类似，请自行更改。

默认使用travis-ci部署，若用GithubAction请将workflew的yml中 `_master` 改为 `master`,并设置俩变量TOKEN和LINK，TOKEN即GithubTOKEN，LINK为下载链接

## Vercel版本（平均耗时12s

> 目前部署速度最快

> 电信用户强推，电信直连香港亚马逊，避免分块麻烦

1. 首先你有个github账号，用此账号登录[vercel](https://vercel.com) ，并Star✨此项目 ~~【可选~~
2. Import此项目
3. 新建一个变量，变量名字为 `LINK` 变量值为下载链接。
4. Deploy！你会获得一串链接，里面是vercel自带的目录列表程序，点击下载即可
5. 以后只要修改LINK值并点击 `redeploy` 重新部署即可。


# 已知的问题：

- [x] 下载Release时可能会下载到Github默认的跳转页面【此时为一个html】
  - ~~暂时解决方法：用记事本打开下载的文件，复制跳转链接，用双引号包裹该链接下载~~
  - ~~注意：建议直接下载Source打包，避免跳转至亚马逊云【亚马逊云链接有特殊字符】~~
  - 已修复，采用-L支持重定向

# Todo：

- [x] 编写GithubAction版本
- [x] 编写Coding版本
- [ ] Netlify支持
- [ ] Heroku支持
- [x] Vercel支持



# 关于

travis-ci集成部署，通过curl下载、tar打包、split切块、git部署至Github、jsdelivr加速下载、windows下copy命令合并。

此脚本可以下载任意直链文件，包括但不限于Github，但是**请勿滥用**。

请注意下载完毕后将 `gh-pages` 分支及时删除!否则会产生比较大的commit影响Github!

## jsdelivr条约约束：

根据2020年8月9日[Jsdelivr Create Acceptable Use Policy](https://github.com/jsdelivr/jsdelivr/pull/18247/files) 

其中第4条Prohibited Use：

```
4. Prohibited Use

The following behavior is prohibited:

 1. Hosting or accessing content that:
     - contains malware or harmful code in any form,
     - violates proprietary rights of others,
     - is sexually explicit,
     - is potentially illegal in the EU or the USA.

 2. Abusing the service and its resources, or using jsDelivr as a general-purpose
    file or media hosting service. This includes, for example:
     - running an image hosting website and using jsDelivr as a storage for all
       uploaded images,
     - hosting videos, file backups, or other files in large quantities.

    We recognize that there are legitimate projects that consist of a large number
    of files, and these are not considered abuse. For example: icons packs, apps,
    or games with a large number of assets.

```

其中第二点明确指出**不得用于大型文件备份**，虽然我们有分块制度，但我个人建议不得下载超过100MB。

第5条Additional Restrictions in China

```
## 5. Additional Restrictions in China

jsDelivr holds an ICP license issued by the Chinese government,
which allows us to operate infrastructure directly in Mainland China.
To keep this license and allow our Chinese users to benefit from the performance
improvements it provides, any content served via our Chinese network must conform
to Chinese policies. Content potentially violating Chinese policies may be
blocked in China without warning.
```

明确指出使用网宿节点的网民【与居住地点、国籍无关】必须遵守中华人民共和国法律约束，不得用于下载当局明令禁止文件。

由于以上原因导致无法使用jsdelivr服务者，本项目、项目组织、项目创始人均不负责。

# 许可

MIT
