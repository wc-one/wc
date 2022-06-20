# Web Component One

现代原生化动态组件 WEB 开发框架。
[主页](https://webcomponent1.com) [NPM](https://www.npmjs.com/package/@wc1/wc)

## 1. 轮子的再造

> WEB 前端的开发越来越复杂臃肿，我们一直在考虑，能否有一种方式，既能应用
> 现代的 WEB 开发技术如 MVVM，Typescript 等，又能够直观和简洁快速开发？
> 我们测试了许多开源框架，如 vue、angular 等,在我们所期望重要的目标都未能或者不方便实现，
> 因此我们决定尝试自己做一下。庆幸的是，经过一年来开发和项目试用，我们的预期得到实现。

## 2. 首要目标

> 本框架首要目标是实现组件化开发和动态加载，动态组装，以期解决在面向复杂业务和产品时的不断迭代，多人多团队协同开发、版本发布和上线等问题。
> 我们的理想目标是各个界面元素组件均是动态发布，实现线上组装和装配，支持在线的开发和协作。
> 有许多常见的开发场景将会用到这个特性。
>
> - 有别于其他的框架，我们的重点的是：**无打包**, **动态依赖**, 所有的组件化方式以标准 WebComponent 技术实现，同时实现数据模型驱动。
> - 因此，我们为本框架命名为 **Web Component One**, 简称为: **WC1**。

- **真实 DOM**
  现代浏览器性能和优化已近很强大了，我们测试过实际的 DOM 性能，在恰当的设计后完全可以满足项目需求，并且能够提供更优秀的性能和可操作性。
  因此，我们决定抛弃 VDOM，完全以原生 DOM 实现，并结合了模板预加载、合并更新、动态变更依赖等技术，来最终实现整个框架。

- **最小尺寸无依赖**
  为优化大小和提升性能，本框架不依赖任何第三方组件，目前包括动态加载等完整功能库大小约为 60K，gzip 压缩后 20K 左右。

## 3.功能

以下是目前已经实现的基本特性：

- _动态加载和依赖加载_：
  > 全动态和依赖加载可支持 _组件_,_第三方库_,JS,CSS; 每个组件均可定义自身依赖项，当页面使用到此组件时自动加载相关依赖，并能避免重复多次加载。
- _MVVM 视图和数据依赖变更_：
  > 简化的数据视图模型，类似 VUE
- _增强的样式数据绑定_：
  > 可直接数据绑定到任意 style
- _开发模式热更新_
- _自动配色管理_
- _模板加载和缓存_
- _路由_
- _动画的支持_
- _UI 库的支持_
  > UI 库的引入使得可以使用最熟悉的 HTML 标准标签，并可方便更换 UI 库，样式，风格等。
- ...

### 4. 快速开始

- **回归和直观**

  > 曾经的年代，前端开发很直观，写写 HTML，调调 JS，再改点 CSS...
  > 现在却要折腾许多打包、环境、分段、框架...，本该集中在美观和设计的精力却大量放在了代码和环境。
  > 因此，我们期望能回归原始，开始尝试下吧。

- **快速上手**

  > WC1 的项目工程很简洁，不需要更多的附加库和依赖，直接动手就行。

  - **主入口 index.html:**
    新建文件,内容如下:

    ```html
    <!DOCTYPE html>
    <html>
      <head>
        <meta name="npm" content="https://cdn.jsdelivr.net/npm/" />
      </head>
      <script src="https://cdn.jsdelivr.net/npm/@wc1/wc@1.5.12/index.js"></script>
      <body>
        <main-></main->
      </body>
    </html>
    ```

    - 这里的 _\<meta name="npm"\>_ 代表 npm 软件包仓库位置，所需要的第三方包和组件的位置，这里我们使用标准 CDN
    - _\<main-\>\</main-\>_ 标签代表这是一个我们的组件，由于 WebComponent 规范的要求, 自定义组件必须包含 **"-"**
      这个标签代表将要加载当前目录下的 main.html 作为组件。

  - **主页面组件 main.html:**
    新建文件,内容如下(`./simple1/main.html` ):

    ```html
    <template @timer.1000="counter++">
      <meta name="scope" counter.number="0" />
      <style>
        :host {
          display: block;
          border: ":${counter%10}px solid red ";
        }
        button {
          background-color: rgb("$counter*20%255", "$counter*50%255", 0);
        }
      </style>
      <h3>Main Content</h3>
      <h3 $>counter</h3>
      <h5 :>This is a Counter: ${counter}</h5>
      <button @click="data.a++">click data</button>
      <h3 $>data</h3>
      <button @click="onClick($ev)">click data</button>
      <h3 $if="counter%2">Hello World</h3>
      <script scope=".">
        define(() =>
          class {
            data = { a: 1, b: 2 };
            onClick() {
              alert("CLICK ME");
            }
          });
      </script>
    </template>
    ```

    - 这就是一个标准的组件
    - _`<template @timer.2000="counter++" >`_
      组件标准入口标签，所有组件均为此入口。@timer 代表当前组件事件,可以绑定任意的 DOM 事件或者组件标准事件(timer,ready ...) - \
    - _`<meta name="scope" />`_
      当前组件作用域,你可以理解为 Vue 的组件域，其中定义的变量可以直接通过数据绑定使用。
    - _`html <h3 $>counter</h3>`_ 和 _`<h5 :>This is a Counter: ${counter}</h5>`_
      这个就是数据绑定了, 其中 "\$" 代表值绑定, 代表直接执行内容作为*js*求值语句,并把结果传递给绑定元素。当前单独的 "\$" 代表绑定到元素的文本内容。
      另外就是 ":" 内容绑定，其实就是 ":" 绑定内容作为 ES6 的模板字符串执行，这个很容易理解。
    - _`<button @click="data.a++">click data</button>`_ 这一行使用了 "@" 事件绑定, 和 vue 中基本一致，可以绑定所有原生事件和自定义事件，
      可以直接写执行语句(JS)或者使用 scope 中定义的方法。
    - _`<script scope=".">`_
      使用 js 导入作用域, scope='.' 代表引入根作用域，可自定作用域变量。js 引用使用标准 AMD 动态加载格式。
      使用 _script_ 定义的作用域变量和使用 _meta name=scope_ 可以混用，也可单独使用。
    - _`border: ":${conter%10}px solid red";`_
      可以看到,数据绑定可直接使用在 style 中, 格式要求使用 **"** 或者 **'**包括, 引号内第一个字符代表使用 \$ 还是 \:引用, 格式同上

- **运行**
  无需更多操作，你可以使用任意 HttpServer 挂载当前目录即可运行，也可直接上传到发布服务器，CDN 等。
  > 我是用的是 _`pnpm i -g http-server`_ 软件包
  > 可以执行 _`http-server -c-1 ./sample1`_ 命令直接执行并访问 _localhost:8080_ 地址。

### 5. 标准项目

标准项目支持热更新，typescript 等特性，无需 webpack 等复杂打包工具，秒开和快速打包。

> 标准项目需要标准 npm 包管理工具, 我们推荐使用 **pnpm**

- **初始化目录和包:**
  新建目录,然后 "_pnpm init_" 来初始化包。
- 全局安装我们的命令行工具 _`@wc1/cli`_
- 将上一节的源文件放入 "src" 子目录
- 在项目目录下运行 **wc1** 即可启动开发环境
- 如需 typescript 支持, 全局安装 typescript : `pnpm i -g typescript`, 创建 tsconfig.json,样例如下:

```json
{
  "include": ["./src", "./package.json"],
  "exclude": [],
  "compilerOptions": {
    "target": "ES6",
    "module": "AMD",
    "lib": ["ES6"],
    "declaration": false,
    "outDir": "./build/",
    "removeComments": true,
    "resolveJsonModule": true,
    "moduleResolution": "node",
    "baseUrl": "./",
    "allowSyntheticDefaultImports": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  }
}
```

### 6. 更多

还有更多功能，如$if,$show, $for 的支持, 外部 js 支持，typescript 支持等,详情请关注首页。

### 7. 关于开源

本仓库不包含源码，仅包含文档以及示例。目前尚未开源，但是发布包遵循 MIT 协议，可任意免费使用。当前项目状态还在开发中，主体功能基本稳定，还有不少细节和特性还在修订和完善。
如喜欢本项目人数较多，我们会考虑尽快开源。
