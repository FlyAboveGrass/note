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

"editor.defaultFormatter": "esbenp.prettier-vscode"

},

"breadcrumbs.enabled": true,

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

"cursor.cpp.disabledLanguages": [

"plaintext",

"markdown",

"scminput"

],

"cursor.cpp.enablePartialAccepts": true,

"debug.console.fontSize": 13,

"debug.onTaskErrors": "debugAnyway",

"diffEditor.hideUnchangedRegions.enabled": true,

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

"editor.fontSize": 16,

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

"editor.rulers": [

120

],

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

"wxml": "html"

},

"emmet.showAbbreviationSuggestions": true,

"emmet.showExpandedAbbreviation": "always",

"emmet.triggerExpansionOnTab": true,

"eslint.validate": [

"javascript",

"javascriptreact",

"typescript",

"typescriptreact",

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

"files.associations": {

"*.cjson": "jsonc",

"*.vue": "vue",

"*.wpy": "vue",

"*.wxml": "html",

"*.wxs": "javascript",

"*.wxss": "css"

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

"gitMergeBranchTo.branches": [

"develop",

"uat"

],

"gitMergeBranchTo.deployConfig": {

"projectList": [

"monkey-cms-web-new",

"monkey-saas-enterprise-web",

"monkey-saas-web"

],

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

"gitlens.graph.layout": "editor",

"gitlens.views.remotes.branches.layout": "list",

"gitlens.views.scm.grouped.views": {

"branches": false,

"commits": false,

"contributors": false,

"launchpad": false,

"remotes": false,

"repositories": false,

"searchAndCompare": false,

"stashes": false,

"tags": false,

"worktrees": false

},

"gitlens.views.worktrees.files.layout": "list",

"javascript.updateImportsOnFileMove.enabled": "never",

"multiDiffEditor.experimental.enabled": true,

"path-intellisense.extensionOnImport": true,

"path-intellisense.showHiddenFiles": true,

"prettier.endOfLine": "auto",

"prettier.printWidth": 120,

"projectManager.git.baseFolders": [

"/Users/yangjiajian/Work/workspace/"

],

"projectManager.tags": [

"Work",

"Dependencies",

"OldRepo",

"selfRepo"

],

// 在使用搜索功能时，将这些文件夹/文件排除在外

"search.exclude": {

"**/bower_components": true,

"**/logs": true,

"**/node_modules": true,

"**/target": true

},

"stylelint.snippet": [

"css",

"less",

"postcss",

"vue"

],

"stylelint.validate": [

"css",

"less",

"postcss",

"vue"

],

"terminal.integrated.accessibleViewFocusOnCommandExecution": true,

"terminal.integrated.defaultProfile.osx": "zsh",

"terminal.integrated.env.osx": {

"FIG_NEW_SESSION": "1",

"PATH": "/Users/yangjiajian/.volta/bin:/opt/homebrew/bin:/opt/homebrew/sbin:/usr/local/bin:$PATH"

},

"terminal.integrated.fontFamily": "monospace",

"terminal.integrated.profiles.osx": {

"bash": {

"args": [

"-l"

],

"icon": "terminal-bash",

"path": "bash"

},

"fish": {

"args": [

"-l"

],

"path": "fish"

},

"pwsh": {

"icon": "terminal-powershell",

"path": "pwsh"

},

"tmux": {

"icon": "terminal-tmux",

"path": "tmux"

},

"zsh": {

"args": [

"-l"

],

"path": "zsh"

}

},

"terminal.integrated.scrollback": 20000,

"terminal.integrated.stickyScroll.enabled": true,

"terminal.integrated.tabs.description": "${task}",

"terminal.integrated.tabs.location": "left",

"terminal.integrated.tabs.title": "${task}",

"turboConsoleLog.delimiterInsideMessage": " ->",

"turboConsoleLog.includeFileNameAndLineNum": false,

"turboConsoleLog.insertEnclosingClass": false,

"turboConsoleLog.logMessagePrefix": "🚀-",

"turboConsoleLog.quote": "'",

"typeChallenges.workspaceFolder": "/Users/yangjiajian/.typeChallenges",

"typescript.updateImportsOnFileMove.enabled": "always",

"vetur.grammar.customBlocks": {

"docs": "md",

"i18n": "json"

},

"vetur.validation.script": false,

"vetur.validation.style": false,

"vsicons.dontShowNewVersionMessage": true,

"workbench.activityBar.orientation": "vertical",

"workbench.colorTheme": "One Dark Pro",

"workbench.editor.decorations.colors": false,

"workbench.iconTheme": "vscode-icons",

"workbench.layoutControl.enabled": false,

"workbench.list.smoothScrolling": true,

"workbench.preferredHighContrastColorTheme": "Default Dark+",

"workbench.preferredLightColorTheme": "Visual Studio Dark",

"workbench.tree.enableStickyScroll": true,

"workbench.tree.renderIndentGuides": "always"

}
```
