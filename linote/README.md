# electron+vue3+ts
<center>
<img src="https://img.shields.io/badge/vue-3.0.4-green"/>
<img src="https://img.shields.io/badge/electron-%5E11.1.0-brightgreen"/>
<img src="https://img.shields.io/badge/typescript-~3.9.3-yellowgreen"/>
<img src="https://img.shields.io/badge/nedb-%5E1.8.0-orange"/>
</center>

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/83e3a65a44524252af6adf0135270216~tplv-k3u1fbpfcp-watermark.image)

```
electron-notes
├── public
│   ├── css
│   └── index.html
├── src
│   ├── assets
│   ├── components
│   ├── config
│   ├── inotedb
│   ├── less
│   ├── router
│   ├── script
│   ├── store
│   ├── utils
│   ├── views
│   ├── App.vue
│   ├── background.ts
│   ├── main.ts
│   └── shims-vue.d.ts
├── .browserslistrc
├── .eslintrc.js
├── .prettierrc.js
├── babel.config.js
├── inoteError.log
├── LICENSE
├── package-lock.json
├── package.json
├── README.md
├── tsconfig.json
├── vue.config.js
└── yarn.lock
```
- public公共资源css等等
- src 主要开发内容
  - assets 静态图片资源
  - components 组件
  - config 配置
  - inotedb nedb的封装
  - script 脚本
  - router 路由
  - store 状态管理
  - utils 工具
  - views 页面
  - App.vue
  - background.ts入口文件
  - main.ts vue入口文件

## 启动
```
yarn electron:serve
```

## 打包
```
yarn electron:build
```
