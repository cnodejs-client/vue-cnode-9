# vue-cnode

[Live demo](http://damonowl.himmas.cc/vue-cnode/demo)


# 项目说明

该项目为实操vue所写的SPA，本来为多人协作，本人为了学习到更多的vue实践操作而单独进行。感谢[cnode社区](https://cnodejs.org/)提供的api，感谢男叔和我开耐心指点及讲解。

# 技术范畴
vue + vue-router + axios + less + ES6；使用webpack进行打包

# 功能

* 主题列表，上拉加载
* 主题分类
* 主题详情，回复

# 目录结构
.
├── build                           # 构建脚本
│   ├── dev-server.js               # development server script
│   ├── webpack.base.conf.js        # shared base webpack config
│   ├── webpack.dev.conf.js         # development webpack config
│   ├── webpack.prod.conf.js        # production webpack config
│   └── ...
├── config                          # 配置文件
│   ├── dev.env.js                 
│   ├── index.js         
│   └── prob.env.js       
│
├── node_modules                    # node模块依赖
├── src
│   ├── components                  # 组件文件夹
│   │        ├── a.vue              # 单文件组件
│   │        ├── b.vue
│   │        └── ...
│   ├── router                      # 路由
│   │        ├── router.js            
│   │        └── routes.js 
│   ├── asserts                     # 资源文件夹
│   │        ├── images             # 图片
│   │        ├── css                # 样式表
│   │        └── fonts              # 字体
│   │
│   ├── main.js                     # webpack entry
│   └── ...
│
│
│
├── static                          # 静态资源目录               
└── package.json



## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For detailed explanation on how things work, checkout the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).
