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

"emmet.triggerExpansionOnTab": true,

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

//失去焦点后自动保存

//"files.autoSave": "onFocusChange",

// #值设置为true时，每次保存的时候自动格式化；

"editor.formatOnSave": true,

//每120行就显示一条线

"editor.rulers": [],

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

"javascript": "node",

},

"sync.gist": "a8048af23a5ee669d5db4d04974cb3d3",

"sync.forceUpload": true,

"editor.codeActionsOnSave": {

"source.fixAll.eslint": true

},

"explorer.confirmDragAndDrop": false,

"editor.renderControlCharacters": true,

"editor.minimap.enabled": false,

"breadcrumbs.enabled": true,

"editor.renderWhitespace": "none",

"vscodePluginDemo.showTip": false,

"cSpell.language": "en",

"cSpell.enableFiletypes": [

"vue"

],

"terminal.external.osxExec": "iTerm.app",

"editor.wordSeparators": "`~!@#$%^&*()=+[{]}\\|;:'\",.<>/?",

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

"editor.fontSize": 13,

"editor.fontFamily": "Consolas",

"workbench.preferredHighContrastColorTheme": "Default Dark+",

"workbench.preferredLightColorTheme": "Visual Studio Dark",

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

"editor.multiCursorModifier": "alt",

"editor.codeLens": true,

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

"tabnine.experimentalAutoImports": true,

"leek-fund.flash-news": false,

"leek-fund.hideStatusBarStock": true,

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

"diffEditor.ignoreTrimWhitespace": false,

"gitlab-mr.accessToken": "a0c09b49e37731543fa44f11db5e606a",

"gitlab-mr.accessTokens": {

"http://gitlab.internal.supermonkey.com.cn/": "a0c09b49e37731543fa44f11db5e606a"

},

"gitlab-mr.autoOpenMr": true,

"gitlab-mr.openToEdit": true,

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

"leek-fund.fundGroups": [

"我的基金"

],

"prettier.endOfLine": "auto",

"projectManager.sortList": "Recent",

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

"workbench.colorTheme": "Visual Studio Light"

}
```

