# Mac 装机记录

[余腾靖](https://juejin.cn/user/2664871915684493/posts)

2023-05-2019693阅读13分钟

由于这两年公司裁了不少人，所以其实一直都是有不少 m1 机器可以换的，但是之前由于软件兼容性的问题一直没有换。今年工作内容变了，加上最近夏天快到了，温度越来越高，实在是忍不了那滚烫的 touch bar，于是决定换台 m1。

本文的写作目的：

- 分享一些在用的工具和系统优化技巧
- 做个记录，方便下次换机的时候可以有条不紊，快速恢复开发环境

## 机器配置

旧机器：

- MacBook Pro 2019 13 寸
- 1.4 GHz 四核 Intel Core i5
- 16 GB 内存 250G SSD

新机器：

- MacBook Pro 2020 13 寸
- M1 芯片
- 16G 内存 245G SSD

最后一台无刘海 + Touch bar + M 芯片的 mbp。

## 初始化系统

机器拿到手的时候是在选择系统语言界面，按部就班走流程，需要注意的有：

- 先不使用 Apple Id 登入，iCloud 并不支持同步系统设置，分辨率，手势设置都需要重新设置一遍，所以不急着登入。
- 用户账号使用自己的姓名拼音全拼，你也不想每次打开 terminal 对着一个意义不明的 home 目录名敲命令吧
- 不共享故障数据给苹果和开发者，尤其是对于重视自己隐私的用户

## 准备工作

- 更新最新的系统 Ventura 13.3.1 (a)
- 登入 Apple Id，目的是为了从 Apple Store 安装应用
- Apple Store 下载安装[微信](https://link.juejin.cn/?target=https%3A%2F%2Fweixin.qq.com%2F "https://weixin.qq.com/")，先装它的目的是为了手机上传文件过去
- Safari 下载安装[搜狗输入法](https://link.juejin.cn/?target=https%3A%2F%2Fpinyin.sogou.com%2Fmac%2F "https://pinyin.sogou.com/mac/")，不理解为什么有人喜欢折腾别的输入法。使用微信账号同步数据，我已经全面放弃 QQ 账号，即便是已经用了好几年的 QQ 音乐和腾讯视频。

## [Chrome](https://link.juejin.cn/?target=https%3A%2F%2Fwww.google.com%2Fchrome%2F "https://www.google.com/chrome/)

`Safari` 对我来说主要功能就是当做 `Chrome` 下载器。开启代理后，从 [www.google.com/chrome/](https://link.juejin.cn/?target=https%3A%2F%2Fwww.google.com%2Fchrome%2F "https://www.google.com/chrome/") 下载安装 `Chrome`，而不是去 [www.google.cn/intl/zh-CN/…](https://link.juejin.cn/?target=https%3A%2F%2Fwww.google.cn%2Fintl%2Fzh-CN%2Fchrome%2F "https://www.google.cn/intl/zh-CN/chrome/")。很多国内专属版应用总是被代理商加入各种奇奇怪怪的东西，最好还是从原始的官网下载。

我平常浏览器只用 `Chrome`，使用谷歌账号同步数据。`Chrome` 默认走的就是系统代理，配置好代理后登入自己的账号，等待插件同步完成。

### flags

打开 [chrome://flags](https://link.juejin.cn/?target=)，以下是我开启的 flags：

- Parallel downloading 强烈推荐，多线程下载加速，其它开启的 flag 只是做个记录，用到的时候再开
- Experimental QUIC protocol
- Experimental JavaScript
- Future V8 VM features
- Enable experimental cookie features
- Enable the battery saver mode feature in the settings
- Enable the high efficiency mode feature in the settings

### 一些日常在用的 Chrome 插件

- [Better History](https://link.juejin.cn/?target=https%3A%2F%2Fchrome.google.com%2Fwebstore%2Fdetail%2Fbetter-history%2Fegehpkpgpgooebopjihjmnpejnjafefi "https://chrome.google.com/webstore/detail/better-history/egehpkpgpgooebopjihjmnpejnjafefi") 我觉得 Chrome Devtools 团队是在做事情的，每个月都有惊喜。但感觉 Chrome 浏览器本身的产品经理正事不干，净添乱。Chrome 浏览器本身非常简陋，自带的历史记录很难用。
- [uBlock Origin]( https://link.juejin.cn/?target=https%3A%2F%2Fchrome.google.com%2Fwebstore%2Fdetail%2Fublock-origin%2Fcjpalhdlnbpafiamejdnhcphjbkeiagm " https://chrome.google.com/webstore/detail/ublock-origin/cjpalhdlnbpafiamejdnhcphjbkeiagm" ) 拦截网页广告
- ublacklist  过滤网站搜索结果，跟 csdn 说拜拜！
- [Vimium C](https://link.juejin.cn/?target=https%3A%2F%2Fchrome.google.com%2Fwebstore%2Fdetail%2Fvimium-c-all-by-keyboard%2Fhfjbmagddngcpeloejdejnfgbamkjaeg "https://chrome.google.com/webstore/detail/vimium-c-all-by-keyboard/hfjbmagddngcpeloejdejnfgbamkjaeg") 虽然我不用 VIM 写代码，但是用 VIM 的方式来操作网页确实挺方便。不用原版是因为原版不咋更新了，和这个国内开发者在 github issue 交流还是蛮愉快的。
- [Notifications Preview for GitHub](https://link.juejin.cn/?target=https%3A%2F%2Fchrome.google.com%2Fwebstore%2Fdetail%2Fnotifications-preview-for%2Fkgilejfahkjidpaclkepbdoeioeohfmj "https://chrome.google.com/webstore/detail/notifications-preview-for/kgilejfahkjidpaclkepbdoeioeohfmj") Github 虽然一直在做事，但太慢了，新的 Code Search 不错，可以替代 OctoTree。这个插件的功能不知道啥时候能内置，让你不用打开 issue 页面就预览消息页表，简直不要太方便。
- [Minimal Theme for Twitter](https://link.juejin.cn/?target=https%3A%2F%2Fchrome.google.com%2Fwebstore%2Fdetail%2Fminimal-theme-for-twitter%2Fpobhoodpcipjmedfenaigbeloiidbflp "https://chrome.google.com/webstore/detail/minimal-theme-for-twitter/pobhoodpcipjmedfenaigbeloiidbflp") 装上它后，刷 twitter 的瘾更大了，更喜欢摸鱼了
- [v2ex plus](https://link.juejin.cn/?target=https%3A%2F%2Fchrome.google.com%2Fwebstore%2Fdetail%2Fv2ex-plus%2Fdaeclijmnojoemooblcbfeeceopnkolo "https://chrome.google.com/webstore/detail/v2ex-plus/daeclijmnojoemooblcbfeeceopnkolo") 好像最近同类的插件在 V2EX 上卷起来了，但是我觉得这个已经够用了
- [Wappalyzer](https://link.juejin.cn/?target=https%3A%2F%2Fchrome.google.com%2Fwebstore%2Fdetail%2Fwappalyzer-technology-pro%2Fgppongmhjkpfnbhagpmjfkannfbllamg "https://chrome.google.com/webstore/detail/wappalyzer-technology-pro/gppongmhjkpfnbhagpmjfkannfbllamg") 查看网页用到的技术
- [JSON Viewer Pro](https://link.juejin.cn/?target=https%3A%2F%2Fchrome.google.com%2Fwebstore%2Fdetail%2Fjson-viewer-pro%2Feifflpmocdbdmepbjaopkkhbfmdgijcc "https://chrome.google.com/webstore/detail/json-viewer-pro/eifflpmocdbdmepbjaopkkhbfmdgijcc") 可能不是最好用的 JSON Viewer，但绝对是最好看的
- [SwitchyOmega](https://link.juejin.cn/?target=https%3A%2F%2Fchrome.google.com%2Fwebstore%2Fdetail%2Fproxy-switchyomega%2Fpadekgcemlokbadohgkifijomclgjgif "https://chrome.google.com/webstore/detail/proxy-switchyomega/padekgcemlokbadohgkifijomclgjgif") 用的最多的是反倒是切换系统代理和直连，其它可能用到的场景例如配置网页走 charles 代理，调试网页接口
- [Chrono]( https://link.juejin.cn/?target=https%3A%2F%2Fchrome.google.com%2Fwebstore%2Fdetail%2Fchrono-download-manager%2Fmciiogijehkdemklbdcbfkefimifhecn " https://chrome.google.com/webstore/detail/chrono-download-manager/mciiogijehkdemklbdcbfkefimifhecn" ) 上上一次更新好像还是几年前。今年忽然诈尸了
- 侧边翻译。
- continue to visit。跳过网页跳转的确认步骤直接跳转。

以上是部分我日常在用的插件，其实还有一些开着但不常用，以及装了但是平时禁用的插件就不介绍了，总共有 90 多个插件。好在 Chrome 支持同步安装的插件和插件数据，只要登入了 Google 账号就能把旧电脑上的插件同步过来了。

其中一些 github 相关的插件需要重新生成 token。

### [TamperMonkey Beta](https://link.juejin.cn/?target=https%3A%2F%2Fchrome.google.com%2Fwebstore%2Fdetail%2Ftampermonkey-beta%2Fgcalenpjmijncebpfijmoaglllgpjagf "https://chrome.google.com/webstore/detail/tampermonkey-beta/gcalenpjmijncebpfijmoaglllgpjagf")

用 Beta 是因为某些 api 稳定版没有。在旧的设备上利用它自带的备份功能将最新的脚本备份到 Google Driver，再在新设备上恢复最新的备份。

以下几个是日常在用的脚本：

- [Refined GitHub Reactions](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fpatak-dev%2Frefined-github-reactions "https://github.com/patak-dev/refined-github-reactions") 社区总是喜欢教 Github 做事
- [Anti Redirect](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Faxetroy%2Fanti-redirect "https://github.com/axetroy/anti-redirect") 去除重定向
- [Bilibili Evolved](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fthe1812%2FBilibili-Evolved "https://github.com/the1812/Bilibili-Evolved") B 站用户必备
- [DouyuEx](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fqianjiachun%2FdouyuEx "https://github.com/qianjiachun/douyuEx") 斗鱼用户必备

## 应用软件

以下是日常在用的，所以先安装：

- [Draw.io](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fjgraph%2Fdrawio-desktop "https://github.com/jgraph/drawio-desktop") 不会画流程图的程序员不是好程序员，注意下载的时候选择 arm64 版本
- [MonitorControl](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2FMonitorControl%2FMonitorControl "https://github.com/MonitorControl/MonitorControl") 调节外接显示器亮度
- [Obsidian](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fobsidianmd%2Fobsidian-releases "https://github.com/obsidianmd/obsidian-releases") 多数时候是用来记 TODO 的
- [QQ 音乐](https://link.juejin.cn/?target=https%3A%2F%2Fy.qq.com%2F "https://y.qq.com/") 版权最多的国内音乐平台，其实更多时候我是开着 Youtube 音乐合集，除非喜欢的歌手发了新歌，可能就用它单曲循环
- [Snipaste](https://link.juejin.cn/?target=https%3A%2F%2Fwww.snipaste.com%2F "https://www.snipaste.com/") 截图标注界的神
- [Typora](https://link.juejin.cn/?target=https%3A%2F%2Ftypora.io%2F "https://typora.io/") 付费用户，弥补了 Obsidian 不能单独打开 markdown 文件编辑的问题，暂时还是写博客的主力工具，后续可能会迁移到使用 Obsidian 写博客
- [柠檬清理](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2FTencent%2Flemon-cleaner "https://github.com/Tencent/lemon-cleaner") 腾讯开源的系统维护工具
- clashX 连接 VPN
- obsidian 笔记软件
- pa.per 壁纸软件
- alfred 快速切换查找应用/文件
- paste 复制粘贴历史
- switchhost 修改本机 host
- Hidden Bar  隐藏状态栏图标
- loop 支持快捷键分屏

以下很少用，用到的时候再安装：

- [Adobe After Effects](https://link.juejin.cn/?target=https%3A%2F%2Fwww.macw.com%2Fmac%2F3727.html "https://www.macw.com/mac/3727.html")
- [Adobe Photoshop](https://link.juejin.cn/?target=https%3A%2F%2Fwww.macw.com%2Fmac%2F4244.html "https://www.macw.com/mac/4244.html") 和 AE 一样都是工作内容强相关应用
- [Better Display](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fwaydabber%2FBetterDisplay "https://github.com/waydabber/BetterDisplay") 显示器设置工具
- [Charles](https://link.juejin.cn/?target=https%3A%2F%2Fwww.charlesproxy.com%2F "https://www.charlesproxy.com/") 以前调试移动端 app 用到过
- [IINA](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fiina%2Fiina "https://github.com/iina/iina") 视频播放器
- [Key Codes](https://link.juejin.cn/?target=https%3A%2F%2Fapps.apple.com%2Fcn%2Fapp%2Fkey-codes%2Fid414568915%3Fmt%3D12 "https://apps.apple.com/cn/app/key-codes/id414568915?mt=12") Debug Key Code 的时候很有用
- [KeyCastr](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fkeycastr%2Fkeycastr "https://github.com/keycastr/keycastr") 录屏时显示键盘输入
- [MacUpdater](https://link.juejin.cn/?target=https%3A%2F%2Fwww.corecode.io%2Fmacupdater%2F "https://www.corecode.io/macupdater/") 我不用它去更新，只用来查看哪些 app 可以更新，可能会考虑入正
- [Maccy](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fp0deje%2FMaccy "https://github.com/p0deje/Maccy") 剪贴板管理器，批量复制
- [OSS Browser](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Faliyun%2Foss-browser "https://github.com/aliyun/oss-browser") 之前把 CDN 当数据库存东西的时候，感觉图形化操作还是很方便的
- [Parallels Desktop](https://link.juejin.cn/?target=https%3A%2F%2Fwww.macw.com%2F "https://www.macw.com/") 之前测试一个 Electron 客户端的时候经常要在 Windows 上跑
- [PicGo](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2FMolunerfinn%2FPicGo "https://github.com/Molunerfinn/PicGo") 图床应用，自从 sm.ms 不支持国内用户后就不咋用了
- [Postman](https://link.juejin.cn/?target=https%3A%2F%2Fwww.postman.com%2Fdownloads%2F%3Futm_source%3Dpostman-home "https://www.postman.com/downloads/?utm_source=postman-home") 调式接口
- [QQ](https://link.juejin.cn/?target=https%3A%2F%2Fim.qq.com%2Fmacqq%2Findex.shtml "https://im.qq.com/macqq/index.shtml") 某些人只有 QQ 联系方式
- [Silicon](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2FDigiDNA%2FSilicon "https://github.com/DigiDNA/Silicon") 查看已经安装的软件哪些是 intel 芯片的哪些是 arm 的
- [ScreenFlow](https://link.juejin.cn/?target=https%3A%2F%2Fwww.macw.com%2Fmac%2F3537.html "https://www.macw.com/mac/3537.html") 录屏工具
- [Sourcetree](https://link.juejin.cn/?target=https%3A%2F%2Fwww.sourcetreeapp.com%2F "https://www.sourcetreeapp.com/") 其实 VSCode 内置的 Source Control + Gitlens（我是付费用户） 对我来说已经完全够用。
- [stats](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fexelban%2Fstats "https://github.com/exelban/stats") 有些系统参数还是得用它来看
- [Telegram](https://link.juejin.cn/?target=https%3A%2F%2Fmacos.telegram.org%2F "https://macos.telegram.org/") 一些隐私要求比较高的场景需要用它
- [Unarchiver](https://link.juejin.cn/?target=https%3A%2F%2Ftheunarchiver.com%2F "https://theunarchiver.com/") 解压工具
- [Warp](https://link.juejin.cn/?target=https%3A%2F%2Fwww.warp.dev%2F "https://www.warp.dev/") 骚气的现代 Terminal，不过对我说其实有点花里胡哨了，我连 Fig 补全都不用，[fzf-tab](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2FAloxaf%2Ffzf-tab "https://github.com/Aloxaf/fzf-tab") 对我来说足矣
- [WPS](https://link.juejin.cn/?target=https%3A%2F%2Fmac.wps.cn%2F "https://mac.wps.cn/") 上一次用它还是帮我姐处理 doc 上照片格式问题
- [百度网盘](https://link.juejin.cn/?target=https%3A%2F%2Fpan.baidu.com%2Fdownload%23pan "https://pan.baidu.com/download#pan")
- [迅雷](https://link.juejin.cn/?target=https%3A%2F%2Fmac.xunlei.com%2F "https://mac.xunlei.com/") 国内下载磁力速度这块真没能打得过迅雷的吧？不过现在动不动就因为版权问题下载不了也是很久没用了
- [速享](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fnightmare-space%2Fspeed_share "https://github.com/nightmare-space/speed_share") 应该是最好的局域网跨端传输客户端工具
- [向日葵](https://link.juejin.cn/?target=https%3A%2F%2Fsunlogin.oray.com%2F "https://sunlogin.oray.com/") 偶尔需要远程同事电脑

## 安装字体

- [Fira Code](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Ftonsky%2FFiraCode "https://github.com/tonsky/FiraCode") 代码字体
- [Meslo Nerd Font](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fromkatv%2Fpowerlevel10k%23meslo-nerd-font-patched-for-powerlevel10k "https://github.com/romkatv/powerlevel10k#meslo-nerd-font-patched-for-powerlevel10k") 终端字体, 支持显示各种图标，这个后面配置 powerlevel10k 的时候就会自动安装

## 开发环境配置

### [VSCode](https://link.juejin.cn/?target=https%3A%2F%2Fcode.visualstudio.com%2F "https://code.visualstudio.com/")

我用的是稳定版的 `VSCode`，安装的时候注意选择苹果芯片版本的安装包。

之前有一段时间一直用的是 `insiders` 版本，现在我只用稳定版：

- `insiders` 版本有时候会出现严重 bug，而且还不是一两天就能修好，有些问题都没法 reproduce，会影响我开发进度
- 很多插件没有对 `insiders` 版做适配和测试

`VSCode` 自带设置同步功能，我选择使用 `Github` 账号登入。

配置备份
```
{

"files.associations": {

"*.vue": "vue",

"*.wpy": "vue",

"*.wxml": "html",

"*.wxss": "css",

"*.cjson": "jsonc",

"*.wxs": "javascript"

},

"git.enableSmartCommit": true,

"git.autofetch": true,

"emmet.showAbbreviationSuggestions": true,

"emmet.showExpandedAbbreviation": "always",

"emmet.includeLanguages": {

"vue-html": "html",

"vue": "html",

"wpy": "html",

"wxml": "html"

},

"markJump.maximumLimit": -1,

"editor.fontWeight": "500",

"git.confirmSync": false,

"explorer.confirmDelete": false,

"editor.wordWrap": "on",

"editor.detectIndentation": false,

// 重新设定tabsize

"editor.tabSize": 2,

//每120行就显示一条线

"editor.rulers": [120],

// 在使用搜索功能时，将这些文件夹/文件排除在外

"search.exclude": {

"**/node_modules": true,

"**/bower_components": true,

"**/target": true,

"**/logs": true

},

// 这些文件将不会显示在工作空间中

"files.exclude": {

"**/.DS_Store": true,

"**/.git": true,

"**/.hg": true,

"**/.svn": true,

"**/*.js": {

"when": "$(basename).ts"

},

"**/CVS": true

},

// #让vue中的js按"prettier"格式进行格式化

"vetur.format.defaultFormatter.html": "js-beautify-html",

"vetur.format.defaultFormatter.js": "prettier",

"vetur.format.defaultFormatterOptions": {

"js-beautify-html": {

// #vue组件中html代码格式化样式

"wrap_attributes": "force-aligned", //也可以设置为“auto”，效果会不一样

"wrap_line_length": 200,

"end_with_newline": false,

"semi": false,

"singleQuote": true

},

"prettier": {

"semi": false,

"singleQuote": true

}

},

"vetur.validation.template": false,

"minapp-vscode.disableAutoConfig": true,

"eslint.validate": [

"javascript",

"javascriptreact",

{

"language": "html",

"autoFix": true

},

{

"language": "vue",

"autoFix": true

}

],

"javascript.updateImportsOnFileMove.enabled": "never",

"workbench.startupEditor": "newUntitledFile",

"i18n-ally.displayLanguage": "cn",

"terminal.integrated.rendererType": "dom",

"commentTranslate.targetLanguage": "zh-CN",

"code-runner.defaultLanguage": "javascript",

"code-runner.executorMap": {

"javascript": "node"

},

"sync.gist": "a8048af23a5ee669d5db4d04974cb3d3",

"sync.forceUpload": true,

"editor.codeActionsOnSave": {

"source.fixAll.eslint": "explicit"

},

"explorer.confirmDragAndDrop": false,

"editor.renderControlCharacters": true,

"editor.minimap.enabled": false,

"breadcrumbs.enabled": true,

"editor.renderWhitespace": "none",

"vscodePluginDemo.showTip": false,

"cSpell.language": "en",

"cSpell.enableFiletypes": ["vue"],

"terminal.external.osxExec": "iTerm.app",

"editor.wordSeparators": "`~!@#$%^&*()=+[{]}\\|;:'\",.<>/?",

"path-intellisense.mappings": {

"/": "${workspaceRoot}",

"@": "${workspaceRoot}/src",

"utils": "${workspaceRoot}/src/utils",

"redux-persist": "${workspaceRoot}/src/packages/redux-persist",

"redux-persist-transform-expire": "${workspaceRoot}/src/packages/redux-persist/redux-persist-transform-expire",

"sm-api-service": "${workspaceRoot}/src/packages/sm-api-service",

"sm-frame-mp": "${workspaceRoot}/src/packages/sm-frame-mp",

"sm-redux-core": "${workspaceRoot}/src/packages/sm-redux-core",

"sm-redux-persist": "${workspaceRoot}/src/packages/sm-redux-persist",

"sm-sa": "${workspaceRoot}/src/packages/sm-sa",

"sm-socket": "${workspaceRoot}/src/packages/sm-socket",

"imports": "${workspaceRoot}/src/imports",

"images": "${workspaceRoot}/src/images",

"mixins": "${workspaceRoot}/src/mixins",

"configs": "${workspaceRoot}/src/configs",

"action-types": "${workspaceRoot}/src/store/types",

"store": "${workspaceRoot}/src/store",

"models": "${workspaceRoot}/src/models",

"api": "${workspaceRoot}/src/services/api",

"socket": "${workspaceRoot}/src/services/web-socket",

"constant": "${workspaceRoot}/src/constant"

},

"path-intellisense.showHiddenFiles": true,

"path-intellisense.extensionOnImport": true,

"typescript.updateImportsOnFileMove.enabled": "always",

"terminal.integrated.fontFamily": "monospace",

"settingsSync.ignoredSettings": ["terminal.integrated.fontFamily"],

"editor.fontFamily": "Consolas",

"workbench.preferredHighContrastColorTheme": "Default Dark+",

"workbench.preferredLightColorTheme": "Visual Studio Dark",

"cSpell.userWords": [

"ahooks",

"antd",

"bindtouchend",

"dissoc",

"echarts",

"elif",

"iconfont",

"keyframes",

"longpress",

"maxlength",

"miniprogram",

"mixins",

"ramda",

"rgba",

"SAAS",

"supermonkey",

"swiper",

"tarojs",

"typeof",

"unnest",

"wepy",

"wxss"

],

"editor.multiCursorModifier": "alt",

"editor.codeLens": true,

"gitHistory.sideBySide": true,

"turboConsoleLog.quote": "'",

"turboConsoleLog.insertEnclosingClass": false,

"turboConsoleLog.includeFileNameAndLineNum": false,

"turboConsoleLog.logMessagePrefix": "🚀-",

"turboConsoleLog.delimiterInsideMessage": " ->",

"terminal.integrated.scrollback": 20000,

"workbench.iconTheme": "vscode-icons",

"debug.onTaskErrors": "debugAnyway",

"[javascript]": {

"editor.defaultFormatter": "esbenp.prettier-vscode"

},

"[typescriptreact]": {

"editor.defaultFormatter": "esbenp.prettier-vscode"

},

"editor.unicodeHighlight.allowedCharacters": {

"：": true,

"（": true

},

"editor.quickSuggestions": {

"strings": true

},

"[html]": {

"editor.defaultFormatter": "esbenp.prettier-vscode"

},

"editor.unicodeHighlight.allowedLocales": {

"zh-hant": true

},

"prettier.endOfLine": "auto",

"projectManager.sortList": "Recent",

"projectManager.tags": ["Work", "Dependencies", "OldRepo", "selfRepo"],

"[vue]": {

"editor.defaultFormatter": "hu2ren.vetur-wepy"

},

"vetur.validation.style": false,

"vetur.validation.script": false,

"projectManager.git.baseFolders": ["/Users/yangjiajian/Work/workspace/"],

"[typescript]": {

"editor.defaultFormatter": "esbenp.prettier-vscode"

},

"workbench.tree.renderIndentGuides": "always",

"debug.console.fontSize": 13,

"gulp.autoDetect": "on",

"files.autoGuessEncoding": true,

"gitlens.graph.layout": "editor",

"typeChallenges.workspaceFolder": "/Users/yangjiajian/.typeChallenges",

"[json]": {

"editor.defaultFormatter": "vscode.json-language-features"

},

"prettier.printWidth": 120,

"diffEditor.ignoreTrimWhitespace": false,

"workbench.colorTheme": "Atom One Dark",

"explorer.autoRevealExclude": {

"": true,

"**/node_modules": false

},

"github.copilot.advanced": {

"debug.overrideChatEngine": "gpt-4"

},

"terminal.integrated.defaultProfile.osx": "zsh (2)",

"editor.fontSize": 14,

"editor.lineHeight": 17,

"terminal.integrated.stickyScroll.enabled": true,

"terminal.integrated.tabs.location": "left",

"gitlens.views.remotes.branches.layout": "list",

"workbench.tree.enableStickyScroll": true,

"workbench.list.smoothScrolling": true,

"comment_alias-skip": "解决wpy无法正常跳转到导入路径的问题",

"alias-skip.mappings": {

"constant": "/src/constant",

"http-api": "/src/setup/http",

"redux-plugin": "/src/plugins/redux"

},

"terminal.integrated.env.osx": {

"PATH": "/Users/yangjiajian/.volta/bin:/opt/homebrew/bin:/opt/homebrew/sbin:/usr/local/bin:$PATH",

"FIG_NEW_SESSION": "1"

},

"extensions.autoUpdate": "onlySelectedExtensions",

"extensions.autoCheckUpdates": false,

"multiDiffEditor.experimental.enabled": true,

"gitlens.views.worktrees.files.layout": "list",

"quokka.compactMessageOutput": true,

"quokka.automaticStartRegex": "\\.(tsx,ts,js)$\\",

"vscode_custom_css.imports": [

"file:///Users/yangjiajian/.vscode/extensions/brandonkirbyson.vscode-animations-2.0.1/dist/updateHandler.js"

],

"emmet.triggerExpansionOnTab": true,

"gitMergeBranchTo.feishuId": "6943778355237912604",

"gitMergeBranchTo.deployConfig": {

"projectList": ["monkey-cms-web-new", "monkey-saas-enterprise-web", "monkey-saas-web"],

"urlConfig": [

{

"env": "fat",

"defaultBranch": "develop",

"clientWebhookList": [],

"serverWebhookMap": {

"monkey-cms-web-new": {

"hookUrl": "http://flow-openapi.aliyun.com/pipeline/webhook/CBOjeSVVg1OiJwYKcz8I",

"webUrl": "https://flow.aliyun.com/pipelines/2956295/current"

},

"monkey-saas-enterprise-web": {

"hookUrl": "http://flow-openapi.aliyun.com/pipeline/webhook/JELRY26yPK6JWy4y71PP",

"webUrl": "https://flow.aliyun.com/pipelines/2952820/current"

},

"monkey-saas-web": {

"hookUrl": "http://flow-openapi.aliyun.com/pipeline/webhook/5XzBF3s7ncdVxZANu9oR",

"webUrl": "https://flow.aliyun.com/pipelines/1782215/current"

}

}

},

{

"env": "dev",

"defaultBranch": "develop",

"clientWebhookList": [],

"serverWebhookMap": {

"monkey-cms-web-new": {

"hookUrl": "http://flow-openapi.aliyun.com/pipeline/webhook/IQJpnlAfnzicJul0WIKK",

"webUrl": "https://flow.aliyun.com/pipelines/2948323/current"

},

"monkey-saas-enterprise-web": {

"hookUrl": "http://flow-openapi.aliyun.com/pipeline/webhook/utlDL7tWtg7rta26nzSB",

"webUrl": "https://flow.aliyun.com/pipelines/1880194/current"

},

"monkey-saas-web": {

"hookUrl": "http://flow-openapi.aliyun.com/pipeline/webhook/aMBKlOzUDQvYf1yMe9e7",

"webUrl": "https://flow.aliyun.com/pipelines/1782218/current"

}

}

},

{

"env": "uat",

"defaultBranch": "develop",

"clientWebhookList": [],

"serverWebhookMap": {

"monkey-saas-web": {

"hookUrl": "http://flow-openapi.aliyun.com/pipeline/webhook/CR91Qj1MAe85lYoZKXl1",

"webUrl": "https://flow.aliyun.com/pipelines/1782216/current"

},

"monkey-saas-enterprise-web": {

"hookUrl": "http://flow-openapi.aliyun.com/pipeline/webhook/4K6Px3gMLrQipIQCi4lR",

"webUrl": "https://flow.aliyun.com/pipelines/2965729/current"

},

"monkey-cms-web-new": {

"hookUrl": "http://flow-openapi.aliyun.com/pipeline/webhook/XXjV0AEkhCXjDplkhjxY",

"webUrl": "https://flow.aliyun.com/pipelines/2956305/current"

}

}

},

{

"env": "fat1",

"defaultBranch": "develop",

"clientWebhookList": [],

"serverWebhookMap": {

"monkey-saas-web": {

"hookUrl": "http://flow-openapi.aliyun.com/pipeline/webhook/lpoSNFHuMP0fPwXj3X5c",

"webUrl": "https://flow.aliyun.com/pipelines/2979935/current"

},

"monkey-cms-web-new": {

"hookUrl": "http://flow-openapi.aliyun.com/pipeline/webhook/JC1YmMa8pm8tFlSsMHDX",

"webUrl": "https://flow.aliyun.com/pipelines/2979934/current"

}

}

}

]

},

"gitMergeBranchTo.branches": ["develop", "uat"],

"application.shellEnvironmentResolutionTimeout": 18,

"Lingma.LocalStoragePath": "/Users/yangjiajian/.lingma"

}
```



### [Iterm2](https://link.juejin.cn/?target=https%3A%2F%2Fiterm2.com%2F "https://iterm2.com/")

其实我日常用的最多的 terminal 是 `VSCode` 的集成 terminal，其次就是 `iterm2`。`Warp` 暂时还没有啥吸引我转过去的杀手级优势，反倒很多功能都是没有的，例如 Key Mapping。

安装完后切换 `Minimal` 主题。

### [HomeBrew](https://link.juejin.cn/?target=https%3A%2F%2Fbrew.sh%2F "https://brew.sh/")

[安装步骤](https://gitee.com/cunkai/HomebrewCN)

### 安装常用命令行工具

使用 brew 安装以下命令行工具：

- [git](https://link.juejin.cn/?target=https%3A%2F%2Fgit-scm.com%2F "https://git-scm.com/") 替换苹果内置的 git，方便后续通过 brew 更新
- [lsd](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Flsd-rs%2Flsd "https://github.com/lsd-rs/lsd") ls 命令替代品
- [fd](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fsharkdp%2Ffd%23installation "https://github.com/sharkdp/fd#installation") find 命令替代品
- [rg](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2FBurntSushi%2Fripgrep "https://github.com/BurntSushi/ripgrep") 最强大的文本搜索根据，VSCode 内置的搜索也是用的它
- [fzf]( https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fjunegunn%2Ffzf " https://github.com/junegunn/fzf" ) 模糊搜索工具
- [volta]( https://volta.sh/) node 版本管理包

通过命令 `brew leaves` 输出所有手动安装的包（排除依赖包）：

```
volta
zsh
```

### 配置 shell

#### [oh-my-zsh](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fohmyzsh%2Fohmyzsh "https://github.com/ohmyzsh/ohmyzsh")

zsh 框架。

修改以下配置：
`
```
# 时间格式
HIST_STAMPS="yyyy-mm-dd" 
# 设置语言问英文，这样很多命令行工具就不会输出中文了，例如 git clone
export LANG=en_US.UTF-8
export LC_ALL=en_US.UTF-8 export LC_CTYPE=en_US.UTF-8
```
`

安装以下非官方插件：

- [zsh-autosuggestions](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fzsh-users%2Fzsh-autosuggestions "https://github.com/zsh-users/zsh-autosuggestions")
- [zsh-syntax-highlighting](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fzsh-users%2Fzsh-syntax-highlighting "https://github.com/zsh-users/zsh-syntax-highlighting")
- [you-should-use](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fsearch%3Fq%3Dyou-should-use%26type%3Drepositories "https://github.com/search?q=you-should-use&type=repositories")
- [fzf-tab](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2FAloxaf%2Ffzf-tab "https://github.com/Aloxaf/fzf-tab") 强烈推荐，也是我为啥一直没有用 [fig](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fwithfig%2Fautocomplete "https://github.com/withfig/autocomplete") 和 warp 的原因

#### [powerlevel10k](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fromkatv%2Fpowerlevel10k "https://github.com/romkatv/powerlevel10k")

zsh prompts，安装完后初次打开 terminal 会运行 `p10k configure`，下载字体需要走代理，因此你可能需要先开启全局代理。

修改以下配置：
`
```
# 显示具体的错误码
typeset -g POWERLEVEL9K_STATUS_ERROR=true
# 命令执行时间精度改为 ms
typeset -g POWERLEVEL9K_COMMAND_EXECUTION_TIME_PRECISION=1
# 修改默认 Nodejs 图标
typeset -g POWERLEVEL9K_NODE_ICON='\uF898'
```
`

如果你问我为啥不用 [starship](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fstarship%2Fstarship "https://github.com/starship/starship")，我的回答是：

- 对我来说，好像它比起 p10k 没啥优势
- p10k 杀手级的优势就是它交互式的配置了吧
- 懒得折腾，p10k 已经很够用，碰到问题我提 issue 半天内就能收到作者回复

### nodejs

使用 volta 管理 node 版本，安装最新的 lts node V18.16.0。


#### [rust](https://link.juejin.cn/?target=https%3A%2F%2Fwww.rust-lang.org%2Ftools%2Finstall "https://www.rust-lang.org/tools/install")

直接按照官网提示使用 `rustup` 安装 `rust`。

使用 `cargo install --list` 列出 cargo 全局安装的命令行工具。

#### [golang](https://link.juejin.cn/?target=https%3A%2F%2Fgo.dev%2Fdl%2F "https://go.dev/dl/")

直接从官网下载安装器安装最新的 Golang。

使用 `ls $GOPATH/bin` 列出所有全局安装的命令行工具。
`
```
clash-speedtest  github-compare   go-global-update
```
`

#### python

使用 [pyenv](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fpyenv%2Fpyenv "https://github.com/pyenv/pyenv") 管理 python 版本。

## 系统设置优化

### 关闭用不到的服务

- Siri，并且从 touch 移除
- 自动亮度，显示器 -> 自动亮度 -> 关闭，我不用自动调节亮度，平时都是亮度拉满。

### 轻点来点按

系统设置 -> 触控板 -> 轻点来点按

### 三指拖移

系统设置 -> 辅助功能 -> 触控板选项 -> 拖移样式 -> 三指拖移

这不比按住拖移方便一万倍？

### 按键速率

系统设置 -> 键盘:

- 键重复速率 拉到最右边，最快
- 重复前的延迟 拉到右边第二个，第二短

这就是同事问我为啥么我的机器按键这么丝滑的原因。

### 指针控制

系统设置 -> 辅助功能 -> 指针控制：

- 连按速度，右边第三个
- 弹开载入速度，右边第三个

### 安装软件

安全性 -> 运行从以下位置下载的应用程序 -> Apple Store 和被认可的开发者

### 桌面优化

- 把用不到的内置应用全部收到一个文件夹
- 把几个常用内置应用拖到程序坞
    - 系统设置
    - 活动监视器
    - QuickTime Player
    - 字体册 开发字体需求的时候经常要用

## 数据同步

### iCloud

能用 `iCloud` 同步的都用 `iCloud` 同步：

- 文档，包括简历，晋升 ppt 之类的
- 代理配置

### 壁纸

因为我电脑上图片不多，我是把整个 `~/Pictures` 目录当做一个 git 仓库管理，使用 github 私有仓库同步。

### 代码

所有的代码我都用 git 管理，都统一放到 `~/code/` 目录下，在销毁旧电脑数据之前，注意将最新的代码变更同步到 github 或者公司内部的 git 托管服务器。

### 序列号

很多付软件的序列号或者激活码有激活设备数量限制，在你销毁电脑之前记得先取消激活。我所有的付费项目用的都是统一一个邮箱，需要激活码的时候直接取谷歌邮箱搜索。

现在很多平台都有二次认证，例如 github 和 npm，它们的 recovery code 我都是使用 github 的私有 gist 来备份。

### dotfiles

这里我指的是在 `home` 目录下的一堆点开头的配置文件：

例如：

- .gitconfig 全局的 `git` 配置文件
- .my-spell-check.txt [spell check](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fstreetsidesoftware%2Fvscode-spell-checker "https://github.com/streetsidesoftware/vscode-spell-checker") 配置文件
- .zshrc `zsh` 配置文件
- .p10k.zsh `powerlevel10k` 配置文件
- .npmrc `npm` 全局配置文件
- .prettierrc.js 全局的 `prettier` 配置文件

以及其它一些配置文件我都是使用一个私有的 github 仓库做备份同步。

总之，github 大法好，感谢微软财大气粗，私有仓库免费不限量。

## 销毁旧电脑数据

- 需要同步的数据同步最新数据
- 退出 apple 账号
- 退出 google 账号
- 删除 chrome 所有本机数据
- 退出应用登入
- 取消激活应用
- 将涉及到隐私的数据全删除
- 清空回收站
- 归还电脑