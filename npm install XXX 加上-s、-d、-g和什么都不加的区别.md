## 1. npm install XXX -s

- `npm install XXX -s` 
-  `npm install -S XXX` 
-  `npm install --save XXX`   

上述三种写法是一样的。这样安装是局部安装的，会写进 package.json 文件中的 dependencie 中的。

dependencie：表示**生产环境**下的依赖管理

即如果安装一个库是用来构建你的项目的，比如 echarts、element-ui，是实际在项目中起作用的，就可以使用 -s 来安装。

## 2. npm install XXX -d

- `npm install XXX -d`
- `npm install -D XXX`
- `npm install --save-dev XXX`

上述三种写法是一样的。这样安装是局部安装的，会写进 package.json 文件中的 devDependencie 中的。

devDependencie：表示**开发环境**下的依赖管理

即如果安装的库是用来打包的、解析代码的，比如 webpack、babel，就可以用 -d 来安装，项目上线了，这些库就没用了。

## 3. npm install XXX -g

`npm install XXX -g` 表示全局安装，安装一次后，你就可以在其他地方直接使用啦。

## 4. npm install XXX

`npm install XXX` 什么都不加的时候

在 npm5 开始可以通过 `npm install` 什么都不加 和 `npm install --save` 一样的。都是局部安装并会把模块自动写入 package.json 中的 dependencies里。