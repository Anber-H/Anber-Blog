---
title: 搭建前端UI组件库
date: 2020-05-28 18:16:11
tags:
---
# 项目初始化
使用vue-cli脚手架快速搭建一个项目
```
vue create demo
```

# 创建的时候选择：(也可根据自己需求选择)
```
Manually select features
↓
babel、CSS Pre-processors、Linter/formatter
↓
Sass/SCSS(with dart-sass)
↓
ESlint+Standard config
↓
Lint on save->In package.json
↓
no
```
<!--more-->
# 启动项目
cd demo
yarn serve

# 修改目录结构
```
├── dist        // 打包后的文件（即用户安装的包）
├── examples        // 示例代码（即原本的src修改为examples,用作测试组件）
    ├── App.vue
    ├── main.js
├── packages        // 组件包
    ├── fonts       // 字体包
    ├── index.js    // 整个包的入口
    ├── xxx.vue     // 组件
├── public
├── .editorconfig
├── .gitignore
├── .npmignore
├── .babel.config.js 
├── vue.config.js       //配置文件
└── package.json
```

#### vue.config.js
```
const path = require('path')
module.exports = {
  pages: {
    index: {
      //修改项目的入口文件
      entry: 'examples/main.js',
      template: 'public/index.html',
      filename: 'index.html'
    }
  },
  // 扩展 webpack 配置，使 packages 加入编译
  chainWebpack: config => {
    config.module
      .rule('js')
      .include.add(path.resolve(__dirname, 'packages')).end()
      .use('babel')
      .loader('babel-loader')
      .tap(options => {
        // 修改它的选项...
        return options
      })
  }
}
```

### packages/index.js
(导出install方法)
```
import xxx from './xxx' // packages里面的组件（有几个组件引几个组件）
import './fonts/iconfont.css'

const components = [
  xxx
]
const install = function (Vue) {
    // 全局注册所有组件
  components.forEach(item => {
    Vue.component(item.name, item)
  })
}
// 判断是否直接引入文件，如果是，就不用调用 Vue.use()
if (typeof window !== 'undefined' && window.Vue) {
  install(window.Vue)
}

export default { install }

```

### 在examples里面测试
1、修改examples/main.js
```
// 新增如下两行代码
// xxUI为自己定义的组件名
import xxUI from '../packages'
Vue.use(xxUI)
```
2、之后在App.js里面就可以直接用组件，类似element-ui的用法
```
<xx-button>按钮</xx-button>
```

# 构建成库
在package.json中添加命令(指定相应的打包文件)
```
"lib": "vue-cli-service build --target lib packages/index.js"
```
```
// 执行命令
yarn lib
// 会生成一个dist打包后的文件
```

# 将项目放到github
组件使用方法readme.md
```
## 初始化vue项目
`vue create demo`

## 安装组件库
`yarn add ly-ui`

## 全局导入
`import LyUI from 'lyln-ui'`

`import 'lyln-ui/dist/ly-ui.css'`

`Vue.use(LyUI)`
```

# 将包发布到npm
1. 修改package.json
```
"private": false,
"main": "dist/xx-ui.umd.min.js", // 包的入口文件
```
2. package.json中的name必须是npm上没有的名字，否则午发上传
3. 新增.npmignore文件
```
# 忽略目录
examples/
packages/
public/

#忽略指定文件
vue.config.js
babel.config.js
*.map
```

# npm上传命令
```
nrm ls // 按装nrm，用nrm检查源是不是npm，如果不是改成npm
npm login // 登录npm，如果npm没有账号就去注册一个
npm publish // 发布完之后就能在npm上看到自己的包了
```
注：每次打完包上传是需要该版本的（package.json里面的version）