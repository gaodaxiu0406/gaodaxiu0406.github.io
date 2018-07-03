## commonjs规范  node



> 浏览器不支持commonjs规范

## es6规范
- import  export(node不支持)
- export let  a = 100 一个一个导出
- export default {} 全部导出

## webpack 打包 (开发不考虑ie8及以下)
- 会将css js 打包到一个文件中
- 帮压缩 帮合并
- 可以帮我们解决兼容性问题 es5

> 插件需要记

# 步骤
## 1.新建一个文件夹

## 2.npm init -y 生成一个文件package.json
- 此文件不能写注释
```
{
  "name": "webpack",//此处的名字不能与目录文件夹名字相同 否则会
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"//在此处匹配命令-->原理:我们下载的webpack生成一个node_modules目录,webpack会帮我们自动处理，node_modules-->.bin-->webpack (node  "$basedir/../webpack/bin/webpack.js" "$@")-->这句话就会运行webpack.js
    改成脚本语言:
    "scripts": {
        "start": "webpack"
      }-->给脚本webpack起个名字叫start,那么我们可以直接在cmd中使用npm run srtart启动文件
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```
## 3.安装npm install webpack
- 采用本地安装 不会导致版本冲突(兼容性问题 每个人下载的webpack版本可能不一样)
- 开发依赖  上线不需要
## 4.npm install nrm -g全局安装nrm
nrm test测试哪个网速最快
nrm use taobao

> package-lock.json 文件是帮忙锁定webpack版本的,不需要理会

## 5.开发依赖下载
```
npm install webpack --save-dev
```

## 6.建一个入口文件main.js

## 7.在webpack.config.js中设置入口和出口

#### 7-1.用es6 需要改编成es5 匹配js 变成es5  需要下载使用babel  需要babel-core babel-loader

#### 7-2.下载babel (babel-core 和 babel-loader)开发依赖模块
```
npm install babel-core babel-loader --save-dev
```
--save-dev可以简写成-D， babel-core babel核心

#### 7-3.需要告诉她认识es5
告诉babel根据什么去转
新建一个.babelrc的文件(此文件不能注释)
```
{
  "presets": ["es2015"]
}
```
- presets": ["es2015"]-->预设es2015 es2015就是es6
#### 7-4.需要将es6转为es5 需要babel-preset-es2015(开发依赖)
```
npm install babel-preset-es2015 --save-dev
```

#### 7-5.es7转换成es5，用"stage-0" 在.babelrc文件中也要加上"stage-0"
- 安装"stage-0"  全称:babel-preset-stage-0
```
npm install babel-preset-stage-0 --save-dev
```
- "stage-0""stage-1""stage-2"三个版本 "stage-0"支持的内容最多包含"stage-1" "stage-2""stage-3"的所有内容  ； 数字越大支持的越少"stage-4"就是es2015

#### 7-6.在webpack.config.js中添加exclude:/node_modules/ 这句话如下:
```
{test:/\.js$/,use:'babel-loader',exclude:/node_modules/}
```
- test测试 exclude:/node_modules/ node_modules这个文件夹不需要babel-loader来编译的  exclude表示忽略

## 解析css
-  需要两个loader css-loader style-loader以style标签的形式插入到html中
```
npm install style-loader css-loader --save-dev
```
- 从右往左写，实际先转换成css，再插到style标签里
    - 在webpack.config.js中添加出口文件如下:
```
{test:/\.css/,use:['style-loader','css-loader']}
```

## 解析less 开发
```
npm install less less-loader --save-dev
```

## 查看某个文件有没有安装
```
//例如less
less //输入less，如果报错'less' 不是内部或外部命令，也不是可运行的程序或批处理文件。-->说明没有在全局安装过less
```

## 查看全下安装过什么
```
npm root -g
```
    - 命令行会输出一个地址，这个地址就是npm命令安装的全局文件的地址，打开这个地址就可以看到所有安装的全局文件

> 一个import导入(css/less)，页面上就会生成一个style标签:`<style>...</style>`

## 实现自动重启
## 安装webpack服务 在内存中打包 可以时刻安装最新的改动
```
npm install webpack-dev-server --save-dev
```
- 这个服务帮我们配置一个端口号,并且实时刷新
```
  "scripts": {
    "build": "webpack",//会产生实体文件
    "dev":"web-dev-server"//以当前目录为基准,实现一个虚拟的服务，可以帮我们在内存中打包 不会产生实体文件-->实时获取数据
  },
```

## 自动将src下的html打包到dist下
## html-webpack-plugin插件安装

```
npm install html-webpack-plugin --save-dev
```

- 第三方模块  在webpack.config.js中引入插件
```
let HtmlWebpackPlugin=require('html-webpack-plugin');//HtmlWebpackPlugin是一个类
```

- 插件匹配 在webpack.config.js中
```
plugins:[//可以放很多插件
        new HtmlWebpackPlugin({
            template:'./src/index.html'//会自动将此html引入打包后的文件 导出到dist目录下-->也就是说src下永远放的是我们的源码
        })
    ]
```

## 解析图片问题(背景图会自动base64)
- 解析图片url-loader  url-loader需要file-loader
```
npm install url-loader file-loader --save-dev
```
- 在webpack.config.js中配置
```
{test:/\.(jpg|png|gif)$/,use:["url-loader?limit=8192"]},//8*1024=8192
```
    - ?limit=8192-->一般情况下8k以下的转化成base64 问号传参limit

## vue-cli自动生成webpack(脚手架)
- 需要安装vue-cli(命令行工具 需要加-g全局安装)
```
npm install vue-cli -g
```
#### vue-cli可以帮我们生成项目

- 初始化一个项目(随便找个目录)

- 1.第一种 一般不用
```
vue init simple 项目名  //-->一般不用
```

- 2.第二种 一般不用
```
vue init webpack-simple 项目名  //-->css js babel vue webpak都配置好了  resove别名(路径可以有别名)
```
- 项目下的render
```
render:h=>h(App)

var Home={template:'<h1>hello world</h1>'}
template:'<Home/>',
components:{Home}


render:function(creatElement){
    return creatElement('h1','hello')
    return creatElement(App)//这里可以直接写一个组件
}
```

- 3.第三种
```
vue init webpack vue-webpack(文件名)
```

## 1.vue = runtime + complier
#### Runtime + Compiler: recommended for most users

## 2.Runtime-only 只能用render函数 比第一种小6kb  一般用这个
#### Runtime-only: about 6KB lighter min+gzip, but templates (or any Vue-specific HTML) are ONLY allowed in .vue files - render functions are required elsewhere

Install vue-router? (Y/n)?  Y
是否用vue-router路由
Use ESLint to lint your code? n
Setup unit tests with Karma + Mocha? n测试框架不要
? Setup unit tests with Karma + Mocha? No
? Setup e2e tests with Nightwatch? No

## 安装后使用 只需要安装相关的模块即可,例如less
```
npm install less less-loader --save-dev
```
- 使用时需要在标签上设置语言lang="less"
```
<style scoped lang="less">
  div{
    h1{
      background-color: aquamarine;
    }
  }
</style>
```


## vue的使用方法 (1.html+css+js)(2.工程化)









