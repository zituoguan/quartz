---
title: 架构
---

Quartz 是一个静态网站生成器。它是如何工作的？

这个问题最好的解答方式，是追踪当用户（也就是你！）在命令行中运行 `npx quartz build` 时发生了什么：

## 在服务器端

1. 运行 `npx quartz build` 后，npm 会查看 `package.json`，找到 `quartz` 的 `bin` 条目，该条目指向 `./quartz/bootstrap-cli.mjs`。
2. 这个文件顶部有一个 [shebang](<https://en.wikipedia.org/wiki/Shebang_(Unix)>) 行，告诉 npm 使用 Node 执行它。
3. `bootstrap-cli.mjs` 负责以下几件事：
   1. 使用 [yargs](http://yargs.js.org/) 解析命令行参数。
   2. 使用 [esbuild](https://esbuild.github.io/) 将 Quartz 的其余部分（用 Typescript 编写）转译并打包为普通 JavaScript。这里的 `esbuild` 配置有些特殊，还处理了 `.scss` 文件的导入，使用 [esbuild-sass-plugin v2](https://www.npmjs.com/package/esbuild-sass-plugin)。此外，我们还会为组件声明的“内联”客户端脚本（任何 `.inline.ts` 文件）使用自定义 `esbuild` 插件，再次运行 esbuild，为浏览器而不是 node 打包。两种类型的模块都作为纯文本导入。
   3. 如果设置了 `--serve`，则运行本地预览服务器。这会启动两个服务器：
      1. 在 3001 端口启动 WebSocket 服务器，用于处理热重载信号。它跟踪所有入站连接，并在检测到服务器端更改（内容或配置）时发送“rebuild”消息。
      2. 在用户定义的端口（通常为 8080）启动 HTTP 文件服务器，用于实际提供网站文件。
   4. 如果检测到 `--serve` 标志，还会启动一个文件监听器，检测源代码更改（如 `.ts`、`.tsx`、`.scss` 或打包器文件）。发生更改时，我们会使用 esbuild 的 [rebuild API](https://esbuild.github.io/api/#rebuild) 重新构建模块（上面的第 2 步），大大减少构建时间。
   5. 在转译主 Quartz 构建模块（`quartz/build.ts`）后，我们将其写入缓存文件 `.quartz-cache/transpiled-build.mjs`，然后使用 `await import(cacheFile)` 动态导入。不过，我们需要巧妙地绕过 Node 的 [import 缓存](https://github.com/nodejs/modules/issues/307)，所以会添加一个随机查询字符串，伪装成新模块。但这会导致内存泄漏，所以我们只能希望用户在单次会话中不要热重载配置太多次 :))（每次重载大约泄漏 ~350kB 内存）。导入模块后，我们会调用它，传入之前解析的命令行参数以及一个用于通知客户端刷新的回调函数。
4. 在 `build.ts` 中，我们首先手动安装 source map 支持，以应对前面引入的查询字符串缓存绕过 hack。然后开始处理内容：
   1. 清理输出目录。
   2. 递归地对 `content` 文件夹中的所有文件进行 glob，遵循 `.gitignore`。
   3. 解析 Markdown 文件。
      1. Quartz 会检测可用线程数，如果要解析的内容超过 128 个（粗略估算），则选择生成 worker 线程。如果需要生成 worker，会再次调用 esbuild 转译 worker 脚本 `quartz/worker.ts`。然后创建一个工作窃取的 [workerpool](https://www.npmjs.com/package/workerpool)，将 128 个文件一批分配给 worker。
      2. 每个 worker（或如果没有并发则为主线程）会基于 [[configuration]] 中定义的插件创建一个 [unified](https://github.com/unifiedjs/unified) 解析器。
      3. 解析分为三步：
         1. 将文件读入 [vfile](https://github.com/vfile/vfile)。
         2. 对内容应用插件定义的文本转换。
         3. 对文件路径进行 slugify，并存储在文件的数据中。关于 Quartz 中路径逻辑的更多细节，请参见 [[paths]] 页面（剧透：很复杂）。
         4. 使用 [remark-parse](https://www.npmjs.com/package/remark-parse) 进行 Markdown 解析（文本转为 [mdast](https://github.com/syntax-tree/mdast)）。
         5. 应用插件定义的 Markdown 到 Markdown 转换。
         6. 使用 [remark-rehype](https://github.com/remarkjs/remark-rehype) 将 Markdown 转为 HTML（[mdast](https://github.com/syntax-tree/mdast) 到 [hast](https://github.com/syntax-tree/hast)）。
         7. 应用插件定义的 HTML 到 HTML 转换。
   4. 使用插件过滤掉不需要的内容。
   5. 使用插件输出文件。
      1. 收集每个 emitter 插件声明的所有静态资源（如外部 CSS、JS 模块等）。
      2. 输出 HTML 文件的 emitter 这里会做一些额外工作，需要将解析步骤中生成的 [hast](https://github.com/syntax-tree/hast) 转为 JSX。这是通过 [hast-util-to-jsx-runtime](https://github.com/syntax-tree/hast-util-to-jsx-runtime) 和 [Preact](https://preactjs.com/) 运行时完成的。最后，使用 [preact-render-to-string](https://github.com/preactjs/preact-render-to-string) 将 JSX 静态渲染为 HTML（即不关心 `useState`、`useEffect` 或其他 React/Preact 交互特性）。在这里，我们还会做很多有趣的事情，比如从 `quartz.layout.ts` 组装页面 [[layout]]，组装所有实际发送到客户端的内联脚本，以及所有转译后的样式。大部分逻辑在 `quartz/components/renderPage.tsx` 中。其他有趣的点包括：
         1. 使用 [Lightning CSS](https://github.com/parcel-bundler/lightningcss) 对 CSS 进行压缩和转换，添加供应商前缀并降级语法。
         2. 脚本分为 `beforeDOMLoaded` 和 `afterDOMLoaded`，分别插入 `<head>` 和 `<body>`。
      3. 最后，每个 emitter 插件负责输出并写入自己的文件到磁盘。
   6. 如果检测到 `--serve` 标志，还会设置另一个文件监听器，检测内容更改（仅 `.md` 文件）。我们维护一个内容映射，跟踪每个 slug 的已解析 AST 和插件数据，并在文件更改时更新。新添加或修改的路径会被重建并加入内容映射。然后，所有过滤器和 emitter 会在结果内容映射上运行。该文件监听器有 250ms 的防抖阈值。成功后，会通过传入的回调函数发送客户端刷新信号。

## 在客户端

1. 浏览器打开 Quartz 页面并加载 HTML。`<head>` 还会链接页面样式（输出到 `public/index.css`）和页面关键 JS（输出到 `public/prescript.js`）
2. 当 body 加载完成后，浏览器会加载非关键 JS（输出到 `public/postscript.js`）
3. 页面加载完成后，会分发一个自定义的合成浏览器事件 `"nav"`。这样，组件声明的客户端脚本可以“设置”任何需要访问页面 DOM 的内容。
   1. 如果在 [[configuration]] 中启用了 [[SPA Routing|enableSPA option]]，则在任何客户端导航时也会触发 `"nav"` 事件，以便组件注销和重新注册事件处理器及状态。
   2. 如果未启用，则只会在页面加载后触发一次 `"nav"` 事件，以保证在 SPA 和非 SPA 场景下状态设置方式一致。

插件系统的架构和设计在这里故意没有详细描述，更多内容请参见 [[making plugins|making your own plugin]] 指南。

