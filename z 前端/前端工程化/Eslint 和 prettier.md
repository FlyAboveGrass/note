# Prettier



## 基本概念和使用

[Prettier看这一篇就行了 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/81764012)



Prettier 是一个 Opinionated 的代码格式化工具。

什么是 Opinionated（固执己见的）？就是说：你必须认同我的观点，按照我说的做。Prettier 提供的可配置项非常少，除了必要的设置项，不会再给你们更多。给你设置项越多，你们越乱，你们就会继续争吵！



Prettier 的格式美化的重点是 line length，根据设置的行字符长度进行代码的换行等。



### 配置



[配置项清单 · 官网英文](https://prettier.io/docs/en/options.html)

[Prettier的配置文件 · 中文](https://jsweibo.github.io/2019/10/17/Prettier的配置文件/)



[参考配置：](https://juejin.cn/post/6938687606687432740)

```
module.exports = {
  printWidth: 100, // 单行长度
  tabWidth: 2, // 缩进长度
  useTabs: false, // 使用空格代替tab缩进
  semi: true, // 句末使用分号
  singleQuote: true, // 使用单引号
  quoteProps: 'as-needed', // 仅在必需时为对象的key添加引号
  jsxSingleQuote: false, // jsx中也不使用单引号
  bracketSpacing: true,   // 在对象属性添加空格.true { foo: bar }; false {foo: bar}
  trailingComma: 'none', // 多行时不打印尾随逗号
  bracketSpacing: true, // 在对象前后添加空格-eg: { foo: bar }
  jsxBracketSameLine: true, // 多属性html标签的‘/>’折行放置
  arrowParens: 'always', // 单参数箭头函数参数周围使用圆括号-eg: (x) => x
  requirePragma: false, // 无需顶部注释即可格式化
  insertPragma: false, // 在已被prettier格式化的文件顶部加上标注
  proseWrap: 'preserve', // markdown 文本保持原样显示不换行
  htmlWhitespaceSensitivity: 'ignore', //对HTML全局空白区域不敏感
  endOfLine: 'lf' // 结束行形式为 \n
};
```



对现有项目影响：

- arrowParens。 现在项目中存在很多没有使用括号包裹参数的箭头函数。
    - 推荐带括号的原因是在 ts 中更好的进行类型定义。

​	







**忽略文件**

可以在 `.prettierignore` 中设置忽略的文件





**辅助配置工具**

命令行辅助生成生成你的 prettier 配置文件

[leggsimon/create-prettier-eslint: Generates eslint and prettier config files for you (github.com)](https://github.com/leggsimon/create-prettier-eslint)

```
npm init prettier-eslint
```



**配合 git 使用** 

```
"lint-staged": {
    "*.{ts,tsx}": [
      "pretty-quick --staged"
    ]
  },
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
```





### vscode 配置


```

"editor.formatOnSave": true
"editor.defaultFormatter": "esbenp.prettier-vscode"
```





# Eslint





**Prettier** 是为了解决代码的美观性问题。

**Eslint** 是为了解决代码的质量问题，对程序可能出现的隐性 bug 进行预警修复，在无法自动修复时，会给出红线提示，强迫开发人员为其寻求更好的解决方案。



## 配置项



### **Plugin 和 Extend**

**Plugin** 

插件必须安装对应的 npm 包才可以使用，在不同的场景下，有一些定制的 eslint 的检查需求，如果在 eslint 的默认配置下没有，那么就可以通过插件引入第三方的规则。

Plugin 引入了插件之后，只是赋予了eslint解析`jsx-boolean-value`规则的检查能力，真正开启这个规则的检查能力还是要通过rules配置。



**Extend** 

Extend 是集成了 eslint 规则的一个最佳实践，它的内容实际上就是一个个的 `.eslintrc.js` 文件。通过 Extend 可以把别人的 rules 集成到当前的 eslint 中。

例如上面的 Plugin，在 Plugin 中一般会对定制的 eslint  规则进行集成形成最佳实践 `all`以及`recommened`，在配置 eslint 时可以对里面的规则集进行直接引用。

还有一种是以 `eslint-config-xxxx` 命名的包，这种包仅是对规则的一种集成，不存在额外的自定义规则检查，就可以直接进行 extend 引用。



### [参考配置](https://github.com/vortesnail/react-ts-quick-starter/blob/master/.eslintrc.js)

```Scala
const OFF = 0;

const WARN = 1;

const ERROR = 2;



module.exports = {

  env: {

    browser: true,

    es2021: true,

    node: true

  },

  extends: [

    'airbnb',

    'airbnb/hooks', // 支持 hooks

    'plugin:react/recommended',

    'plugin:unicorn/recommended', // 提供了循环依赖检测，文件名大小写风格约束等非常实用的规则集合

    'plugin:promise/recommended',

    'plugin:@typescript-eslint/recommended', // ts 语法规则

    'plugin:prettier/recommended' // 调用 prettier 进行格式检查

  ],

  parser: '@typescript-eslint/parser',

  parserOptions: {

    ecmaFeatures: {

      impliedStrict: true, // 全局启用严格模式

      jsx: true

    },

    ecmaVersion: 12,

    sourceType: 'module'

  },

  plugins: ['react', 'unicorn', 'promise', '@typescript-eslint', 'prettier'],

  settings: {

    'import/resolver': {

      node: {

        extensions: ['.tsx', '.ts', '.js', '.json']

      },

      typescript: {}

    }

  },

  rules: {

    'import/extensions': [

      ERROR,

      'ignorePackages',

      {

        ts: 'never',

        tsx: 'never',

        js: 'never'

      }

    ],

    'import/no-extraneous-dependencies': [ERROR, { devDependencies: true }], // 禁止导入未声明的外部模块。除 devDependencies 外

    'import/prefer-default-export': OFF, // 当只有一个模块，使用 default 导出

    'import/no-unresolved': ERROR, // 必须是一个 js 模块

    'import/no-dynamic-require': OFF, // 允许导入模块时模块名时动态的，如 require(`../${name}`);



    'unicorn/better-regex': ERROR,

    'unicorn/prevent-abbreviations': OFF, // 使用完整的单词

    'unicorn/filename-case': [

      ERROR,

      {

        cases: {

          // 中划线

          kebabCase: true,

          // 小驼峰

          camelCase: true,

          // 下划线

          snakeCase: false,

          // 大驼峰

          pascalCase: true

        }

      }

    ],

    'unicorn/no-array-instanceof': WARN,

    'unicorn/no-for-loop': WARN, // 使用 for-of 而不是 for 循环

    'unicorn/prefer-add-event-listener': [

      ERROR,

      {

        excludedPackages: ['koa', 'sax']

      }

    ],

    'unicorn/prefer-query-selector': ERROR,

    'unicorn/no-null': OFF,

    'unicorn/no-array-reduce': OFF,



    '@typescript-eslint/no-useless-constructor': ERROR, // 禁用不必要的构造函数

    '@typescript-eslint/no-empty-function': WARN,

    '@typescript-eslint/no-var-requires': OFF, // 禁止使用 var

    '@typescript-eslint/explicit-function-return-type': OFF, // 需要函数和类方法的显式返回类型

    '@typescript-eslint/explicit-module-boundary-types': OFF, // 导出的函数和类的公共类方法上需要显式的返回和参数类型

    '@typescript-eslint/no-explicit-any': OFF, // 禁止使用 any 类型

    '@typescript-eslint/no-use-before-define': WARN, // 不能在定义前使用

    '@typescript-eslint/no-unused-vars': WARN, // 禁止未使用过的变量

    'no-unused-vars': OFF,



    'react/jsx-filename-extension': [ERROR, { extensions: ['.tsx', 'ts', '.jsx', 'js'] }],

    'react/jsx-indent-props': [ERROR, 2],

    'react/jsx-indent': [ERROR, 2],

    'react/jsx-one-expression-per-line': OFF,

    'react/destructuring-assignment': OFF,

    'react/state-in-constructor': OFF,

    'react/jsx-props-no-spreading': OFF, // 禁止展开 JSX props， 如 <Component1 {...props} />

    'react/prop-types': OFF, // props 必须有类型



    'jsx-a11y/click-events-have-key-events': OFF, // onClick至少伴随以下选项之一：onKeyUp、onKeyDown、onKeyPress

    'jsx-a11y/no-noninteractive-element-interactions': OFF,

    'jsx-a11y/no-static-element-interactions': OFF,



    'lines-between-class-members': [ERROR, 'always'],

    // indent: [ERROR, 2, { SwitchCase: 1 }],

    'linebreak-style': [ERROR, 'unix'],

    quotes: [ERROR, 'single'], // 单词使用单引号

    semi: [ERROR, 'always'], // 语句末尾使用 ‘;’

    'no-unused-expressions': WARN,

    'no-plusplus': OFF, // 禁止使用一元操作符 ++ 和 --

    'no-console': OFF,

    'class-methods-use-this': ERROR,

    'jsx-quotes': [ERROR, 'prefer-single'], // 强制所有不包含单引号的 JSX 属性值使用单引号

    'global-require': OFF,

    'no-use-before-define': OFF,

    'no-restricted-syntax': OFF,

    'no-continue': OFF,



    'react/function-component-definition': [

      // 允许使用普通函数和箭头函数定义组件

      ERROR,

      {

        namedComponents: ['function-declaration', 'function-expression', 'arrow-function'],

        unnamedComponents: ['function-expression', 'arrow-function']

      }

    ],

    'prettier/prettier': [ERROR, {}, { usePrettierrc: true }], // 读取本地的 prettier 以覆盖插件

    'unicorn/no-array-for-each': WARN

  }

};
```





















