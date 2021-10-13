## 1. 初始化项目
- npm init -y
- npm install webpack@4.31.0 webpack-cli@3.3.2 --save-dev
- ./node_modules/.bin/webpack

## 2. 基础用法

### 2.1 entry 和 output
- entry可以配置多入口，此时output的文件名要用占位符`[name].js`来表示，打包出两个不同文件

### 2.2 loaders
- 写在module/rules数组中，test表示匹配的文件，use表示使用的loader

### 2.3 plugins
- loaders用来解析webpack不能解析的文件，用loaders去解析
- plugins用于增强loaders功能，优化，管理，环境变量注入，例如构建前删除目录
- plugins作用于整个构建过程

### 2.4 mode
- 设置不同的mode会有不同的内置函数

### 2.5 解析配置

#### 2.5.1 解析es6
- ES6的解析依赖babel-loader，babel-loader依赖babel，babel的配置文件是.babelrc
- babel的两个核心概念，plugins代表一个个功能，presets代表一些列plugins的集合
- npm i @babel/core@7.4.4 @babel/preset-env@7.4.4 babel-loader@8.0.5 -D

#### 2.5.2 解析React和jsx
- npm i react@16.8.6 react-dom@16.8.6 @babel/preset-react@7.0.0 -D

#### 2.5.3 解析CSS/Less/Sass
- npm i style-loader@0.23.1 css-loader@2.1.1 -D
```js
// loader 是链式调用，执行顺序从右到左，实际执行是先css-loader解析css，结果传递给style-loader，注意顺序
use: [
      'style-loader',
      'css-loader'
]
```
- npm i less@3.9.0 less-loader@5.0.0 -D

#### 2.5.4 解析图片和字体
- file-loader可以用于解析图片和字体
- npm i file-loader@3.0.1 -D
- url-loader也可以处理图片和字体，可以设置较小资源自动转为base64打包到引入文件
- npm i url-loader@1.1.2 -D
```js
            {
                test: /.(png|jpg|gif|jpeg)$/,
                use: [
                    {
                        loader: 'url-loader',
                        options: {
                            limit: 10240
                          // 单位是字节  10240是10k，图片小于10k，会打包成base64打包进引入文件
                        }
                    }
                ]
            },
```

### 2.6 动态执行，文件监听

- 启动webpack加--watch参数，此种方式需要每次手动刷新浏览器
```js
  "scripts": {
    "build": "webpack",
    "watch": "webpack --watch"
  },
```
- 配置文件加 watch: true
- 文件监听原理见ppt

### 2.7 热更新

#### 2.7.1 webpcak-dev-server
- 开发环境用 -> mode: 'development'
- 配合 HotModuleReplacementPlugin 插件用，这个插件是webpack内置的
- wds会自动更新，不输出文件，放在内存中
- npm i webpack-dev-server@3.3.1 -D
- `--open`每次构建完成自动开启浏览器
```js
"dev": "webpack-dev-server --open"

// 配置文件中配置插件以及devserver
  const webpack = require('webpack');
  plugins: [
    new webpack.HotModuleReplacementPlugin(),
  ],
  devServer: {
    contentBase: './dist',     // 手动刷新，引入文件没有配置
    hot: true
  }
```
- search.js放在dist中
- <script src='./search.js' type="text/javascript"></script> 已经在dist目录下
#### 2.7.2 webpack-dev-middleware
- 更灵活
- 需要引入node server
#### 2.7.3 热更新原理

### 2.8 文件指纹
- 用来做版本管理，对于没有修改的文件，用本地缓存，加速访问
- js文件用chunkhash， css文件用contenthash，取hash的8位
- 文件指纹和热更新不能一起使用，创建新的dev环境的webpack配置

- 拆分webpack配置文件，在package.json中指定环境

- css文件是插入解析css后，传递给style，插入header的，所以先用插件把css提取成独立的文件，加8位hash指纹
  - npm i mini-css-extract-plugin@0.6.0 -D
  - style-loader是插入header，而 MiniCssExtractPlugin是提取出css，功能互斥，加MiniCssExtractPlugin要删掉css-loader

### 2.9 代码压缩

#### 2.9.1 HTML压缩（把html也压缩到dist中）
- 修改 html-webpack-plugin 设置压缩参数，处理空格换行等
- npm i html-webpack-plugin@3.2.0 -D
#### 2.9.2 CSS压缩
- optimize-css-assets-webpack-plugin 同时使用 cssnano 的css处理器 
- npm i optimize-css-assets-webpack-plugin@5.0.1 -D
- npm i cssnano@4.1.10 -D
#### 2.9.3 JS压缩
- webpack内置了 uglifyjs-webpack-plugin 压缩文件，mode: 'production'时，默认开启，不需要额外安装操作
- 如果需要配置额外操作，类似并行压缩，需要安装 npm i uglifyjs-webpack-plugin@2.1.2 -D
