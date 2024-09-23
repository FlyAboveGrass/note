

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

"git.confirmSync": false,

"emmet.triggerExpansionOnTab": true,

"emmet.showAbbreviationSuggestions": true,

"emmet.showExpandedAbbreviation": "always",

"emmet.includeLanguages": {

"vue-html": "html",

"vue": "html",

"wpy": "html",

"wxml": "html"

},

"explorer.confirmDelete": false,

"explorer.confirmDragAndDrop": false,

"editor.fontWeight": "500",

"editor.wordWrap": "on",

"editor.detectIndentation": false,

// 重新设定tabsize

"editor.tabSize": 2,

//每120行就显示一条线

"editor.rulers": [],

"editor.codeActionsOnSave": {

"source.fixAll.eslint": true

},

"editor.renderControlCharacters": true,

"editor.minimap.enabled": false,

"editor.renderWhitespace": "none",

"editor.wordSeparators": "`~!@#$%^&*()=+[{]}\\|;:'\",.<>/?",

"editor.unicodeHighlight.allowedCharacters": {

"：": true,

"（": true

},

"editor.quickSuggestions": {

"strings": true

},

"editor.unicodeHighlight.allowedLocales": {

"zh-hant": true

},

"editor.fontSize": 14,

"editor.fontFamily": "Consolas",

"[html]": {

"editor.defaultFormatter": "esbenp.prettier-vscode"

},

"editor.multiCursorModifier": "alt",

"editor.codeLens": true,

"cSpell.language": "en",

"cSpell.enableFiletypes": [

"vue"

],

"terminal.external.osxExec": "iTerm.app",

// 在使用搜索功能时，将这些文件夹/文件排除在外

"search.exclude": {

"**/node_modules": true,

"**/bower_components": true,

"**/target": true,

"**/logs": true,

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

"i18n-ally.displayLanguage": "cn",

"terminal.integrated.rendererType": "dom",

"commentTranslate.targetLanguage": "zh-CN",

"code-runner.defaultLanguage": "javascript",

"code-runner.executorMap": {

"javascript": "node",

},

"sync.gist": "a8048af23a5ee669d5db4d04974cb3d3",

"sync.forceUpload": true,

  

"path-intellisense.mappings": {

  

"/": "${workspaceFolder}",

"@": "${workspaceFolder}/src",

"utils": "${workspaceFolder}/src/utils",

"redux-persist": "${workspaceFolder}/src/packages/redux-persist",

"redux-persist-transform-expire": "${workspaceFolder}/src/packages/redux-persist/redux-persist-transform-expire",

"sm-api-service": "${workspaceFolder}/src/packages/sm-api-service",

"sm-frame-mp": "${workspaceFolder}/src/packages/sm-frame-mp",

"sm-redux-core": "${workspaceFolder}/src/packages/sm-redux-core",

"sm-redux-persist": "${workspaceFolder}/src/packages/sm-redux-persist",

"sm-sa": "${workspaceFolder}/src/packages/sm-sa",

"sm-socket": "${workspaceFolder}/src/packages/sm-socket",

"imports": "${workspaceFolder}/src/imports",

"images": "${workspaceFolder}/src/images",

"mixins": "${workspaceFolder}/src/mixins",

"configs": "${workspaceFolder}/src/configs",

"action-types": "${workspaceFolder}/src/store/types",

"store": "${workspaceFolder}/src/store",

"models": "${workspaceFolder}/src/models",

"api": "${workspaceFolder}/src/services/api",

"socket": "${workspaceFolder}/src/services/web-socket",

"constant": "${workspaceFolder}/src/assets/js/constant.js"

},

"path-intellisense.showHiddenFiles": true,

"path-intellisense.extensionOnImport": true,

"typescript.updateImportsOnFileMove.enabled": "always",

"terminal.integrated.fontFamily": "monospace",

"settingsSync.ignoredSettings": [

"terminal.integrated.fontFamily"

],

"cSpell.userWords": [

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

"typeof",

"unnest",

"wepy",

"wxss"

],

"gitHistory.sideBySide": true,

"terminal.integrated.tabs.location": "left",

"leek-fund.funds": [

[

"320007",

"001156",

"005506",

"006228",

"008280",

"720001",

"013082",

"008190",

"010052",

"007531",

"002199",

"400015",

"011036",

"004433",

"004643",

"008115",

"003634",

"012414",

"001551",

"000001"

]

],

"leek-fund.fundSort": -1,

"leek-fund.flash-news": false,

"leek-fund.hideStatusBarStock": true,

"turboConsoleLog.quote": "'",

"turboConsoleLog.insertEnclosingClass": false,

"turboConsoleLog.includeFileNameAndLineNum": false,

"turboConsoleLog.logMessagePrefix": "🚀-",

"turboConsoleLog.delimiterInsideMessage": " ->",

"terminal.integrated.scrollback": 20000,

"debug.onTaskErrors": "debugAnyway",

"[javascript]": {

"editor.defaultFormatter": "esbenp.prettier-vscode"

},

"[typescriptreact]": {

"editor.defaultFormatter": "esbenp.prettier-vscode"

},

"prettier.endOfLine": "auto",

"projectManager.sortList": "Saved",

"projectManager.tags": [

"Work",

"Dependencies",

"OldRepo",

"selfRepo"

],

"[vue]": {

"editor.defaultFormatter": "hu2ren.vetur-wepy"

},

"vetur.validation.style": false,

"vetur.validation.script": false,

"workbench.startupEditor": "newUntitledFile",

"workbench.iconTheme": "vscode-icons",

"workbench.colorTheme": "Default Light+",

"chatgpt.lang": "cn",

"editor.inlineSuggest.enabled": true,

"projectManager.git.baseFolders": [

"/Users/yangjiajian/Work/workspace/"

],

"workbench.layoutControl.enabled": false,

"github.copilot.enable": {

"*": true,

"plaintext": true,

"markdown": false,

"scminput": false

},

"gitlens.graph.layout": "editor",

"gitlens.graph.minimap.additionalTypes": [

"localBranches"

],

"git-graph.commitDetailsView.fileView.fileTree.compactFolders": false,

"prettier.printWidth": 120,

"git.mergeEditor": true,

"editor.formatOnSave": true

}
```










```
{

"[html]": {

"editor.defaultFormatter": "esbenp.prettier-vscode"

},

"[javascript]": {

"editor.defaultFormatter": "esbenp.prettier-vscode"

},

"[json]": {

"editor.defaultFormatter": "vscode.json-language-features"

},

"[typescript]": {

"editor.defaultFormatter": "esbenp.prettier-vscode"

},

"[typescriptreact]": {

"editor.defaultFormatter": "esbenp.prettier-vscode"

},

"[vue]": {

"editor.defaultFormatter": "hu2ren.vetur-wepy"

},

"application.shellEnvironmentResolutionTimeout": 18,

"breadcrumbs.enabled": true,

"cSpell.enableFiletypes": ["vue"],

"cSpell.language": "en",

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

"qiankun",

"ramda",

"rgba",

"SAAS",

"supermonkey",

"swiper",

"tarojs",

"typeof",

"umijs",

"unnest",

"wepy",

"wxpay",

"wxss"

],

"code-runner.defaultLanguage": "javascript",

"code-runner.executorMap": {

"javascript": "node"

},

"debug.console.fontSize": 13,

"debug.onTaskErrors": "debugAnyway",

"diffEditor.ignoreTrimWhitespace": false,

"editor.codeActionsOnSave": {

"source.fixAll.eslint": "explicit",

"source.fixAll.stylelint": "explicit"

},

"editor.codeLens": true,

"editor.cursorBlinking": "expand",

"editor.cursorSmoothCaretAnimation": "on",

"editor.detectIndentation": false,

"editor.fontFamily": "Consolas",

"editor.fontSize": 15,

"editor.fontWeight": "500",

"editor.lineHeight": 17,

"editor.minimap.enabled": false,

"editor.mouseWheelZoom": true,

"editor.multiCursorModifier": "alt",

"editor.quickSuggestions": {

"strings": true

},

"editor.renderControlCharacters": true,

"editor.renderWhitespace": "none",

//每120行就显示一条线

"editor.rulers": [120],

"editor.smoothScrolling": true,

// 重新设定tabsize

"editor.tabSize": 2,

"editor.unicodeHighlight.allowedCharacters": {

"（": true,

"：": true

},

"editor.unicodeHighlight.allowedLocales": {

"zh-hant": true

},

"editor.wordSeparators": "`~!@#$%^&*()=+[{]}\\|;:'\",.<>/?",

"editor.wordWrap": "on",

"emmet.includeLanguages": {

"vue": "html",

"vue-html": "html",

"wpy": "html",

"wxml": "html",

"postcss": "css"

},

"emmet.showAbbreviationSuggestions": true,

"emmet.showExpandedAbbreviation": "always",

"emmet.triggerExpansionOnTab": true,

"eslint.validate": [

"javascript",

"javascriptreact",

{

"autoFix": true,

"language": "html"

},

{

"autoFix": true,

"language": "vue"

}

],

"explorer.autoRevealExclude": {

"": true,

"**/node_modules": false

},

"explorer.confirmDelete": false,

"explorer.confirmDragAndDrop": false,

"extensions.autoCheckUpdates": false,

"files.associations": {

"*.cjson": "jsonc",

"*.vue": "vue",

"*.wpy": "vue",

"*.wxml": "html",

"*.wxs": "javascript",

"*.wxss": "css",

"*.css": "tailwindcss"

},

"files.autoGuessEncoding": true,

// 这些文件将不会显示在工作空间中

"files.exclude": {

"**/*.js": {

"when": "$(basename).ts"

},

"**/.DS_Store": true,

"**/.git": true,

"**/.hg": true,

"**/.svn": true,

"**/CVS": true

},

"git.autofetch": true,

"git.confirmSync": false,

"git.enableSmartCommit": true,

"gitHistory.sideBySide": true,

"gitMergeBranchTo.branches": ["develop", "uat"],

"gitMergeBranchTo.deployConfig": {

"projectList": ["monkey-cms-web-new", "monkey-saas-enterprise-web", "monkey-saas-web"],

"urlConfig": [

{

"clientWebhookList": [],

"defaultBranch": "develop",

"env": "fat",

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

"clientWebhookList": [],

"defaultBranch": "develop",

"env": "dev",

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

"clientWebhookList": [],

"defaultBranch": "uat",

"env": "uat",

"serverWebhookMap": {

"monkey-cms-web-new": {

"hookUrl": "http://flow-openapi.aliyun.com/pipeline/webhook/XXjV0AEkhCXjDplkhjxY",

"webUrl": "https://flow.aliyun.com/pipelines/2956305/current"

},

"monkey-saas-enterprise-web": {

"hookUrl": "http://flow-openapi.aliyun.com/pipeline/webhook/4K6Px3gMLrQipIQCi4lR",

"webUrl": "https://flow.aliyun.com/pipelines/2965729/current"

},

"monkey-saas-web": {

"hookUrl": "http://flow-openapi.aliyun.com/pipeline/webhook/CR91Qj1MAe85lYoZKXl1",

"webUrl": "https://flow.aliyun.com/pipelines/1782216/current"

}

}

},

{

"clientWebhookList": [],

"defaultBranch": "develop",

"env": "fat1",

"serverWebhookMap": {

"monkey-cms-web-new": {

"hookUrl": "http://flow-openapi.aliyun.com/pipeline/webhook/JC1YmMa8pm8tFlSsMHDX",

"webUrl": "https://flow.aliyun.com/pipelines/2979934/current"

},

"monkey-saas-web": {

"hookUrl": "http://flow-openapi.aliyun.com/pipeline/webhook/lpoSNFHuMP0fPwXj3X5c",

"webUrl": "https://flow.aliyun.com/pipelines/2979935/current"

}

}

}

]

},

"gitMergeBranchTo.feishuId": "6943778355237912604",

"github.copilot.advanced": {

"debug.overrideChatEngine": "gpt-4"

},

"github.copilot.editor.enableAutoCompletions": true,

"gitlens.graph.layout": "editor",

"gitlens.views.remotes.branches.layout": "list",

"gitlens.views.worktrees.files.layout": "list",

"gulp.autoDetect": "on",

"javascript.updateImportsOnFileMove.enabled": "never",

"multiDiffEditor.experimental.enabled": true,

"path-intellisense.extensionOnImport": true,

"path-intellisense.showHiddenFiles": true,

"prettier.endOfLine": "auto",

"prettier.printWidth": 120,

"projectManager.git.baseFolders": ["/Users/yangjiajian/Work/workspace/"],

"projectManager.sortList": "Recent",

"projectManager.tags": ["Work", "Dependencies", "OldRepo", "selfRepo"],

// 在使用搜索功能时，将这些文件夹/文件排除在外

"search.exclude": {

"**/bower_components": true,

"**/logs": true,

"**/node_modules": true,

"**/target": true

},

"settingsSync.ignoredSettings": ["terminal.integrated.fontFamily"],

"stylelint.snippet": ["css", "less", "postcss", "vue"],

"stylelint.validate": ["css", "less", "postcss", "vue"],

"npm.packageManager": "auto",

"terminal.integrated.automationProfile.osx": {

"path": "/opt/homebrew/bin/zsh"

},

"npm.scriptExplorerAction": "run",

"terminal.integrated.fontFamily": "monospace",

"terminal.integrated.profiles.osx": {

"zsh": {

"path": "/opt/homebrew/bin/zsh"

}

},

"terminal.integrated.scrollback": 20000,

"terminal.integrated.stickyScroll.enabled": true,

"terminal.integrated.tabs.location": "left",

"turboConsoleLog.delimiterInsideMessage": " ->",

"turboConsoleLog.includeFileNameAndLineNum": false,

"turboConsoleLog.insertEnclosingClass": false,

"turboConsoleLog.logMessagePrefix": "🚀-",

"turboConsoleLog.quote": "'",

"typeChallenges.workspaceFolder": "/Users/yangjiajian/.typeChallenges",

"typescript.updateImportsOnFileMove.enabled": "always",

"vetur.validation.script": false,

"vetur.validation.style": false,

"workbench.colorTheme": "One Dark Pro",

"workbench.iconTheme": "vscode-icons",

"workbench.list.smoothScrolling": true,

"workbench.preferredHighContrastColorTheme": "Default Dark+",

"workbench.preferredLightColorTheme": "Visual Studio Dark",

"workbench.startupEditor": "newUntitledFile",

"workbench.tree.enableStickyScroll": true,

"workbench.tree.renderIndentGuides": "always",

"workbench.layoutControl.enabled": false,

"window.commandCenter": false,

"vetur.grammar.customBlocks": {

"docs": "md",

"i18n": "json"

},

"terminal.integrated.accessibleViewFocusOnCommandExecution": true,

"npm.enableRunFromFolder": true,

"task.verboseLogging": true,
"terminal.integrated.env.osx": {

"PATH": "/Users/yangjiajian/.volta/bin"

},

}
```