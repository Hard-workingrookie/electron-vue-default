# 如何搭建

```
npm install -g vue-cli  
vue init simulatedgreg/electron-vue ele-vue
cd ele-vue
npm install
npm run dev
```

## 如果遇到 ReferenceError process is not defined 这个问题
### 解决方法：
1、找到根目录下的.electron-vue目录

2、找到该目录下的webpack.renderer.config.js文件，找到这段代码：

```
new HtmlWebpackPlugin({
  // ...
})
```
3、用下面的代码将这段代码替换：
```
new HtmlWebpackPlugin({
      filename: 'index.html',
      template: path.resolve(__dirname, '../src/index.ejs'),
      minify: {
        collapseWhitespace: true,
        removeAttributeQuotes: true,
        removeComments: true
      },
      templateParameters(compilation, assets, options) {
        return {
          compilation: compilation,
          webpack: compilation.getStats().toJson(),
          webpackConfig: compilation.options,
          htmlWebpackPlugin: {
            files: assets,
            options: options
          },
          process,
        };
      },
      nodeModules: process.env.NODE_ENV !== 'production'
        ? path.resolve(__dirname, '../node_modules')
        : false
 }),
```
4、再找到该目录下的webpack.web.config.js文件，找到这段代码：

```
new HtmlWebpackPlugin({
  // ...
})
```
5、用下面的代码将这段代码替换：

```
new HtmlWebpackPlugin({
      filename: 'index.html',
      template: path.resolve(__dirname, '../src/index.ejs'),
      templateParameters(compilation, assets, options) {
        return {
          compilation: compilation,
          webpack: compilation.getStats().toJson(),
          webpackConfig: compilation.options,
          htmlWebpackPlugin: {
            files: assets,
            options: options
          },
          process,
        };
      },
      minify: {
        collapseWhitespace: true,
        removeAttributeQuotes: true,
        removeComments: true
      },
      nodeModules: false
}),
```
最后重新编译就解决了（两个文件替换的内容是不一样的）

# ele-vue

> An electron-vue project

#### Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:9080
npm run dev

# build electron application for production
npm run build


```
#### 项目使用了axios+element-ui+vuex+vue-router+iconfont(阿里巴巴)

# 打包
直接使用npm run build就可以打包，若是要针对不同平台则按需添加参数，打包后的安装包在项目的build文件夹下

---

This project was generated with [electron-vue](https://github.com/SimulatedGREG/electron-vue)@[45a3e22](https://github.com/SimulatedGREG/electron-vue/tree/45a3e224e7bb8fc71909021ccfdcfec0f461f634) using [vue-cli](https://github.com/vuejs/vue-cli). Documentation about the original structure can be found [here](https://simulatedgreg.gitbooks.io/electron-vue/content/index.html).
