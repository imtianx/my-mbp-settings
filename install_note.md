
# Install Softwares

> 参考 ：[bestswifter/macbootstrap](https://github.com/bestswifter/macbootstrap)


## 1.[brew](https://brew.sh/) 安装低版本软件

以 安装 `python` 为例。由于 mac默认安装有 python2 ,为了不覆盖 python2 ,这里安装 python3 ，并且所有的 命令都带 `3` 以区别。

如果目前 python 最新为 `3.7.0`, 但是由于某些软件的兼容性，需要 `3.6.x` 版本 直接用下面的命令 安装的都是最新的。

```
brew install python3
```

**这里，通过在 [Formula](https://github.com/Homebrew/homebrew-core/blob/master/Formula/) 中找到 相关软件的 rb文件的指定版本,在本地切换到相应的版本，然后进行安装。**


这里通过 查看 `python.rb` 文件的提交日志，找到需要安装的 3.6.5 版本的 commit id :  f2a764ef9

然后终端 进入本地的 Formula 目录，切换分支，安装软件，最后再切回到master:

```
cd /usr/local/Homebrew/Library/Taps/homebrew/homebrew-core/Formula

git checkout f2a764ef9

// 注意这里 要用 python ，不能用 python3，否则安装仍是最新版本软件,指定openssl,否则 pip 无法使用 ssl
brew install python --with-brewed-openssl

git checkout master

```
进入目录中，可以查看 python.rb 文件，通过 url 处的连接查看软件的版本。


由于 brew 每次安装软件前都会检查更新，可以关闭 ：

```
export HOMEBREW_NO_AUTO_UPDATE=true
```

## 2.Mac 微信防撤回、多开插件：[WeChatPlugin-MacOS](https://github.com/TKkk-iOSer/WeChatPlugin-MacOS)

```
curl -o- -L https://raw.githubusercontent.com/lmk123/oh-my-wechat/master/install.sh | bash -s
omw

```

> wechat 更新后需重装，安装完插件需重启 wechat

## 3、各种 Preview 插件  [quick-look-plugins](https://github.com/sindresorhus/quick-look-plugins)

主要包括：
 - qlcolorcode: 代码文件预览时高亮
 - qlstephen: 以纯文本的形式预览无拓展名或者未知拓展名的文件
 - qlmarkdown: 预览渲染后的 markdown 文件
 - quicklook-json: 预览格式化后的 json 文件
 - qlimagesize: 预览图片时显示图片大小
 - webpquicklook: 预览 webp 格式的图片
 - suspicious-package: 预览 Mac OX 安装包类型的文件
 - qlvideo: 预览视频文件
 - provisionql: 预览 iOS 开发时用到的 provisionfile 文件
 - quicklookapk: 预览安卓的 apk 文件


## 4、staruml 破解

1. [官网下载安装](http://staruml.io/download)，打开一次然后关闭；
2.  使用 `npm` 安装 **asar** :
   ```
    npm install asar -g
   ```
3. 进入用用中的此目录
  ```
  cd /Applications/StarUML.app/Contents/Resources/
  ```
4. 解压 `app.asar` 
```
asar extract app.asar app 
```
5. 修改 app 该文件： `app/src/engine/license-manager.js` **125行**,

修改前：

```
  checkLicenseValidity () {
    this.validate().then(() => {
      setStatus(this, true)
    }, () => {
      setStatus(this, false)
      UnregisteredDialog.showDialog()
    })
  }

```

**修改后：**

```
  checkLicenseValidity () {
    this.validate().then(() => {
      setStatus(this, true)
    }, () => {
      setStatus(this, true)
    })
  }

```

6. 重新打包 app 即可完成破解

```
asar pack app app.asar
```

# 5、mac 命令删除文件到回收站 [rmtrash](http://macappstore.org/rmtrash/)

```
// install 
brew install rmtrash
// set alia
alias rm=rmtrash
```

# 6、git 开启 gpg

[github doc](https://help.github.com/en/articles/managing-commit-signature-verification)

安装 `gpg`:

```
brew install gpg
```
生成证书，[文档](https://help.github.com/en/articles/generating-a-new-gpg-key)

```
// generate 
gpg --full-generate-key

// view
gpg --list-secret-keys --keyid-format LONG

// show detail
gpg --armor --export <gpg_kek_id>

// add to github 

// test , if error to run : export GPG_TTY=$(tty) 
echo "test" | gpg --clearsign

```

常见错误：

- gpg: agent_genkey failed: No such file or directory
```
 ps axu | grep gpg-agent
 // kill 
```
- [gpg failed to sign data](https://stackoverflow.com/questions/41052538/git-error-gpg-failed-to-sign-data/41054093)

**sourcetree 开启 gpg**

进入 gpg 安装目录，`/usr/local/Cellar/gnupg/2.2.15/bin/` 中，新建链接，
```
ln -s gpg gpg2
```
然后在 sourcetree 中选择 gpg 目录;

接着，对于项目 ，设置 gpg,

最后，提交的时候开启 签名；

**修改签名信息**

```
// 进入gpg
gpg --edit-key <email/key_id>

// 选择 uid 1
uid 1
// 删除
deluid
// add 
adduid
// 信任
trust
// 保存
save
// 对于已上传到密钥服务器的，不想更换，可以进行吊销
revuid
// 更多指定可使用 help 看看
```

# 7、Mac 安装 Adobe Creative Cloud 不显示 app.

> 有：you don't have access to manage apps...类似的提示，不显示 app，解决办法。

找到该文件：`/Library/Application Support/Adobe/OOBE/Configs/ServiceConfig.xml`，内容如下：

```
<config><panel><name>AppsPanel</name><visible>false</visible></panel><feature><name>SelfServeInstalls</name><enabled>false</enabled></feature></config>

```
copy 改文件，将 `visible>false</visible>`中的 `false` 改成 `true`，然后替换源文件，推出软件重新登录即可显示 app。








