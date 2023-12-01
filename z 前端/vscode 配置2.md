

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
