---
title: astro.config
description: 'Lorem ipsum dolor sit amet'
pubDate: 'Jul 22 2022'
---

建议在大多数情况下使用默认文件格式 .mjs，如果你想在配置文件中编写 TypeScript，则使用 .ts。但是，也支持 astro.config.js。


```ts
import { defineConfig } from "astro/config";

export default defineConfig({
  site: "https://www.example.com",
  base: "/docs",
  trailingSlash: "always",
});
```

```ts
//src/components/MainLayout.astro
---
import Head from './Head.astro';

const { ...props } = Astro.props;
---
<html>
  <head>
    <meta charset="utf-8">
    <Head />
    <!-- head 部分的附加元素 -->
  </head>
  <body>
    <!-- 你的页面内容在这 -->
  </body>
</html>
```

```ts
---
import Favicon from '../assets/Favicon.astro';
import SomeOtherTags from './SomeOtherTags.astro';

const { title = 'My Astro Website', ...props } = Astro.props;
---
<link rel="sitemap" href="/sitemap-index.xml">
<title>{title}</title>
<meta name="description" content="Welcome to my new Astro site!">

<!-- 网站分析 -->
<script data-goatcounter="https://my-account.goatcounter.com/count" async src="//gc.zgo.at/count.js"></script>

<!-- 开放图谱标签 -->
<meta property="og:title" content="My New Astro Website" />
<meta property="og:type" content="website" />
<meta property="og:url" content="http://www.example.com/" />
<meta property="og:description" content="Welcome to my new Astro site!" />
<meta property="og:image" content="https://www.example.com/_astro/seo-banner.BZD7kegZ.webp">
<meta property="og:image:alt" content="">

<SomeOtherTags />

<Favicon />
```



官方 Astro VS Code 扩展，为 Astro 项目提供了几个关键特性并改善开发者体验。

    为 .astro 文件提供语法高亮
    为 .astro 文件提供 TypeScript 类型信息。
    VS Code 智能提示提供代码补全和提示

https://marketplace.visualstudio.com/items?itemName=astro-build.astro-vscode


Neovim LSP 和 TreeSitter 插件 社区 - 为 Neovim 内的 Astro 提供语法高亮、treesitter 解析和代码补全。

https://github.com/neovim/nvim-lspconfig/blob/master/doc/configs.md#astro

https://github.com/virchau13/tree-sitter-astro


https://ota-meshi.github.io/eslint-plugin-astro/user-guide/

https://github.com/ota-meshi/eslint-plugin-astro


