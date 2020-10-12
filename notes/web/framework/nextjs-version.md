---
id: nextjs-version
title: NextJS 版本历史
---

# NextJS 版本历史

## 9.5
* [9.5](https://nextjs.org/blog/next-9-5) - 2020-7-28
* 增量静态生成 - 稳定阶段
* 自定义 base path
  * 方便与现有站点共存
  * baseUrl
  * contextPath
* 支持重写、重定向、Header 自定义
  * 例如 静态页面添加 header，重定向到旧站点
* 支持 url 尾部 `/`
* 页面级持久缓存
  * 未变化的页面不会重新构建
  * 构建路径包含 hash
  * 以前 hash 是 build 级别
  * 之前 `/_next/static/ovgxWYrvKyjnlM15qtz7h/pages/about.js`
  * 现在 `/_next/static/chunks/pages/about.qzfS4o5gIEXRME6sTEahL.js`
* 增强快速刷新
* 支持 React Profiling - [How it works](https://nextjs.org/docs/basic-features/fast-refresh)
  * `next build --profile`
* 页面支持捕获所有的路由
  * SEO-driven use-cases
  * `pages/blog/[[...slug]].js`
  * 之前是 api 支持
* Webpack 5 - beta
  * PnP
  * yarn `resolutions` 设置 `^5.0.0-beta.22`
* 增强 macOS 的文件监听

```js
export async function getStaticProps() {
  return {
    props: await getDataFromCMS(),
    // 增量静态生成 - 最多 1 秒 1 次
    // stale-while-revalidate https://tools.ietf.org/html/rfc5861
    revalidate: 1
  }
}
```

```js
// next.config.js
module.exports = {
  // 所有资源也都会使用该路径
  // <Link> 也会自动添加前缀
  basePath: '/docs',

  // url 是否添加最后的 /
  trailingSlash: true,

  // 重写 - 类似于 nginx 的 proxy_pass
  async rewrites() {
    return [
      { source: '/backend/:path*', destination: 'https://example.com/:path*' },
      // 如果路径不存在则尝试使用后端 - 可以实现逐步替换为 NextJS
      {
        source: '/:path*',
        destination: '/:path*'
      },
      {
        source: '/:path*',
        destination: `https://example.com/:path*`
      }
    ]
  },

  // 重定向
  async redirects() {
    return [
      {
        source: '/about',
        destination: '/',
        permanent: true
      }
    ]
  },

  // 自定义 header
  async headers() {
    return [
      {
        source: '/:path*',
        headers: [
          {
            key: 'Feature-Policy',
            // Disable microphone and geolocation
            value: "microphone 'none'; geolocation 'none'"
          }
        ]
      }
    ]
  },
}
```

## 9.4
* [9.4](https://nextjs.org/blog/next-9-4) - 2020-5-11
* 快速刷新
  * 基于 [React Refresh](https://github.com/facebook/react/tree/master/packages/react-refresh)
  * 尽量只刷新组件不丢失状态，异常友好显示
  * 之前是可能整个页面刷新，异常不太友好
* 增量静态从新生成 SSG - Beta
* 内建支持 `.env` 和 `NEXT_PUBLIC_` 前缀环境变量
  * 💚 之前都是需要在配置中添加 dotenv
* 内建 fetch 的 polyfill
* 集成 [Vitals](https://web.dev/vitals/) 报告
  * 修改 `_app.js` 进行记录
```js
// Will be called once for every metric that has to be reported.
export function reportWebVitals(metric) {
  // These metrics can be sent to any analytics service
  console.log(metric)
}

function MyApp({ Component, pageProps }) {
  return <Component {...pageProps} />
}

export default MyApp
```
* 支持绝对路径导入和别名
  * 💚 之前需要配置 `tsconfig-paths-webpack-plugin`
  * 识别 jsconfig.json 和 tsconfig.json 中的配置
* 内建 Sass 支持配置
* 优化日志输出
* 纯静态实验支持 - `unstable_runtimeJS`
  * 不会包含 React 和 NextJS 的脚本
```js
export const config = {
  unstable_runtimeJS: false
}
export default () => <h1>My page</h1>
```

## 9.3
* [9.3](https://nextjs.org/blog/next-9-3) - 2020-3-10
* SSG 服务端静态生成 HTML
  * `getStaticProps` -  构建时获取数据 - 静态生成
    * 支持预览模式 - cookie
  * `getStaticPaths` - 获取动态路由
    * 支持 fallback - 构建时不存在是否运行时请求
  * `getServerSideProps` - __每次__ 请求时获取数据 - 服务端渲染
    * `getInitialProps` 只会在服务端请求一次
* 预览模式 - 条件性跳过 SSG
* 内建 Sass 全局样式
* 内建 Sass CSS 模块支持
* 404 静态优化
* 运行时减少 32 kB

## 9.2
* [9.2](https://nextjs.org/blog/next-9-2)
* 内建 CSS 全局样式支持 - 不再需要 `next-css` 依赖
* 内建 CSS 模块支持 `.module.css`
* 增强代码切分策略
* 新增 `[...slug].js` 全部捕获的动态路由

## 9.1
* [9.1](https://nextjs.org/blog/next-9-1)
* 支持 `src/pages` 作为页面目录
* 支持 `public` 作为静态目录而不是 `static`
