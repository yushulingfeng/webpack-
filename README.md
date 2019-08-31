# Webpack 配置

```js
const path = require('path');
module.exports = {
// Webpack打包的入口
  entry: "./app/entry", 
  
  output: {  // webpack输出选项
  // 打包输出的目标路径
    path: path.resolve(__dirname, "dist"), 
    // 文件命名
    filename: "[chunkhash].js", 
    // 构建文件的输出目录
    publicPath: "/assets/", 
 
    /* 其它高级配置 */
  },
  module: {  // 模块相关配置
    rules: [ // 配置模块loaders，解析规则
      {
        test: /\.jsx?$/, 
        include: [ // 和test一样，必须匹配选项
          path.resolve(__dirname, "app")
        ],
        exclude: [ // 必不匹配选项（优先级高于test和include）
          path.resolve(__dirname, "app/demo-files")
        ],
        loader: "babel-loader", // 模块上下文解析
        options: { // loader的可选项
          presets: ["es2015"]
        },
      },
  },
  resolve: { //  解析模块的可选项
    modules: [ // 模块的查找目录
      "node_modules",
      path.resolve(__dirname, "app")
    ],
    extensions: [".js", ".json", ".jsx", ".css"], // 用到的文件的扩展
    alias: { // 模块别名列表
      "module": "new-module"
	  },
  },
  devtool: "source-map", 
  plugins: [ 
   // 附加插件列表
  ],
}
```

webpack配置中几个核心的概念`Entry` 、`Output`、`Loaders` 、`Plugins`、 `Chunk`

- `Entry`：指定`webpack`开始构建的入口模块，从该模块开始构建并计算出直接或间接依赖的模块或者库

- `Output`：告诉`webpack`如何命名输出的文件以及输出的目录

- `Loaders`：由于`webpack`只能处理`javascript`，所以我们需要对一些非js文件处理成`webpack`能够处理的模块，比如`sass`文件

- `Plugins`：`Loaders`将各类型的文件处理成`webpack`能够处理的模块，`plugins`有着很强的能力。插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量。但也是最复杂的一个。比如对js文件进行压缩优化的`UglifyJsPlugin`插件

- `Chunk`：`coding` `split`的产物，我们可以对一些代码打包成一个单独的`chunk`，比如某些公共模块，去重，更好的利用缓存。或者按需加载某些功能模块，优化加载时间。在`webpack3`及以前我们都利用`CommonsChunkPlugin`将一些公共代码分割成一个`chunk`，实现单独加载。在`webpack4` 中`CommonsChunkPlugin`被废弃，使用`SplitChunksPlugin`

