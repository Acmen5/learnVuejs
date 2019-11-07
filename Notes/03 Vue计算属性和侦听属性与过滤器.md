### 计算属性、侦听属性与过滤器

之前接触到了 Vue 实例选项中的 el、 data、 methods 这三个属性

接下来学习 Vue 实例的其他属性

#### 一、计算属性

> 应用场景：一个数据需要通过其他数据计算而来，例如 ：购物车、平常开发数据与数据关联计算等场景中

> 解决办法：
>
> 使用 Vue 实例中的计算属性即可，像绑定普通属性一样在模板中绑定计算属性，可以直接使用{{}}向页面输出

##### 1.计算属性的基本使用

实例的 **computed** 选项中定义你的计算属性，直接使用Mustache语法向页面输出

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>Vue计算属性的基本使用</title>
	</head>
	<body>
		<div id="app">
			<p>我的名字: {{name}}</p>
			<!-- 计算属性和普通属性的使用方法相同 -->
			<p>通过计算属性翻转我的名字: {{reverseName}}</p>
		</div>
		<script src="https://cdn.bootcss.com/vue/2.6.6/vue.min.js"></script>
		<script type="text/javascript">
			const app = new Vue({
				el: '#app',
				data: {
					name: 'Acmen Five',
				},
				computed:{
					// reverseName是一个计算属性
					reverseName:function() {
						return this.name.split('').reverse().join('');
					}
				}
			})
		</script>
	</body>
</html>
```

**注意** ：在 Vue 中计算属性是 **惰性的**，只有当依赖数据发生改变时，才会触发计算，否则它的值是上一次触发计算的缓存值

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>Vue计算属性的基本使用</title>
	</head>
	<body>
		<div id="app">
			<p>我的名字: {{name}}</p>
			<!-- 计算属性和普通属性的使用方法相同 -->
			<p>通过计算属性翻转我的名字: {{reverseName}}</p>
			<p>当前时间为： {{now}}</p>
		</div>
		<script src="https://cdn.bootcss.com/vue/2.6.6/vue.min.js"></script>
		<script type="text/javascript">
			const app = new Vue({
				el: '#app',
				data: {
					name: 'Acmen Five',
				},
				computed:{
					// reverseName是一个计算属性
					reverseName:function() {
						return this.name.split('').reverse().join('');
					},
					now: function() {
						return Date.now()
					}
				}
			})
		</script>
	</body>
</html>
```

上述例子中： 界面中的时间并不会自动更新的原因是因为我们**定义的now**并**没有**和**实例中数据**建立**相应式依赖**，只是依赖 Data 对象获取系统时间，它只会计算一次，然后将值缓存。若想改变只有刷新页面才能实现

##### 2.计算属性的 setter 和 getter

在计算属性的基本使用中，仅使用到计算属性的getter属性，通过其他数据计算而得，但我们也可以**直接赋值**，通过计算属性来改变依赖数据的值

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>计算属性的getter和setter属性</title>
	</head>
	<body>
		<div id="app">
			<p>firstName: {{firstName}}</p>
			<p>lastName: {{lastName}}</p>
			<p>fullName: {{fullName}}</p>
			<!-- 1. 首先通过点击触发click事件处理函数 -->
			<button @click="changeName">改变名字</button>
		</div>
		<script src="https://cdn.bootcss.com/vue/2.6.6/vue.min.js"></script>
		<script type="text/javascript">
			const app = new Vue({
				el: '#app',
				data: {
					firstName: '王',
					lastName: '花花'
				},
				methods: {
					// 2. 执行事件处理函数,改变了计算属性(fullName)的值,触发了fullName的setter
					// 直接改变计算属性 fullName的值就可以触发setter
					changeName() {
						return this.fullName = '李花花'
					}
				},
				computed: {
					fullName:{
						get() {
							return this.firstName + this.lastName;
						},
						// 3.执行setter
						set(newName) {
							let name = newName.slice(0)
							this.firstName = name.slice(0,1)
							this.lastName = name.slice(1)
						}
					}
				}
			})
		</script>
	</body>
</html>
```

> 如何触发计算属性的setter？
>
> 只需要直接改变计算属性的值就可以触发计算属性的setter了
>
> 上面的例子中 通过直接修改fullName的值this.fullName = xx,这步操作就出发了计算属性的setter

##### 3. 侦听属性

在开发中，Vue 提供了一个通用的方式来侦听属性变化，就是通过 Vue 实例中的 **watch** 属性确定监听项



```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>Vue侦听属性 watch</title>
	</head>
	<body>
		<div id="app">
			<p>{{msg}}</p>
			<button @click="handleClick('hello Acmen5')">改变</button>
		</div>
		<script src="https://cdn.bootcss.com/vue/2.6.6/vue.min.js"></script>
		<script>
			const app = new Vue({
				el: '#app',
				data: {
					msg: 'hello world!'
				},
				methods: {
					handleClick(val) {
						return this.msg = val 
					}
				},     
				watch: {
             //监听新旧值  监听属性有两个参数，第一个新值，第二个旧值
					msg(newVal,oldVal) {
						console.log('新的val= ' + newVal,'旧的val= '+ oldVal)
					}
				}
			})
		</script>
	</body>
</html>
```

>这个例子中通过按钮点击改变 msg 的值，并且监听新旧值改变，并输出新旧值。

计算属性与侦听属性对比

计算属性和侦听属性两者在很多场景都是共通的，都可以是想相同的需求

下面使用侦听属性来改写计算属性的例子

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>计算属性的getter和setter属性</title>
	</head>
	<body>
		<div id="app">
			<p>firstName: {{firstName}}</p>
			<p>lastName: {{lastName}}</p>
			<p>fullName: {{fullName}}</p>
			<button @click="changeName">改变名字</button>
		</div>
		<script src="https://cdn.bootcss.com/vue/2.6.6/vue.min.js"></script>
		<script type="text/javascript">
			const app = new Vue({
				el: '#app',
				data: {
					firstName: '王',
					lastName: '花花',
           	  fullName: '王花花'
				},
				methods: {
					changeName() {
						return this.fullName = '李花花'
					}
				},
           watch: {
               // 侦听属性有两个参数，第一个是新值，第二个是旧值
               // 通过methods中的changeName方法，改变了fullName的值
               // 此时watch侦听到fullName的值发生了改变，于是将新值传到watch的第一个参数中
               fullName(newVal) {
                   let name = newVal
                   this.firstName = name.slice(0,1)
                   this.lastName = name.slice(1)
               }
           }
				
			})
		</script>
	</body>
</html>
```

#### 三、过滤器

计算属性和侦听属性一般不用于处理数据过滤

Vue中专门处理数据过滤的东西： **过滤器**

过滤器可以用在两个地方

1. 双大括号插值
2. V-bind 表达式

```html
<p>{{msg2|getString}}</p>
<p v-bind:class="msg2|getString"></p>
```

##### 1. 过滤器使用方法

在双花括号插值和 v-bind 表达式中把需要过滤的数据用 `|` 与过滤器分割

（data | filter）

例如：

使用 filters 过滤器实现**大写转换**和**自动去除字符串中的数字**

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>Vue过滤器的使用</title>
	</head>
	<body>
		<div id="app">
			<p>小写转大写: 过滤前{{msg}}------过滤后{{msg|toUpperCase}}</p>
			<p>去除字符串中的数字: 过滤前{{msg2}}-----过滤后{{msg2|getString}}</p>
		</div>
		<script src="https://cdn.bootcss.com/vue/2.6.6/vue.min.js"></script>
		<script>
			const app = new Vue({
				el: '#app',
				data: {
					msg: 'hello',
					msg2: 'Acmen5'
				},
				filters: {
					// 过滤器自动将信息作为参数传入到过滤器函数中
					toUpperCase(val) {
						return val.toUpperCase()
					},
					getString(val) {
						let newVal = ''
						val.split('').map(function(current) {
							if(current>=0 && current<=9){
								return
							}else{
								return newVal+=current
							}
						})
						return newVal
					}
				}
			})
		</script>
	</body>
</html>

```

##### 2.过滤器应用场景

应用比较多的商品价格过滤、表单数据过滤等

后台数据一般是

```json
{
    courseName:'xxx',
    price:199,
    coupon:8
}
```

数据中没有￥，但开发中不建议直接操作数据源，因此需要使用过滤器,在视图层面实现了效果

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>Vue过滤器的应用</title>
	</head>
	<body>
		<div id="app">
			<p>不要<s>￥899</s>，只要{{price|joint}}</p>
		</div>
		<script src="https://cdn.bootcss.com/vue/2.6.6/vue.min.js"></script>
		<script>
			const app = new Vue({
				el: '#app',
				data: {
					//后台价格数据
					price: 199
				},
				filters: {
					// hoint 定义一个字符串转大写的过滤器
					joint(price) {
						return '￥'+price
					}
				}
			})
		</script>
	</body>
</html>
```

结果为：![](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1573116848117.png)

#### 四、计算属性和过滤器的小练习

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>Vue计算属性和过滤器的小练习</title>
		<style>
			* {
				margin: 0;
				padding: 0;
			}
			.price {
				font-size: 22px;
				color: brown;
			}
		</style>
	</head>
	<body>
		<!-- 实现一个简易的购物车 -->
		<div id="app">
			<!-- joint 为自定义的过滤器 -->
			<p>单价<span class="price">{{price}}</span></p>
			数量: <input type="number" v-model="goodsNum">
			<p>总价: <span class="price">{{totalPrice|joint}}</span></p>
		</div>
		<script src="https://labfile.oss.aliyuncs.com/courses/1262/vue.min.js"></script>
		<script>
			const app = new Vue({
				el: '#app',
				data: {
					goodsNum: 0,
					price: 199
				},
				computed: {
					totalPrice() {
						return this.price*this.goodsNum
					}
				},
				// filter 过滤器选项
				filters: {
					// joint 定义￥拼接过滤器
					joint(val) {
						return '￥'+val
					}
				}
			})
		</script>
	</body>
</html>
```

