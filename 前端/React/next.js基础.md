# 介绍



### Next.js 是什么

Next.js 是一个轻量级的 React 服务端渲染应用框架。



### Next 优点

1. 零配置

2. 混合模式 SSG 和 SSR

3. 增量静态生成

4. 支持 TS

5. 快速刷新

6. 基于文件系统的路由

7. API 路由

8. 内置支持 CSS

9. 代码拆分和打包

10. 完全可拓展

    



### Next.js 存在的问题

1. 文件系统路由
    - 定制路由只是一个别名，还需要服务端转换
    - 只支持query 传参， params 传参不方便
2. component 支持不好
3. 路由传参刷新页面
    - 只支持query 传参， params 传参不方便
4. 自由度低
    - 带样式或图片的 React 组件无法直接使用
    - 更适合博客，资讯类交互比较少的项目













# 基本特性



## pages 页面



page 是一个导出的 react 组件



pages 文件名即页面路径



### 预渲染

默认情况下，Next.js 将 **预渲染** 每个 page（页面）。这意味着 Next.js 会预先为每个页面生成 HTML 文件，而不是由客户端 JavaScript 来完成。



预渲染两种形式：

1. 静态生成（推荐）。构建时生成，请求时重用
2. 服务器端渲染。 每次请求时重新生成 html





#### 静态生成依赖数据



1. 您的页面 **内容** 取决于外部数据：使用 `getStaticProps`。
2. 你的页面 **paths（路径）** 取决于外部数据：使用 `getStaticPaths` （通常还要同时使用 `getStaticProps`）。



##### 内容依赖外部数据

```
function Blog({ posts }) {
  // Render posts...
}

// 此函数在构建时被调用
export async function getStaticProps() {
  // 调用外部 API 获取博文列表
  const res = await fetch('https://.../posts')
  const posts = await res.json()

  // 通过返回 { props: { posts } } 对象，Blog 组件
  // 在构建时将接收到 `posts` 参数
  return {
    props: {
      posts,
    },
  }
}

export default Blog
```





##### 页面路径依赖外部数据









### 服务端渲染

 page（页面）使用服务器端渲染，你需要 `export` 一个名为 `getServerSideProps` 的 `async` 函数。服务器将在每次页面请求时调用此函数。





## 获取数据



### getStaticProps

```
export async function getStaticProps(context) {
  const res = await fetch(`https://.../data`)
  const data = await res.json()

  if (!data) {
    return {
      notFound: true,
    }
  }

  return {
    props: {}, // will be passed to the page component as props
  }
}
```



**入参**

context 包含以下内容

- params
- getStaticPath
- preview： 是否是预览模式
- previewData
- locale： 
- locales
- defaultLocale



**返回值**

- props： 必须。组件需要的 data
- revalidate： 可选数值。多少秒后页面可重新生成。
- notFound： 可选布尔。为 true 时可以跳转 404
- redirect： 可选。重定向资源。





### 增量静态重新生成



增量静态重新生成允许你更新已存在的页面，通过数据更新时在后台重新渲染他们。





### 读取文件 process.cwd()

```
export async function getStaticProps() {
  const postsDirectory = path.join(process.cwd(), 'posts')
  const filenames = await fs.readdir(postsDirectory)

  const posts = filenames.map(async (filename) => {
    const filePath = path.join(postsDirectory, filename)
    const fileContents = await fs.readFile(filePath, 'utf8')

    // Generally you would parse/transform the contents
    // For example you can transform markdown to HTML here

    return {
      filename,
      content: fileContents,
    }
  })
  // By returning { props: posts }, the Blog component
  // will receive `posts` as a prop at build time
  return {
    props: {
      posts: await Promise.all(posts),
    },
  }
}
```





### getStaticPaths



使用动态路由的时候， next.js 会预渲染所有 getStaticPaths 定义好的路由。

```jsx
export async function getStaticPaths() {
  return {
    paths: [
      { params: { ... } } // See the "paths" section below
    ],
    fallback: true or false // See the "fallback" section below
  };
}
```



#### **返回值**

**paths**

1. `pages/posts/[id].js` : 

    ```js
    { params: { id: '1' } }
    ```

2. `pages/posts/[postId]/[commentId]`:

    ```js
    { params: {  } }
    ```

3. `pages/[...slug]`: 

    ```js
    // 路径为 /foo/bar
    { 
      params: { 
        slug: ['foo', 'bar'] 
      } 
    }
    
    // 路径为 /
    slug: false/undefined/null/[]
    
    
    ```





**fallback**

一个布尔值



1. false

    结果会是404 页面。当你页面很少的时候可以用，这样页面在构建的时候就会全部静态生成。

2. true

    当你有非常大量的页面并且依赖数据，预渲染会花费很长时间。使用 `fallback： true` 可以让你只生成页面的一部分，剩下的根据数据加载。

3. blocking

    `getStaticPaths` 没有返回的新路径会等待  html 生成，和 SSR 一样，然后被缓存起来，等待后续的一次请求。



#### 注意点



当在动态路由页面里面使用 `getStaticProps` 处理参数的时候，必须使用 `getStaticPaths`



`getStaticPaths` 仅在服务端构建的时候执行， 并且在每一次请求的时候都会被调用一次。



`getStaticPaths` 只能在页面文件中导出， 不能在非页面文件中导出。





### getServerSideProps



在页面中使用 `getServerSideProps`  时，next.js 会在每一次请求的时候都对 `getServerSideProps` 返回的数据进行预渲染。



当你的预渲染页面数据必须在请求的时候就要返回，这时候可以使用 `getServerSideProps`



```
export async function getServerSideProps(context) {
  return {
    props: {}, // will be passed to the page component as props
  }
}
```



#### 参数

**context**

包含以下的 key 值

1. req
2. res
3. query
4. preview
5. previewData
6. resovedUrl
7. locale
8. locales
9. defaultLocale



#### 返回值



1. props

2. notFound

3. redirect

    

#### 注意点



`getServerSideProps` 只能在页面文件中导出， 不能在非页面文件中导出。



`getServerSideProps` 只能在服务端运行，不能在客户端运行。







## 内置 CSS 支持

 

### 全局样式





### 组件级的 CSS



Next.js 通过 `[name].module.css` 文件命名约定来支持 [CSS 模块](https://github.com/css-modules/css-modules) 。



CSS 模块通过自动创建唯一的类名从而将 CSS 限定在局部范围内。





### 对 sass 的支持

你可以通过 CSS 模块以及 `.module.scss` 或 `.module.sass` 扩展名来使用组件及的 Sass



配置 Sass 编译器，可以使用 `next.config.js` 文件中的 `sassOptions` 参数进行配置

```
module.exports = {
  sassOptions: {
    includePaths: [path.join(__dirname, 'styles')],
  },
}
```



### CSS-In-JS





#### Styled-jsx

用于支持作用域隔离

> [不支持服务器端渲染且仅支持 JS](https://github.com/w3c/webcomponents/issues/71)



```
    <div>
      Hello world
      <p>scoped!</p>
      <style jsx>{`
        p {
          color: blue;
        }
        div {
          background: red;
        }
        @media (max-width: 600px) {
          div {
            background: blue;
          }
        }
      `}</style>
      <style global jsx>{`
        body {
          background: black;
        }
      `}</style>
    </div>
```





## 图片优化



next.js 无需在构建时优化图片，而是按用户请求优化图片。所有图片数量并不影响构建时间。



图片默认延迟加载，这样可以避免 [累计布局偏移（Cumulative Layout Shift）](https://web.dev/cls/)



### 图片组件





### 配置



#### 域名

对外部图片进行优化，要先把 src 设置成完整的网址，然后指定什么 `域名` 需要开启优化，确保不会误伤其他外部网址。



当loader 被设置成外部图片服务，此选项被忽略。



#### Loader 



如果想使用第三方图片优化而不使用 next 提供的优化时，可以通过配置 loader 和路径前缀参数， 为图片设置相对路径并自动为你选择的云服务生成正确的完整网址。



```
module.exports = {
  images: {
    loader: 'imgix',
    path: 'https://example.com/myaccount/',
  },
}
```



#### 缓存



图片根据用户的请求进行动态优化，并存储到 `<distDir>/cache/images` 目录下。优化过的图片文件将用于后续的请求，直到过期为止。



### 高级设置



#### 设备尺寸



#### 图片尺寸









## 静态文件服务



静态文件存放到根目录下的 `public` 文件夹下，提供对外访问。



> 不要对 `public` 改名，只有这个目录可以存放静态资源并对外提供访问
>
> 确保静态文件中没有与 pages 目录下的文件重名， 否则报错
>
> public 下面的静态资源只能在构建时找到，运行时添加的文件不可以用。建议使用第三方文件服务





## 快速刷新



#### 原理



如果一个文件只导出了 react 组件，那么快速刷新只会刷新着一个文件，你的组件会重新渲染



如果一个文件导出的不是一个react 组件，快速刷新会重新运行在这个文件和其他引入了这个文件的文件。



如果一个文件被react 树之外的文件引入，那么快速刷新会全部重新加载。





#### 局限性



快速刷新会尽量保存 react 状态，但有些情况还是会重置所有状态



1. 本地状态不会为 class 组件保存 （只有 函数组件和 hooks 保存状态）
2. 除了react component之外你还导出了其他的内容
3. 有时文件会导出一个调用高阶组件的返回结果，如果返回组件是一个 class 的话，状态会被重置
4. 匿名函数会导致快速刷新不保存本地组件状态。









#### 快速刷新和 hooks









## TypeScript













## 环境变量









# 路由



### 简介

index 路由



嵌套路由



#### 动态路由部分







#### 页面链接



Link组件



Link 导航到动态路由

```
<Link href={`/blog/${encodeURIComponent(post.slug)}`}>
	<a>{post.title}</a>
</Link>
          
<Link
  href={{
  pathname: '/blog/[slug]',
  query: { slug: post.slug },
  }}
>
	<a>{post.title}</a>
</Link>
```





### 动态路由





#### 匹配所有路由

动态路由可以通过在路由中添加  `...` 到括号中延伸匹配所有路由



`pages/post/[...slug].js` 可以匹配到

- /post/a
- /post/a/b/c
- ......





#### 可选的匹配所有路由

通过在路由参数中添加双中括号可以使得动态路由参数参数可选 (`[[...slug]]`)



与上面的全部匹配不同，(`[[...slug]]`) 当没有参数的时候，会匹配到 /post





#### 注意



预定义路由优先于动态匹配路由



### 浅路由



浅路由让你可以改变当前页面的路由并且不必重新发送数据请求



```
import { useEffect } from 'react'
import { useRouter } from 'next/router'

function Page() {
  const router = useRouter()

  useEffect(() => {
    // Always do navigations after the first render
    router.push('/?counter=10', undefined, { shallow: true })
  }, [])

  useEffect(() => {
    // The counter changed!
  }, [router.query.counter])
}

export default Page
```



**注意**

浅路由只能应用于同页面的 url 改变





# API路由



`pages/api` 目录下的任何文件都将作为 API 端点映射到 `/api/*`，而不是 `page`。这些文件只会增加服务端文件包的体积，而不会增加客户端文件包的大小。



为了使 API 路由能正常工作，你需要导出（export）一个默认函数（即 **请求处理器**），并且该函数能够接收以下参数：

- `req`: 一个 [http.IncomingMessage](https://nodejs.org/api/http.html#http_class_http_incomingmessage) 实例，以及一些预建的中间件，可以在 [此处](https://www.nextjs.cn/docs/api-routes/api-middlewares) 查看
- `res`: 一个 [http.ServerResponse](https://nodejs.org/api/http.html#http_class_http_serverresponse) 实例，以及一些辅助函数，可以在 [此处]](/docs/api-routes/response-helpers.md) 查看





#### API中间件







#### 针对 Connect / Express 中间件的支持









### response 助手函数



- `res.status(code)` - 这是一个设置状态码的函数。`code` 必须是有效的 [HTTP 状态码](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes)
- `res.json(json)` - 发送 JSON 格式的响应。`json` 必须是有效的 JSON 对象
- `res.send(body)` - 发送 HTTP 响应。`body` 的类型可以是 `string`、 `object` 或 `Buffer`
- res.redirect 



























































