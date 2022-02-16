

pnpm 全称是 “performant npm”，即高性能的 npm。





# 为什么需要 pnpm



##  npm 的困境



### 扁平化依赖

当我们在项目中引入一个依赖的时候，安装依赖后经常会发现 node_modules 中多了不止一个依赖。这就是npm 扁平化依赖的结果。



假设我们引入了一个依赖 A，而这个A又依赖于B、C

在 npm1 和 npm2 中会呈现一个嵌套的结构，即

```
node_modules
|_A
	|_index.js
	|_node_modules
		|_B
		|_C
```

如果B、C还依赖于其他的依赖，那么会导致

- 依赖层级过深
- 重复的包安装
- 模块实例不能共享





在 npm3 之后，就开始使用扁平化依赖来解决上面的问题，新的结构会呈现：

```
node_modules
|_A
|_B
|_C
```

这样子的方式看似完美解决了npm1和npm2的翁题，却还是诞生了新的问题：

- 依赖结构的不稳定。(两个项目引入了两个不同版本的依赖，由于名称相同不可以直接平铺在 node_modules , 所以必须有一个以树状结构挂在其中一个项目里，但是具体是哪个项目无法确定)
- 扁平化算法比较复杂，耗时很长
- 幽灵依赖
    - 例如项目中引入了依赖A，A引入了依赖B。在扁平化的依赖下，项目中可以不经声明直接使用依赖B，这会导致更多不确定性。如果A以后升级不使用B了，那么我们的项目就会报错。







## pnpm的优势



1. 下载速度更快
2. 更安全
3. 高效利用磁盘空间
4. 支持 monorepo。（monorepo 的宗旨就是用一个 git 仓库来管理多个子项目，所有的子项目都存放在根目录的`packages`目录下）









# pnpm 如何组织依赖



## 软链接和硬连接结合



### [软链接和硬链接](https://zhuanlan.zhihu.com/p/442133074)

在计算机中我们文件夹中的文件实际上是一个指针，但这个指针并不是直接指向我们在磁盘中存储文件的位置，而是指向一个 inode 块，inode 中存储着文件在磁盘中的各种信息，系统根据 inode 中的文件信息去找到磁盘中的文件。一般我们的文件都是指向 对应文件的 inode，我们把这类链接成为硬链接，但是还有一种链接，它存储的并不是实际的值，而是另一个硬链接的地址，我们把这类链接成为软链接。



### 

#### 特性

**硬链接**

- 具有相同inode节点号的多个文件互为硬链接文件；
- 删除硬链接文件或者删除源文件任意之一，文件实体并未被删除；
- 只有删除了源文件和所有对应的硬链接文件，文件实体才会被删除；
- 硬链接文件是文件的另一个入口；
- 可以通过给文件设置硬链接文件来防止重要文件被误删；
- 创建硬链接命令 ln 源文件 硬链接文件；
- 硬链接文件是普通文件，可以用rm删除；
- 对于静态文件（没有进程正在调用），当硬链接数为0时文件就被删除。注意：如果有进程正在调用，则无法删除或者即使文件名被删除但空间不会释放。

**软链接**

- 软链接类似windows系统的快捷方式；
- 软链接里面存放的是源文件的路径，指向源文件；
- 删除源文件，软链接依然存在，但无法访问源文件内容；
- 软链接失效时一般是白字红底闪烁；
- 创建软链接命令 ln -s 源文件 软链接文件；
- 软链接和源文件是不同的文件，文件类型也不同，inode号也不同；
- 软链接的文件类型是“l”，可以用rm删除。





### pnpm 的 node_modules



以安装koa为例，当我们使用 pnpm 安装koa 时，实际上会生成两个文件夹，两个文件夹的形式分别是：

- node_modules/.pnpm/<package-name>@version/node_modules/<package-name>
- node_modules/<package-name>

```
node_modules
|_.pnpm
	|_koa@2.7.0/node_modules
		|_koa
		|_ ... koa的其他依赖
|_koa
```



​	当我们在项目中使用koa 这个依赖时，会去找 node_modules/koa 文件夹。但是这个文件夹实际上是一个**软链接**，这个软连接会指向 node_modules/.pnpm/koa@2.7.0/node_modules 下面的内容，这下面的文件夹才是一个**硬链接**，这个硬链接会指向 pnpm 在全局创建的存储空间。

​	pnpm 下载的依赖都会放在全局的存储空间，通过硬链接的方式共享给所有使用该依赖的项目，这样可以使得下次再下载依赖的时候不用再去网络上下载。（npm 则会每个项目都下载一次）







# 迁移指南



pnpm 为我们提供了相当方便的方式从 npm 迁移过来， 步骤如下：

1. [安装 pnpm](https://www.pnpm.cn/installation)

    ```
    // 常规安装
    curl -f https://get.pnpm.io/v6.16.js | node - add --global pnpm // linux/mac
    
    Invoke-WebRequest 'https://get.pnpm.io/v6.16.js' -UseBasicParsing -o pnpm.js; node pnpm.js add --global pnpm; Remove-Item pnpm.js // window
    
    // npm 安装
    npm install -g pnpm
    
    //Homebrew 安装
    brew install pnpm
    ```

    

2. 将旧的 package-lock.json 转换为 pnpm-lock.yaml

    ```
    // 支持 yarn.lock/package-lock.json/npm-shrinkwrap.json
    pnpm import
    ```

    

3. 删除原来的 node_modules

4. 使用 pnpm 重新安装依赖

    ```
    pnpm install
    ```

    





> 参考链接：
>
> [浅谈 pnpm 软链接和硬链接 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/442133074)





