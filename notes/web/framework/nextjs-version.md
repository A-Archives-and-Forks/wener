---
title: NextJS 版本历史
tags:
  - Version
---

# NextJS 版本历史

:::tip

- 大版本升级时建议出 .1 时再升级
- 目前还不适合用 yarn2/yarn berry

:::

:::note 值得关注的问题

- [vercel/next.js#16368](https://github.com/vercel/next.js/issues/16368)
  Module Federation Support
- swc babel 插件反馈 [#30174](https://github.com/vercel/next.js/discussions/30174)

:::

| version    | date       | blog      |
| ---------- | ---------- | --------- |
| NextJS 12  | 2021-11-27 | [next-12] |
| NextJS 11  | 2021-06-16 | [next-11] |
| NextJS 10  | 2020-10-27 | [next-10] |
| NextJS 9.5 | 2020-7-28  |

[next-12]: https://nextjs.org/blog/next-12
[next-11]: https://nextjs.org/blog/next-11
[next-10]: https://nextjs.org/blog/next-10

## NextJS 12

### NextJS 12.2

- Stable
  - Middleware
  - On-Demand ISR
    - `res.revalidate('/path-to-revalidate')`
- Experimental

  - Edge API Routes

    ```ts title="api/hello.ts"
    import type {NextRequest} from 'next/server';
    export default (req: NextRequest) => {
      return new Response(`Hello, from ${req.url} I'm now an Edge API Route!`);
    };

    export const config = {
      // 默认 nodejs
      runtime: 'experimental-edge',
    };
    ```

  - Edge Server-Rendering
    - streaming server-rendering
    - 当前 process.env.NEXT_RUNTIME
    ```ts title="next.config.js"
    module.exports = {
      experimental: {
        // 默认 nodejs
        runtime: 'experimental-edge',
      },
    };
    ```
  - SWC Plugins - WASM
  - next/future/image
    - 直接渲染 img 无 div 和 span
    - 无 layout, objectFit, objectPosition
      - 暂不支持 fill 逻辑 - 必须指定宽高
    - 使用浏览器本身的 lazy load - Chrome 77+, Safari 15.4+
      - 不再需要 IntersectionObserver
      - [HTMLImageElement.loading](https://developer.mozilla.org/en-US/docs/Web/API/HTMLImageElement/loading)
    - 无 loader 配置，使用 loader props

- `outputStandalone: true` -> `output: 'standalone'`

```js title="next.config.js"
module.exports = {
  compiler: {
    // true || {}
    // 支持 @emotion/babel-plugin 除 importMap 之外的功能
    emotion: {
      sourceMap: processs.env.NODE_EV !== 'production',
      autoLabel: 'dev-only', // dev-only,never,always
      // 启用 autoLabel 时
      labelFormat: 'local', // local,filename,dirname,
    },
  },
  experimental: {
    // 实验支持 browsersList
    legacyBrowsers: false,
    browsersListForSwc: true,

    // 之前
    outputStandalone: true,
  },
  // 现在
  output: 'standalone',
};
```

### NextJS 12.0

- swc 替代 babel - 更快的刷新和构建
  - styled-component 支持还需要完善
- 中间件 - \_middleware.ts
- 支持 React 18
- Server-Side Streaming - 实验阶段
  - 支持 react 18 server-side Suspense
- React Server Components - 实验阶段
  - React 服务端组件
- 支持 ESM
- URL Imports - 实验阶段
  - 会生成 next.lock 跟踪远程资源
- Bot-Aware ISR Fallback
  - 检测到爬虫使用 `fallback: 'blocking'`
- next/image - 支持 AVIF
- @vercel/nft 替代 target - Output File Tracing
- 支持 next.config.mjs
- 移除 webpack 4
- [next-12](https://nextjs.org/blog/next-12)

```js title="next.config.js"
module.exports = {
  // opt-in swc minify 7x faster than Terser.
  swcMinify: true,
  images: {
    // opt-in avif
    formats: ['image/avif', 'image/webp'],
  },
  experimental: {
    // SSS
    concurrentFeatures: true,
    serverComponents: true,
    // opt-in esm url import
    urlImports: ['https://cdn.skypack.dev'],
  },
};
```

```ts title="pages/_middleware.ts"
import type {NextFetchEvent, NextRequest} from 'next/server';

export function middleware(req: NextRequest, ev: NextFetchEvent) {
  // https://developer.mozilla.org/en-US/docs/Web/API/Response
  return new Response('Hello, world!');
}
```

## NextJS 11

- Webpack 5

:::caution

- styled-components 可能会有问题
  - [vercel/next.js#26799](https://github.com/vercel/next.js/issues/26799)

:::

> swc 的开发者加入了 vercel，集成 swc 开发体验会有所提高。

- next lint
- next/script
- next/image
  - placeholder
  - 自动大小
- Webpack 5
- npx @next/codemod cra-to-next
  - 迁移 CRA 到 NextJS
- 预览发布 Next.js Live

### NextJS 11.1

- ESM 实验阶段 - `{experimental: { esmExternals: true }}`
- 使用 SWC 替代 Babel 和 Terser
- 渲染使用 keep-alive 增加数据请求速度
- 内置 ESLint - `next lint`

## NextJS 10

- `next/image` - 图片组件，自动图片优化
  - WebP、JPEG
- 国际化路由
- [Analytics](http://nextjs.org/analytics) - 性能分析
- [Commerce](https://nextjs.org/commerce) - 电商模版
- React 17
- `getStaticProps` / `getServerSideProps` Fast Refresh 支持
- `@next/mdx` Fast Refresh 支持
- 从 NPM 导入组件 CSS
- 自动处理 `href` 属性，可以不用 `as`
- @next/codemod - nextjs 升级代码转换
- 允许阻塞 getStaticPaths - 之前是 fallback

> Webpack 开发者加入 Vercel

```js
// 国际化路由
module.exports = {
  i18n: {
    locales: ['en', 'nl'],
    defaultLocale: 'en',
    domains: [
      {
        domain: 'example.com',
        defaultLocale: 'en',
      },
      {
        domain: 'example.nl',
        defaultLocale: 'nl',
      },
    ],
  },
};
```

```js
import DatePicker from 'react-datepicker';
// 支持导入 css - 之前需要在 _app.js 导入
import 'react-datepicker/dist/react-datepicker.css';
```

```js
export function getStaticPaths() {
  return {
    // enables blocking mode for the fallback behavior
    fallback: 'blocking',
  };
}
```

```js
export function getStaticProps() {
  return {
    // returns the default 404 page with a status code of 404
    notFound: true,

    // returns a redirect to an internal page `/another-page`
    redirect: {
      destination: '/another-page',
      // destination: 'https://example.com',
      permanent: false,
    },
  };
}
```

## NextJS 9.5

- [9.5](https://nextjs.org/blog/next-9-5) - 2020-7-28
- 增量静态生成 - 稳定阶段
- 自定义 base path
  - 方便与现有站点共存
  - baseUrl
  - contextPath
- 支持重写、重定向、Header 自定义
  - 例如 静态页面添加 header，重定向到旧站点
- 支持 url 尾部 `/`
- 页面级持久缓存
  - 未变化的页面不会重新构建
  - 构建路径包含 hash
  - 以前 hash 是 build 级别
  - 之前 `/_next/static/ovgxWYrvKyjnlM15qtz7h/pages/about.js`
  - 现在 `/_next/static/chunks/pages/about.qzfS4o5gIEXRME6sTEahL.js`
- 增强快速刷新
- 支持 React Profiling - [How it works](https://nextjs.org/docs/basic-features/fast-refresh)
  - `next build --profile`
- 页面支持捕获所有的路由
  - SEO-driven use-cases
  - `pages/blog/[[...slug]].js`
  - 之前是 api 支持
- Webpack 5 - beta
  - PnP
  - yarn `resolutions` 设置 `^5.0.0-beta.22`
- 增强 macOS 的文件监听

```js
export async function getStaticProps() {
  return {
    props: await getDataFromCMS(),
    // 增量静态生成 - 最多 1 秒 1 次
    // stale-while-revalidate https://tools.ietf.org/html/rfc5861
    revalidate: 1,
  };
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
      {source: '/backend/:path*', destination: 'https://example.com/:path*'},
      // 如果路径不存在则尝试使用后端 - 可以实现逐步替换为 NextJS
      {
        source: '/:path*',
        destination: '/:path*',
      },
      {
        source: '/:path*',
        destination: `https://example.com/:path*`,
      },
    ];
  },

  // 重定向
  async redirects() {
    return [
      {
        source: '/about',
        destination: '/',
        permanent: true,
      },
    ];
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
            value: "microphone 'none'; geolocation 'none'",
          },
        ],
      },
    ];
  },
};
```

## NextJS 9.4

- [9.4](https://nextjs.org/blog/next-9-4) - 2020-5-11
- 快速刷新
  - 基于 [React Refresh](https://github.com/facebook/react/tree/master/packages/react-refresh)
  - 尽量只刷新组件不丢失状态，异常友好显示
  - 之前是可能整个页面刷新，异常不太友好
- 增量静态从新生成 SSG - Beta
- 内建支持 `.env` 和 `NEXT_PUBLIC_` 前缀环境变量
  - 💚 之前都是需要在配置中添加 dotenv
- 内建 fetch 的 polyfill
- 集成 [Vitals](https://web.dev/vitals/) 报告
  - 修改 `_app.js` 进行记录

```js
// Will be called once for every metric that has to be reported.
export function reportWebVitals(metric) {
  // These metrics can be sent to any analytics service
  console.log(metric);
}

function MyApp({Component, pageProps}) {
  return <Component {...pageProps} />;
}

export default MyApp;
```

- 支持绝对路径导入和别名
  - 💚 之前需要配置 `tsconfig-paths-webpack-plugin`
  - 识别 jsconfig.json 和 tsconfig.json 中的配置
- 内建 Sass 支持配置
- 优化日志输出
- 纯静态实验支持 - `unstable_runtimeJS`
  - 不会包含 React 和 NextJS 的脚本

```js
export const config = {
  unstable_runtimeJS: false,
};
export default () => <h1>My page</h1>;
```

## NextJS 9.3

- [9.3](https://nextjs.org/blog/next-9-3) - 2020-3-10
- SSG 服务端静态生成 HTML
  - `getStaticProps` - 构建时获取数据 - 静态生成
    - 支持预览模式 - cookie
  - `getStaticPaths` - 获取动态路由
    - 支持 fallback - 构建时不存在是否运行时请求
  - `getServerSideProps` - **每次** 请求时获取数据 - 服务端渲染
    - `getInitialProps` 只会在服务端请求一次
- 预览模式 - 条件性跳过 SSG
- 内建 Sass 全局样式
- 内建 Sass CSS 模块支持
- 404 静态优化
- 运行时减少 32 kB

## NextJS 9.2

- [9.2](https://nextjs.org/blog/next-9-2)
- 内建 CSS 全局样式支持 - 不再需要 `next-css` 依赖
- 内建 CSS 模块支持 `.module.css`
- 增强代码切分策略
- 新增 `[...slug].js` 全部捕获的动态路由

## NextJS 9.1

- [9.1](https://nextjs.org/blog/next-9-1)
- 支持 `src/pages` 作为页面目录
- 支持 `public` 作为静态目录而不是 `static`
