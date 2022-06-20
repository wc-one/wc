# Web Component One

Modern native dynamic component WEB development framework.[homepage](https://webcomponent1.com) [NPM](https://www.npmjs.com/package/@wc1/wc) [中文](./README.cn.md)

## 1. Re-engineering of wheels

> The development of WEB front-end is becoming more and more complicated and bloated. We have been considering whether there is a way to apply both
> Modern WEB development technologies such as MVVM, Typescript, etc., can be developed intuitively and concisely?
> We have tested many open source frameworks, such as vue, angular, etc., when the important goals we expect have failed or are inconvenient to achieve,
> So we decided to try to do it ourselves. Fortunately, after a year of development and project trials, our expectations have been fulfilled.

## 2. Primary goal

> The primary goal of this framework is to achieve componentized development, dynamic loading, and dynamic assembly, in order to solve problems such as continuous iteration, multi-person and multi-team collaborative development, version release, and launch in the face of complex businesses and products.
> Our ideal goal is that each interface element component is released dynamically, realizes online assembly and assembly, and supports online development and collaboration.
> There are many common development scenarios where this feature will be used.
>
> - Different from other frameworks, our focus is: **no packaging**,**dynamic dependencies** , All componentization methods are implemented by standard WebComponent technology, and data model-driven is implemented at the same time.
> - Therefore, we named this framework as **Web Component One** , Referred to as:**WC1** .

- **REAL DOM** Modern browser performance and optimizations are nearly powerful, we tested actual DOM performance, and when properly designed, it can fully meet the needs of the project, and can provide better performance and operability. Therefore, we decided to abandon VDOM and implement it entirely with native DOM, and combined with technologies such as template preloading, merge update, and dynamic change dependencies to finally implement the entire framework.

- **Minimum size independent** In order to optimize the size and improve performance, this framework does not rely on any third-party components. Currently, the complete function library including dynamic loading and other functions is about 60K in size, and about 20K after gzip compression.

## 3.Features

The following are the basic features that have been implemented so far:

- _Dynamic loading and dependency loading_:
  > Full dynamic and dependency loading supported* components * ,_Third-party library_,JS,CSS; Each component can define its own dependencies. When the page uses this component, the relevant dependencies are automatically loaded, and repeated loading can be avoided.
- _MVVM view and data dependency changes_:
  > Simplified data view model, similar to VUE
- _Enhanced style data binding_:
  > Direct data binding to any style
- _Development mode hot update_
- _Automatic color matching management_
- _template loading and caching_
- _routing_
- _Animation support_
- _UI library support_
  > The introduction of UI library makes it possible to use the most familiar HTML standard tags, and it is convenient to replace UI library, style, style, etc.
- ...

### 4. quick start

- **Regression and Intuitive**

  > In the past, front-end development was very intuitive, writing HTML, adjusting JS, and changing CSS...
  > Now we have to toss a lot of packaging, environments, segmentation, frameworks..., and the energy that should be focused on aesthetics and design has been largely placed on code and environment.
  > Therefore, we expect to return to the original, let's try it.

- **Get started quickly**

  > WC1's project engineering is very simple, no more additional libraries and dependencies are needed, and you can do it directly.

  - **Main entry index.html:**
    Create a new file with the following contents:

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

    - here*\<meta name="npm"\>* Represents the location of the npm package repository, the location of the required third-party packages and components, here we use the standard CDN
    - \_\<main-\>\</main-\>\_The tag indicates that this is our component, and due to the requirements of the WebComponent specification, custom components must contain**"-"** This tag represents that main.html in the current directory will be loaded as a component.

  - ** Main page component main.html: **Create a new file with the following content( `./simple1/main.html`):

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

    - This is a standard component
    - \_ `<template @timer.2000="counter++" >` \_Component standard entry label, all components are this entry. @timer represents the current component event, you can bind any DOM event or component standard event(timer, ready...)— \
    - \_ `<meta name="scope"/>` \_The current component scope, you can understand it as the component scope of Vue, the variables defined in it can be used directly through data binding.
    - _ `html <h3 $>counter</h3>` _ and\_ `<h5:>This is a Counter: ${counter}</h5>` \_This is data binding, where "\$" represents value binding, which means that the content is directly executed as a _js_ evaluation statement, and the result is passed to the binding element. Currently a single "\$" represents the text content bound to the element. The other is the ":" content binding, in fact, the ":" binding content is executed as an ES6 template string, which is easy to understand.
    - _ `<button @click="data.a++">click data</button>` _ This line uses "@" event binding, which is basically the same as in Vue. It can bind all native events and custom events, and can directly write execution statements(JS) or use methods defined in scope.
    - _ `<script scope=".">` \_Use js to import the scope, scope='.' means to import the root scope, you can customize the scope variable. js references use the standard AMD dynamic loading format. use_ script _ Defined scope variables and usage_ meta name=scope \_ Can be mixed or used alone.
    - \_ `border: ":${conter%10}px solid red";` \_You can see that data binding can be used directly in the style, and the format requires the use of**"** or**'** Include, the first character in quotation marks indicates whether to use \$ or \: quotation, the format is the same as above

- **run**
  No more operations are required, you can use any HttpServer to mount the current directory to run, or upload directly to the publisher, CDN, etc.
  > I am using _`pnpm i-g http-server`_ packages
  > can execute _`http-server-c-1./sample1`_ commands are directly executed and accessed* localhost:8080 * address.

### 5. Standard Project

Standard projects support hot updates, typescript and other features, without the need for complex packaging tools such as webpack, which can be opened and packaged in seconds.

> Standard projects require standard npm package management tools, we recommend using **pnpm**

- **Initialize directories and packages:** Create a new directory, then "_pnpm init_" to initialize the package.
- Install our command line tools globally* `@wc1/cli` *
- Put the source files from the previous section into the "src" subdirectory
- run in the project directory **wc1** Start the development environment
- For typescript support, install typescript globally: `pnpm i-g typescript`, create tsconfig.json, the example is as follows:

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

### 6. More

And more: $if,$show, $for, external js，typescript...

please goto homepage.

### 7. opensource

There is currently no open source, However, the release package uses the MIT License, If more people like it, I will open source in the future.
