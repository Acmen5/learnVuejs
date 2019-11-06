### Vue的基础模板语法

Vue.js 使用了基于 **HTML 的模板语法**，允许开发者声明式地将 **DOM** 绑定至**底层 Vue 实例**的**数据**。

所有 Vue.js 的模板都是**合法的 HTML** ，所以能被遵循规范的浏览器和 HTML 解析器解析

通俗的讲 **Vue 模板语法**就是在使用 Vue.js 开发时，你可以**写在 HTML 元素上**的**操作语法**，让你开发更高效，例如：绑定样式v-bind，循环出元素列表v-for等。

#### 一、 双大括号表达式 - Mustache语法

Vue.js 借鉴了 Angular.js 的双大括号的方式，进行**向页面输出数据**和**调用对象方法**

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>模板语法 - 双大括号表达式</title>
	</head>
	<body>
		<!-- 数据双向绑定 -->
		<div id="app">
			<input type="text" v-model="msg">
			<p>{{msg}}</p>
		</div>
		<!-- 通过CDN方式引入Vuejs -->
		<script src="https://cdn.bootcss.com/vue/2.6.6/vue.min.js"></script>
		<script>
			const app = new Vue( {
				// el挂载点 
				el: '#app',
				// 数据选项
				data: {
					msg: 'i am Acmen5',
				}
			})
		</script>
	</body>
</html>
```

双大括号中的 {{msg}} ，绑定至底层的 Vue 实例中的数据 ( data )，在浏览器中就被渲染成实例 data 选项中 msg 的值

#### 二、 插值

##### 1. 文本

在Vue.js中数据绑定最常见的形式就是使用 “ **Mustache** ” 语法的文本插值

**双大括号中的值**将会被替代为对应 data 对象上 **`msg` 属性的值**。

```html
<div id="app">msg: {{msg}} </div>
```

 

一次性插值指令 **v-once**

```html
<p v-once>{{msg}}</p>
//这条指令的作用：一次性地插值
```

##### 2.原始HTML

以上的双大括号表达式只会将数据解释为普通文本，因此如果想解释HTML代码则需要使用 **v-html** 指令

```html
<p v-html="msg"></p>
```

##### 3.特性

双大括号语法不能作用在HTML特性 (标签属性)上，因此如果需要对标签属性操作，应该使用 **v-bind** 指令

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>操作html标签属性的指令v-bind</title>
	</head>
	<body>
		<div id="app">
			<!-- ischecked 数据绑定到checked属性上 -->
			<input type="checkbox" v-bind:checked="ischecked">
		</div>
		<script src="https://cdn.bootcss.com/vue/2.6.6/vue.min.js"></script>
		<script>
			const app = new Vue({
				el: '#app',
				data: {
					ischecked: false
				}
			})
		</script>
	</body>
</html>
```

 结果为一个没有选中的复选框：![](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1573037066455.png)

##### 4. javascript 表达式

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>JS表达式</title>
	</head>
	<body>
		<!-- JS表达式 -->
		<div id="app">
			<!-- 运算符 -->
			<p>num + 24 = {{num + 24}}</p>
			<!-- 三元表达式 -->
			<p>Are you OK? {{ok ? 'i am OK!' : 'no'}}</p>
			<!-- 对象方法直接调用 -->
			<p>我的名字： {{name}}</p>
			<p>名字倒转： {{name.split(' ').reverse().join(' ')}}</p>
			<!-- 属性值运算 -->
			<p v-bind:class="'col' + colNum">Acmen5</p>
		</div>
		<script src="https://cdn.bootcss.com/vue/2.6.6/vue.min.js"></script>
		<script>
			const app = new Vue({
				el: "#app",
				data: {
					num: 10,
					ok: true,
					name: 'Acmen five',
					colNum: 12
				}
			})
		</script>
	</body>
</html>

```

结果：

![](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1573038414014.png)

#### 三、指令

指令（Directives）是带有 **v-** 前缀的特殊特性 

##### 1. 参数 

某些指令能够接受一个**参数** ，在指令名称后 以冒号表示，例如



**v-bind** 指令可以响应式的更新HTML特性，在这里 **href** 是参数，

告知 **v-bind** 指令将该元素的 **href** 特性与表达式 **url** 的值绑定

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>Vue指令</title>
	</head>
	<body>
		<div id="app">
			<!-- v-bind将html标签的href属性和数据选项中url的值绑定 -->
			<a v-bind:href="url">我的github主页</a>
		</div>
		<script src="https://cdn.bootcss.com/vue/2.6.6/vue.min.js"></script>
		<script>
			const app = new Vue({
				el: '#app',
				data: {
					url: 'https://github.com/Acmen5'
				}
			})
		</script>
	</body>
</html>
```

**v-on** 指令，用于监听 DOM 事件

**v-on** 指令将元素的DOM中的**click操作**与methods方法选项中**handleclick的值**进行绑定

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>Vue指令 V-on</title>
	</head>
	<body>
		<div id="app">
			<p>我叫: {{name}}</p>
			<!-- v-on指令将DOM中的 click 指令与methods方法选项中 handleClick的值 绑定 -->
			<button v-on:click="handleClick">点击</button>
		</div>
		<script src="https://cdn.bootcss.com/vue/2.6.6/vue.min.js"></script>
		<script>
			const app = new Vue({
				el: '#app',
				data: {
					name: 'Acmen Five'
				},
				methods: {
					handleClick:function() {
						this.name = this.name.split(' ').reverse().join(' ')
					}
				}
			})
		</script>
	</body>
</html>
```

##### 2. 动态参数

上面的属性(href)或者事件(click)都是固定写死的，因此叫静态参数，但是在Vue中参数也可是动态的

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>Vue指令 V-on</title>
	</head>
	<body>
		<div id="app">
			<p>我叫: {{name}}</p>
			<!-- v-on指令将DOM中的[event]操作与method方法选项中handleClick的值绑定 -->
			<button v-on:[event]="handleClick">点击</button>
		</div>
		<script src="https://cdn.bootcss.com/vue/2.6.6/vue.min.js"></script>
		<script>
			const app = new Vue({
				el: '#app',
				data: {
					event: 'click',
					name: 'Acmen Five'
				},
				methods: {
					handleClick:function() {
						this.name = this.name.split(' ').reverse().join(' ')
					}
				}
			})
		</script>
	</body>
</html>
```

此例结果与上一个例子的结果相同

##### 3.修饰符

修饰符是以半角句号 `.` 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定

大致分为三类:

- 事件修饰符
- 按键修饰符
- 系统修饰符

例如： 事件修饰符中的 ` .prevent ` 修饰符和原生 event.preventDefault() 效果一样，可以阻止事件默认行为，在表单中点击提交按钮，就会发生页面跳转，但是使用了 ` .prevent` 就不会发生跳转，

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>Vue指令修饰符</title>
	</head>
	<body>
		<div id="app">
			<form action="/" v-on:submit.prevent="submit">
				<button type="submit">提交</button>
			</form>
		</div>
		<script src="https://cdn.bootcss.com/vue/2.6.6/vue.min.js"></script>
		<script type="text/javascript">
			const app = new Vue({
				el: '#app',
				data: {
					
				},
				methods: {
					submit: function() {
						console.log('提交成功！')
					}
				}
			})
		</script>
	</body>
</html>

```

#### 四、指令缩写

顾名思义，也就是简化指令的格式

**v-bind**： 

v-bind:href 的简写为  :href

**v-on** :

v-on:click 的简写为   @click