# Entry 入口起点

📌 Webpack 配置必填项。[文档传送门](https://webpack.docschina.org/concepts/entry-points)

* 起点或是应用程序的起点入口。
* 从这个起点开始，应用程序启动执行。
* 如果传递一个数组，那么数组的每一项都会执行。
* 📌 <u>动态加载</u>的模块**不是**入口起点。

简单规则：每个 HTML 页面都有一个入口起点。

单页应用（SPA）：一个入口起点。

多页应用（MPA）：多个入口起点。

## Context

Webpack 寻找相对路径的文件时会以 context 为根目录，默认为执行启动 Webpack 时所在当前工作目录。

context 必须是绝对路径字符串。

```js
module.exports = {
    context: path.resolve(__dirname, 'app')
}
```

Entry 路径及其依赖的模块的路径可能采用相对于 context 的路径来描述，context 会影响到这些相对路径所指向的真实文件。

## Entry 类型

| 类型   | 例子                               | 含义                                 |
| ------ | ---------------------------------- | ------------------------------------ |
| string | `./app/entry`                      | 入口模块的文件路径，可以使相对路径   |
| array  | `['./app/entry', './app/index']`   | 入口模块的文件路径，可以使相对路径   |
| object | `{a: 'main', b: ['index', 'app']}` | 配置多个入口，每个入口声称一个 Chunk |

如果是 array 类型，则搭配 `output.library` 配置项使用时，只有数组里的最后一个入口文件的模块会被导出。

如果是 objct 类型，则 entry 为可扩展的配置。可重用并且可以与其他配置组合使用。这是一种流行的技术，用于将关注点（concern）从环境（environment）、构建目标（build target）、运行时（runtime）中分离。然后使用专门的工具（如 [webpack-merge](https://github.com/survivejs/webpack-merge)）将它们合并。

## Chunk 命名

如果传入一个字符串或字符串数组，chunk 会被命名为 `main`。如果传入一个对象，则每个键（key）会是 **chunk** 的名称，该值描述了 **chunk** 的入口起点。

* 如果 entry 是一个 string 或 array，就只会声称一个 Chunk，这时 Chunk 的名称是 main
* 如果 entry 是一个 object，就可能会出现多个 Chunk，这时 Chunk 的名称是 object 键值中键的名称

## 动态入口

```js
// 同步函数
module.exports = {
    // ...
    entry: () => './demo'
}

// 异步函数
module.exports = {
    entrt: () => new Promise(resolve => {
        resolve({
            a: './page/a',
            b: './page/b'
        })
    })
}
```

## 常见场景

* 分离应用程序和第三方库入口 [传送门](https://webpack.docschina.org/concepts/entry-points#%E5%88%86%E7%A6%BB-%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F-app-%E5%92%8C-%E7%AC%AC%E4%B8%89%E6%96%B9%E5%BA%93-vendor-%E5%85%A5%E5%8F%A3)
* 多页面应用程序 [传送门](https://webpack.docschina.org/concepts/entry-points#%E5%A4%9A%E9%A1%B5%E9%9D%A2%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F)