---
title: Astro project structure
description: 'Lorem ipsum dolor sit amet'
pubDate: 'Jul 08 2022'
---

Astro 项目的根目录：

- src/* - 项目源代码（组件、页面、样式、图片等）。
- public/* - 非代码、未处理的资源（字体、图标等）。
- package.json - 项目清单。
- astro.config.mjs - Astro 配置文件（推荐）。
- tsconfig.json - TypeScript 配置文件（推荐）。

## Astro 项目目录


- public/
  - robots.txt
  - favicon.svg
  - my-cv.pdf

- src/
  - blog/
    - post1.md
    - post2.md
    - post3.md

  - components/
    - Header.astro
    - Button.jsx

  - images/
    - image1.jpg
    - image2.jpg
    - image3.jpg

  - layouts/
    - PostLayout.astro

  - pages/
    - posts/
        - [post].astro
    - index.astro
    - rss.xml.js

  - styles/
    - global.css
  - content.config.ts

- astro.config.mjs
- package.json
- tsconfig.json

### src/

src/ 文件夹是大部分项目源代码所在的地方。这包括：

- 页面
- 布局
- Astro 组件
- UI 框架组件（React 等）
- 样式（CSS、Sass）
- Markdown
- 由 Astro 优化和处理的图像

Astro 处理、压缩和打包你的 src/ 文件以创建最终传递到浏览器的网站。与静态的 public/ 目录不同，src/ 文件是由 Astro 为你构建和处理的。

有些文件（如 Astro 组件）甚至不会按写入方式传递到浏览器，而是被渲染成静态 HTML。其他文件（如 CSS）则会被传递到浏览器，但可能被会压缩或与其他 CSS 文件打包在一起以提高性能。

Astro 保留的目录只有 src/pages/。你可以自由的以最适合自己的方式重命名和重新组织任何其他目录。

- src/pages

页面 是一种用于创建新的页面的特殊组件。一个页面可以是一个 Astro 组件，也可以是表示站点某些页面内容的 Markdown 文件。

src/pages 是 Astro 项目中必须要有的子目录。没有它，你的网站将没有任何页面或路径！

src/pages/ 中的文件负责处理路由、数据加载以及网站中每个页面的整体页面布局。

Astro 支持 src/pages/ 目录中的以下文件类型：

- .astro
- .md
- .mdx (需要安装 MDX 集成)
- .html
- .js/.ts (as endpoints)

Astro 使用一种名为 基于文件的路由 的路由策略。src/pages/ 目录中的每个文件都会根据其文件路径成为网站上的一个端点。

一个文件也可以使用动态路由来生成多个页面。这允许你即使内容不在特殊的 /pages/ 目录中，也可以创建页面，例如在内容集合或内容管理系统中。

Astro 页面 .astro 支持与 Astro 组件相同的功能。

一个页面必须生成完整的 HTML 文档。如果没有显式包含，Astro 将会自动为位于 src/pages/ 目录下的任何 .astro 组件默认添加必要的 <!DOCTYPE html> 声明和 <head> 内容。你可以通过将组件标记为局部页面来为每个组件避免这种行为。

为了避免在每个页面上重复相同的 HTML 元素，你可以将常见的 <head> 和 <body> 元素移动到自己的 布局组件 中。你可以使用任意多的布局组件。

---
<MySiteLayout>
  <p>包裹在 layout 里的页面内容！</p>
</MySiteLayout>

### Markdown/MDX 页面

Astro 还将 src/pages/ 目录中的任何 Markdown (.md) 文件视为网站中的页面。如果你安装了 MDX 集成，它也会将 MDX (.mdx) 文件视为相同的页面。

当内容为博客文章或者产品项目这类共享相似结构的相关 Markdown 文件时，请考虑创建 内容集合 而不是页面。

Markdown 文件可以使用特殊的 layout 前置属性来指定一个 布局组件，它将包装其 Markdown 内容在一个完整的 <html>...</html> 页面文档中。

HTML 页面

.html 文件扩展名的文件可以放在 src/pages/ 中，并直接用作网站上的页面。请注意，某些关键的 Astro 功能在 HTML 组件 中不受支持。


自定义 404 错误页面

想要自定义 404 错误页面，你可以在 src/pages 中创建 404.astro 或 404.md 文件。

它将生成 404.html 页面。大多数部署服务都自动找到并使用它。


自定义 500 错误页面

要为按需渲染的页面创建自定义的 500 错误页面，可以创建一个 src/pages/500.astro 文件。但这个自定义页面不适用于预渲染的页面。

如果在渲染此页面时发生错误，将显示主机默认的 500 错误页面给访问者。

添加于： astro@4.10.3

在开发过程中，如果你有一个 500.astro 文件，运行时抛出的错误将被记录在终端中，而不是显示在错误覆盖层中。

error

添加于： astro@4.11.0

src/pages/500.astro 是一个特殊页面，它会在渲染过程中对抛出的任何错误都会自动传递一个 error 属性。这允许你向访客展示详细的错误信息（例如来自页面、中间件等）。

error 属性的数据类型可以是任何类型，这可能会影响你在代码中的定义或使用值的方式：
src/pages/500.astro

---
interface Props {
  error: unknown;
}
const { error } = Astro.props;
---
<div>{error instanceof Error ? error.message : "Unknown error"}</div>

为了避免在展示内容时从 error 属性中显示敏感信息，请首先考虑评估错误，并根据抛出的错误返回适当的内容。例如，应避免显示错误的堆栈，因为它包含有关服务器上代码结构的信息。
局部页面

添加于： astro@3.4.0

警告

局部页面是用于与前端库（如 htmx 或 Unpoly）搭配使用的。如果你习惯编写原生的前端 JavaScript，也可以使用它。这是一项高级功能。

如果组件包含有作用域的样式或脚本，那么不应该使用局部页面，因为这些元素将从 HTML 输出中剥离；如果需要有作用域的样式，最好使用常规的非局部的页面，并配合一个知道如何将内容合并到 head 标签中的前端库。

局部页面是位于 src/pages/ 目录中的页面组件，但不会作为完整页面进行渲染。

与位于此文件夹之外的组件一样，这些文件不会自动包含 <!DOCTYPE html> 声明，也不会包含任何作用域样式和脚本的 <head> 内容。

不过，由于它们位于特殊的 src/pages/ 目录中，所以生成的 HTML 可以通过与文件路径相对应的 URL 来访问。这允许渲染库（如 htmx、Stimulus、jQuery 等）在客户端访问它，并在页面上动态加载 HTML 的部分，而无需刷新浏览器或进行页面导航。

局部页面与渲染库结合时，局部页面提供了一种可以代替 Astro 岛屿和 <script> 标签方式用于在 Astro 中构建动态内容的方法。

可以导出一个值为 partial 的页面文件（例如 .astro 和 .mdx，但不是 .md）可以标记为局部页面。
src/pages/partial.astro

---
export const partial = true;
---
<li>我是一个局部页面！</li>

和库搭配使用

局部页面用于使用诸如 htmx 的库动态更新页面的某个部分。

下面的示例展示了将 hx-post 属性设置为局部页面的 URL。局部页面的内容将用于更新此页面上目标 HTML 元素的内容。
src/pages/index.astro <html>
  <head>
    <title>我的页面</title>
    <script src="https://unpkg.com/htmx.org@1.9.6"
      integrity="sha384-FhXw7b6AlE/jyjlZH5iHa/tTe9EpJ1Y55RjcgPbjeWMskSxZt1v9qkxLJWNJaGni"
      crossorigin="anonymous"></script>
  </head>
  <body>
    <section>
      <div id="parent-div">Target here</div>

      <button hx-post="/partials/clicked/"
        hx-trigger="click"
        hx-target="#parent-div"
        hx-swap="innerHTML"
      >
        点我！
      </button>
    </section>
  </body>
</html>

.astro 局部页面必须在相应的文件路径中存在，并且包含一个导出定义将页面定义为局部页面：
src/pages/partials/clicked.astro

---
export const partial = true;
---
<div>我被点击了！</div>





- src/components

组件是你在 HTML 页面中可重复使用的代码单元。它可以是 Astro 组件 或是像 React 或 Vue 这样的 UI 框架组件。通常将你项目中所有组件都分组放在这个文件夹中。

这是 Astro 项目约定俗成的，但不是必需的。你可以随意组织你的组件！

- src/layouts

布局 是定义一个或多个 页面 共享的 UI 结构的 Astro 组件。

和 src/components 一样，这个目录也只是约定俗成，但不是必需的。

- src/styles

将 CSS 或 Sass 文件存储在 src/styles 目录中是一种常见的约定，但这不是必需的。只要你的样式位于 src/ 目录中的某个位置并且正确导入，Astro 就会处理和压缩它们。

- public/

public/ 目录用于项目中不需要在 Astro 构建过程中处理的文件和资源。这个文件夹中的文件将会被原封不动地复制到构建文件夹中，然后你的网站将被构建。

这种行为使 public/ 非常适合用来存放不需要任何处理的常见资源，如某些图片和字体，或特殊文件，如 robots.txt 和 manifest.webmanifest。

你也可以把 CSS 和 JavaScript 放在 public/ 目录中，但要注意这些文件不会在最终构建中被打包或压缩。

一般而言，你自己编写的 CSS 或 JavaScript 都应该放在 src/ 目录下。

- package.json

JavaScript 包管理器用它来管理依赖关系。它也定义了通常用于运行 Astro 的脚本（例如：npm run dev、npm run build）。

在 package.json 中可以指定 两种依赖关系：dependencies 和 devDependencies。在大多数情况下它们效果一样，Astro 在构建时需要所有依赖，而你的包管理器则会同时安装这两种依赖。我们建议把所有的依赖项放在 dependencies 中，只有在你发现有特殊需要后，再使用 devDependencies。

如果想要为你的项目创建新的 package.json 文件时遇到困难，请查看 手动安装 中的说明。

- astro.config.mjs

该文件在每个入门模板中都会生成，它包括你的 Astro 项目的配置。你可以在这里指定要使用的集成、构建选项、服务器选项以及其他内容。

Astro 支持多种 JavaScript 配置文件格式：astro.config.js、astro.config.mjs 以及 astro.config.ts。大多数情况下，我们更建议使用 .mjs，如果你想在配置文件中编写 TypeScript 则建议使用 .ts。

TypeScript 配置文件加载是使用 tsm 来进行处理的，并且会遵循你项目中的 tsconfig 选项。

- tsconfig.json

该文件在每个入门模板中都会生成，它包括你的 Astro 项目的 TypeScript 配置项。如果没有 tsconfig.json 文件，编辑器将不完全支持某些功能（如 npm 包的导入）。

