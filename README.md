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
- loaders用来解析webpack不能解析的文件，用loaders去解析额
- plugins用于增强loaders功能，优化，管理，环境变量注入，例如构建前删除目录
- plugins作用于整个构建过程

### 2.4 mode
- 设置不同的mode会有不同的内置函数