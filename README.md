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
