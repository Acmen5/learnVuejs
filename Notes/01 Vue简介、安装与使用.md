### 一、Vue的简介

#### 1、Vue.js是什么？

1. Vue (读音 /vjuː/，类似于 **view**) 是一套用于构建用户界面的**渐进式框架**

2. 与其它大型框架不同的是，Vue 被设计为可以**自底向上**逐层应用
3. Vue 的核心库**只关注视图层**，不仅易于上手，还便于与第三方库或既有项目整合。

4. 与[现代化的工具链](https://cn.vuejs.org/v2/guide/single-file-components.html)以及各种[支持类库](https://github.com/vuejs/awesome-vue#libraries--plugins)结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。

#### 2、作者

> 尤雨溪

#### 3、作用

Vue框架的作用主要注重**动态的构建用户界面**，前端**工程化**和**模块化**开发



Vue与其他框架的对比



|  框架   | 设计模式 | 数据绑定 | 灵活度 | 文件模式  | 复杂性 | 生态 |
| :-----: | :------: | :------: | :----: | :-------: | :----: | :--: |
|   Vue   |   MVVM   |   双向   |  灵活  |  单文件   |   小   | 完善 |
|  React  |   MVC    |   单向   | 较灵活 | all in js |   大   | 丰富 |
| Angular |   MVC    |   双向   |  固定  |  多文件   |  较大  | 独立 |

> Vue 是一个推陈出新的前端框架，吸收了很多前端框架的优点。例如，Vue 借鉴了 React 的组件化和虚拟 DOM ，借鉴了 Angular 的模块化和数据绑定。

> 选择 Vue.js 的更多原因是，就框架的 API 而言，对比之下，Vue 更加轻便简洁。

### 二、安装

#### 1.  \<script\>标签引入

这种方式无需多言，直接将文件路径写入script标签的src中

下载地址：https://vuejs.org/js/vue.js

#### 2. CDN引入

不用下载到本地

使用方法如下:

```js
<script src="https://cdn.bootcss.com/vue/2.6.6/vue.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

两个cdn任选其一

#### 3. 脚手架工具vue-cli安装

（1）由于vue-cli是基于npm的，所以应该先安装node环境---下载npm

node下载地址(node官网)：http://nodejs.cn/

> npm是随同Node.js一起安装的包管理工具

（2）通过npm包管理工具全局安装vue-cli----安装脚手架vue-cli

```cmd
# 在终端输入npm指令
# npm命令如下
npm install -g vue-cli
```

（3）然后再命令行输入 vue ，出现 Usage 表示安装成功

（4）使用指令生成一个 vue 应用----生成一个vue应用

```cmd
vue init webpack sylApp # sylApp 这里是项目名AppName
# 系统提示输入如下
# 项目名称
# 项目描述
# 作者名
# 创建方式： 标准模式runtime-conpiler 与 runtime-only模式
# 	两者主要区别： main.js中
# 是否使用vue-router
# 是否遵循ESLint标准
# 是否安装单元测试： no
# 是否安装e2e测试：no
# 选择使用哪种包管理工具
# 	npm方式 或者 yarn 或者 自定义

```

（5）进入刚刚创建的app文件夹中，使用命令，运行vue应用

```cmd
npm run start
```

### 三、使用

每个**Vue应用**都是通过**Vue函数**创建一个新的**Vue实例**开始的

```js
const app = new Vue({
	// 选项
})
```



> 创建第一个Vue文件

```html
<!DOCTYPE html>
<html>
    <head>
    		<meta charset="UTF-8">
    		<title>Acmen5-vue-test</title>
    </head>
    <body>
        <div id="app">{{msg}}</div>
        <script src="https://cdn.bootcss.com/vue/2.6.6/vue.min.js"></script>
        <script>
        		const app = new Vue({
                    el: "#app",
                    data: {
                        msg: "hello world!"
                    }
                })
        </script>
    </body>
</html>
```

### 四、理解Vue的MVVM模式

![](https://doc.shiyanlou.com/document-uid940410labid10292timestamp1552636493741.png/wm)

M：Model 即数据逻辑处理

V：View 即视图（用户界面）

VM: ViewModel 即数据视图，用于监听更新，View 与 Model 数据的双向绑定



Vue的两大特点：

1. **数据的双向绑定** 
2. **响应式** 

接下来就来分别体验一下



#### 1、数据双向绑定

Vue中最常用的就是表单数据的双向绑定

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>初次体验Vue数据的双向绑定</title>
	</head>
	<body>
		<div id="app">
			<input type="text" v-model="msg">
			<p>{{msg}}</p>
		</div>
		<script src="https://cdn.bootcss.com/vue/2.6.6/vue.min.js"></script>
		<script>
			const app = new Vue({
				el: "#app",
				data: {
					msg: 'hello'
				}
			})
		</script>
	</body>
</html>

```

#### 2. 感受响应式

 Vue 是一个 MVVM 架构的框架

通过上面的例子成功创建了一个Vue应用，看起来这跟渲染一个字符串模板非常类似，但是Vue在背后做了大量工作。

因此现在**数据**和**DOM**已经被建立了关联，**所有东西都是响应式的**。

如何验证呢？

打开浏览器的JavaScript控制台，修改app.msg的值，然后就会发现上面的例子也相应的跟新，更改数据也触发视图的相应更新

