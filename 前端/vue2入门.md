视频：https://www.bilibili.com/video/BV1zq4y1p7ga?p=2&spm_id_from=pageDriver&vd_source=077aacef4f0d3f612de03e3448b09fb4、

整理文档：https://blog.csdn.net/weixin_44772326/article/details/121628617

```text
必备前端知识:
HTML+CSS+JavaScript
WebAPI ( DOM + BOM )
Ajax
NodeJs


```

# 1 前端工程化和webpack
```text
一、前端工程化
1. 现代化前端编程
模块化（js的模块化、CSS的模块化、资源的模块化）
组件化（复用现有的UI结构、样式、行为）
规范化（目录结构的划分、编码规范化、接口规范化、文档规范化、Git分支管理）
自动化（自动化构建、自动部署、自动化测试）

2. 前端工程化
是指：在企业级的前端项目开发中，把前端开发所需的工具、技术、流程、经验等进行规范化、标准化。
企业中的Vue项目和React项目都是基于工程化的方式进行开发的。

优点：前端开发自成体系，有一套标准的开发方案和流程。

3. 前端工程化的解决方案
目前主流的前端工程化解决方案：
webpack
parcel
```

```text
二、webpack的基本使用

1. 什么是webpack
概念：webpack是前端项目工程化的具体解决方案。

主要功能：提供了友好的前端模块化开发支持，以及代码压缩混淆（减小体积）、处理浏览器端JavaScript的兼容性（将高级的代码转换成低级的没有兼容问题的代码，有对应不同版本的解决方案）、性能优化等强大的功能。

优点：让程序员把工作的重心放在具体功能的实现上，提高了前端开发效率和项目的可维护性。

目前Vue, React等前端项目，基本上都是基于webpack进行工程化开发的。

2. 实践
初始化包管理配置文件package.json
npm init -y

安装jQuery
npm install jquery -S
-S等价于--save

ES6模块化的方式导入jquery
import $ from 'jquery'

3. 在项目中安装webpack
npm install webpack@5.42.1 webpack-cli@4.7.2 -D

4. 在项目中配置webpack
在项目根目录中，创建名为webpack.config.js的webpack配置文件，并初始化如下的基本配置：

//使用node.js中的导出语法，向外导出一个webpack的配置对象
module.exports = {    
	// 代表webpack的运行模式，有 development 和 production
	mode: 'development'
}

production模式（需要重新运行脚本）会把文件压缩（webpack的功能之一） 。体积减小，打包时间增长

开发阶段使用development 追求打包速度快；
发布上线阶段使用production 追求打包体积小

在package.json的script节点下，新增dev脚本如下： 
"scripts": {
	"dev": "webpack"
},

script节点下的脚本，可以通过 npm run 执行 例如: npm run dev
此时真正执行的命令是 "webpack"
在运行webpack之前，会先读取根目录下webpack.config.js这个配置文件，拿到配置文件中向外导出的配置选项mode

在终端中运行 npm run dev 命令，启动webpack进行项目的打包构建。

mode的可选值
development
开发环境
不会对打包生成的文件进行代码压缩和性能优化
打包速度快，适合在开发阶段使用

production 
生产环境
会对打包生成的文件进行代码压缩和性能优化
打包速度很慢，仅适合在项目发布阶段使用

webpack.config.js文件的作用
是webpack的配置文件。webpack在真正开始打包构建之前，会读取这个配置文件（拿到向外导出的配置选项），从而基于给定的配置，对项目进行打包。

注意：由于webpack是基于node.js开发出来的打包工具，因此在它的配置文件中，支持使用node.js相关的语法和模块进行webpack的个性化配置。

webpack中的默认约定
在webpack 4.x和5.x的版本中有如下默认约定：
（1）默认的入口打包文件为 src -> index.js （没有会报错）
（2）默认的输出文件路径为 dist -> main.js
注意：可以在webpack.config.js中修改打包的默认约定

自定义打包的入口和出口
在webpack.config.js配置文件中，通过entry节点指定打包的入口。通过output节点指定打包的出口。示例代码：
const path=require('path')//导入node.js中专门操作路径的模块
module.exports = {
    entry:path.join(__dirname,'./src/index.js'),//打包入口文件的路径
    output:{
        path:path.join(__dirname,'./dist'),//输出文件的存放路径
        filename:'bundle.js'//输出文件的名称
    }
}
__dirname代表当前文件的存放路径

记得index.html中引用的js文件路径也要跟着改变
<!-- <script src="./index.js"></script> -->
<!-- <script src="../dist/main.js"></script> -->
<script src="../dist/bundle.js"></script>

每次修改index.js需要重新npm run dev进行打包，页面才能展示修改后的内容，很麻烦，下面使用插件来解决。
```

```text
三、webpack中的插件

1. webpack插件的作用
通过安装和配置第三方的插件，可以拓展webpack的能力，从而使webpack用起来更方便。最常用的webpack插件有如下两个：
（1）webpack-dev-server
类似node.js阶段用到的nodemon工具
每当修改了源代码，webpack会自动进行项目的打包和构建

（2）html-webpack-plugin
webpack中的HTML插件（类似于一个模板引擎插件）

可以通过此插件自定制index.html页面中的内容

2. 安装配置使用webpack-dev-server
安装webpack-dev-server
npm install webpack-dev-server@3.11.2 -D
该包会进入devDependencies节点中

配置webpack-dev-server
（1）修改package.json -> scripts 中的dev命令如下
"scripts": {
	"dev": "webpack serve"
},
（2）再次运行npm run dev命令，重新进行项目的打包

webpack-dev-server没有将输出文件放在物理磁盘上，而是放到了内存中，所以资源管理器中看不到，但是可以通过路径访问到

3. 安装配置使用html-webpack-plugin
安装html-webpack-plugin
npm install html-webpack-plugin@5.3.2 -D

配置html-webpack-plugin
/**
 * webpack.config.js
 */
// 1、导入html-webpack-plugin这个插件，得到插件的构造函数
const HtmlPlugin = require('html-webpack-plugin')
 
// 2、new构造函数，创建插件的实例对象
const htmlPlugin = new HtmlPlugin({
    // 指定要复制哪个文件
    template: './src/index.html',
    // 指定复制出来的文件名和存放路径
    filename: './index.html'
})
 
module.exports = {
    // 代表webpack的运行模式，有 development 和 production
    mode: 'development',
    // 3、插件的数组，将来webpack运行时会加载并调用这些插件
    plugins:[htmlPlugin]
}
再 npm run dev

解释html-webpack-plugin
（1）通过HTML插件复制到项目根目录中的index.html页面，也被放到了内存中
（2）HTML插件在生成的index.html页面，自动注入了打包的bundle.js文件（即不需要手动在HTML文件里引入bundle.js）

devServer节点
在webpack.config.js配置文件中，可以通过devServer节点对webpack-dev-server插件进行更多的配置，示例代码：
/*
*webpack.config.js中的module.exporte
*/
devServer:{
	// 初次打包完成后,自动打开浏览器
	open:true,
	// 在http协议中,如果端口号是80,可以被省略(仅显示为localhost)
	port:80,
	// 指定运行的主机地址
	host:'127.0.0.1'
}


```

```text
四、webpack中的loader
1. loader概述
在实际开发中，webpack默认只能打包处理以.js后缀名结尾的模块。其他非.js后缀名结尾的模块，webpack默认处理不了，需要调用loader加载器才可以正常打包，否则会报错！

loader加载器的作用：协助webpack打包处理特定的文件模块。比如：
css-loader可以打包处理.css相关的文件
less-loader可以打包处理.less相关的文件
babel-loader可以打包处理webpack无法处理的高级js语法

2. loader调用的过程[image]

3. 打包处理CSS文件
在src下新建css文件夹，css下新建index.css
/* index.css */
li {
    list-style: none;
}

在index.js中导入index.css
// 导入样式(在webpack中,一切皆模块都可以通过ES6导入语法进行导入和使用)
import './css/index.css'

注意：module和 mode 、entry、output、plugins、devServer平级
注意：style-loader和css-loader顺序不能写反，因为loader在调用的时候是从后往前调的。webpack把index.css这个自己不能处理的文件，先转交给最后一个个loader（css-loader）处理，处理完后转交给前面一个loader（style-loader），直至前面没有loader了，就转交给webpack，weebpack把结果合并到 /dist/bundle.js中，最终生成打包好的文件

当webpack发现某个文件处理不了的时候，会查找webpack.config.js这个配置文件，看module.rules数组中，是否配置了对应的loader加载器。

4. 打包处理less文件
安装的less是配置依赖项，即less-loader依赖的，并没有在rules中使用
// index.less
html, body, ul {
    margin: 0;
    padding: 0;
    li {
        line-height: 30px;
        padding-left: 20px;
        font-size: 12px;
    }
}

5. 打包处理样式表中与url路径相关的文件
base24
使用相对路径和base24路径都能显示相同的小图片。使用base24可以防止浏览器发起不必要的请求（标签一次请求，logo.jpg又是一次请求），缺点是会稍微增加体积。所以一般图片用base24

webpack处理样式的过程： 
在webpack中一切皆模块，通过import 加载过来的样式文件会被webpack转换成类似js的形式运行。如果某个模块中，接收到的成员为undefined，则没有必要进行接收，即“import xx from xx”和“import xx”的区别。

6. 打包处理js文件中的高级语法
安装babel-loader相关的包

```
![[Pasted image 20220617212026.png]]

```text
五、打包发布
发布上线：前端将写好的项目文件打包（dist）发给后端，后端部署上线
现在需要解决的问题是：把页面（根目录下的index.html和bundle.js）生成到实际的物理磁盘上

通过相应的配置：
把JavaScript文件统一生成到js目录中
把图片文件统一生成到image目录中
自动清理dist目录下的旧文件

```

```text
六、Source Map
1. 什么是Source Map
Source Map是一个信息文件，里面存储着位置信息。即Source Map存储着压缩混淆后的代码，所对应的转换前的位置。有了它，出错的时候，除错工具将直接显示原始代码，而不是转换后的代码，能极大的方便后期的调试

eg. 控制台的报错行是bundle.js打包后的行数，要转为原始index.js的行数

2. webpack开发环境下的Source Map
开发环境下，webpack默认启用了Source Map功能。当程序运行出错时，可以直接在控制台提示错误行的位置，并定位到具体的源代码

默认Source Map的问题 
开发环境下生成的Source Map ，记录的是生成后代码的位置，会导致运行时报错的行数和源代码行数不一致的问题

webpack生产环境下的Source Map
在生产环境下，如果省略了devtool选项，则最终生成的文件中不包含Source Map，这能够防止源代码通过Source Map的形式暴露给别有所图之人

Source Map的最佳实践
```

```text
总结：
以上，介绍了webpack的一些配置，可以看到是比较复杂容易出错的，在实际开发中往往借助命令行工具（俗称CLI）一键生成带有webpack的项目，不需要自己配置，开箱即用，所有的webpack配置项都是现成的。但以上实操并不是无用功，了解一些基础原理有助于更好地理解项目打包，以及遇到一些报错时能够知道原因。

在import导入文件的时候建议使用 @ 表示源代码目录，从外往里查找 不要使用../从里往外查找。但在使用前需要配置webpack：
/**
 * webpack.config.js 与module平齐
 */
resolve: {
	alias: {
		// 告诉webpack，程序员写的代码中，@符号表示src这一层目录
		'@': path.join(__dirname, './src/')
	}
}
这样可以用 import msg from '@/msg.js' 代替import msg from '../../msg.js'

在实际生产中其实往往并不需要手动配置webpack，可以借助脚手架或第三方前端应用框架开箱即用配置好的webpack。但通过简单了解webpack的一些概念以及常见配置，有助于更好地理解项目开发过程，以及有需求需要更改配置时知道如何下手。
```

# 2 vue2 基础
```text
一、Vue简介
1. 什么是Vue
是一套用于构建（往页面填数据）用户界面（用户看得到的）的前端框架（现成的解决方案，程序员需要遵守规范编写自己的业务功能）

学习Vue就是学习Vue框架中规定的用法，例如：vue指令、组件（是对UI结构的复用）、路由、Vuex、Vue组件库

2. Vue的特性
vue框架的特性，主要体现在如下两个方面：
（1）数据驱动视图
（2）双向数据绑定
数据：AJAX从服务器请求回来的东西

数据驱动视图 
数据的变化会驱动视图自动更新
程序员只需将数据维护好，页面结构会被Vue自动渲染出来

双向数据绑定
表单负责采集数据，AJAX负责提交数据

js数据的变化，会被自动渲染到页面上
页面上表单采集的数据发生变化时，会被vue自动获取到，并更新到js数据中

MVVM
Model数据源，View视图，ViewModel就是Vue的实例

MVVM的工作原理

3. Vue的版本
```
![[Pasted image 20220618093407.png]]
![[Pasted image 20220618093445.png]]
![[Pasted image 20220618093549.png]]
![[Pasted image 20220618093655.png]]

```text
二、Vue的基本使用
<body>
  <!-- 希望 Vue 能够控制下面的这个 div，帮我们在把数据填充到 div 内部 -->
  <div id="app">{{ username }}</div>
 
  <!-- 1. 导入 Vue 的库文件，在 window 全局就有了 Vue 这个构造函数 -->
  <script src="./lib/vue-2.6.12.js"></script>
  <!-- 2. 创建 Vue 的实例对象 -->
  <script>
    // 创建 Vue 的实例对象
    const vm = new Vue({
      // el 属性是固定的写法，表示当前 vm 实例要控制页面上的哪个区域，接收的值是一个选择器
      el: '#app',
      // data 对象就是要渲染到页面上的数据
      data: {
        username: 'zhangsan'
      }
    })
  </script>
</body>

基本代码与MVVM的对应关系

```
![[Pasted image 20220618093835.png]]

```text
三、Vue的调试工具

安装与配置vue-devtools调试工具

使用vue-devtools调试Vue页面
```

```text
四、vue的指令与过滤器
1. 指令
指令（Directives）是vue为开发者提供的模板语法，用于辅助开发者渲染页面的基本结构。

Vue中的指令按照不同的用途可以分为如下6大类：
（1）内容渲染指令

（2）属性绑定指令

（3）事件绑定指令

（4）双向绑定指令

（5）条件渲染指令

（6）列表渲染指令

指令用于渲染数据
1.1 内容渲染指令
内容渲染指令用来辅助开发者渲染DOM元素的文本内容，常用的内容渲染指令有以下3个：
（1）v-text
用法实例（作为属性来使用）
缺点：会覆盖元素内部原有的内容

（2）{{ }}
在实际开发中用的最多，只是内容的占位符，不会覆盖原有的内容
只能写简单的js表达式，不能写if等复杂的js语句

（3）v-html

总结：
el属性值只能是一个标签元素，如果存在多个同名标签，只会控制第一个。官方推荐的做法是在所有希望被控的元素外面包裹一个id为app的div，让Vue控制这个div，里面的元素也会被控制。但在实际开发中，往往不需要自己指定控制元素，Vue项目会自动配好。

1.2 属性绑定指令
注意：插值表达式只能用在元素的内容节点，不能用在属性节点
v-bind
<div id="app">
    <input type="text" :placeholder="tips">
    <hr>
    <!-- vue 规定 v-bind: 指令可以简写为 : -->
    <img :src="photo" alt="" style="width: 150px;">
  </div>
 
  <!-- 1. 导入 Vue 的库文件，在 window 全局就有了 Vue 这个构造函数 -->
  <script src="./lib/vue-2.6.12.js"></script>
  <!-- 2. 创建 Vue 的实例对象 -->
  <script>
    // 创建 Vue 的实例对象
    const vm = new Vue({
      // el 属性是固定的写法，表示当前 vm 实例要控制页面上的哪个区域，接收的值是一个选择器
      el: '#app',
      // data 对象就是要渲染到页面上的数据
      data: {
        tips: '请输入用户名',
        photo: 'https://cn.vuejs.org/images/logo.svg',
      }
    })
  </script>
</div>

使用JavaScript表达式
在使用v-bind属性绑定期间，如果绑定内容需要进行动态拼接，则字符串的外面应该包裹单引号。如果没有单引号，不会被认为是字符串而会被认为是变量，数据源中无此变量则会报错。

1.3 事件绑定指令
v-on
<div id="app">
    <p>count 的值是：{{ count }}</p>
    <!-- 在绑定事件处理函数的时候，可以使用 () 传递参数 -->
    <!-- v-on: 指令可以被简写为 @ -->
    <button v-on:click="add(1)">+1</button>
    <button v-on:click="sub">-1</button>
  </div>
 
  <!-- 1. 导入 Vue 的库文件，在 window 全局就有了 Vue 这个构造函数 -->
  <script src="./lib/vue-2.6.12.js"></script>
  <!-- 2. 创建 Vue 的实例对象 -->
  <script>
    // 创建 Vue 的实例对象
    const vm = new Vue({
      // el 属性是固定的写法，表示当前 vm 实例要控制页面上的哪个区域，接收的值是一个选择器
      el: '#app',
      // data 对象就是要渲染到页面上的数据
      data: {
        count: 0
      },
      // methods 的作用，就是定义事件的处理函数
      methods: {
        add(n) {
          // 在 methods 处理函数中，this 就是 new 出来的 vm 实例对象
          // console.log(vm === this)
          //console.log(vm)
          // vm.count += 1
          this.count += n
        },
        sub() {
          // console.log('触发了 sub 处理函数')
          this.count -= 1
        }
      }
    })
  </script>
</div>

给谁绑定事件就给谁加v-on，后面跟事件（如click）和处理函数（如add）。如果传参就在定义处理函数小括号里加形参，绑定事件小括号里加实参。

通过this. 调用数据源中的变量与方法。

v-on:简写为@

如果不传实参，则默认传一个事件对象给处理函数。
<div id="app">
    <p>count 的值是：{{ count }}</p>
    <!-- 如果 count 是偶数，则 按钮背景变成红色，否则，取消背景颜色 -->
    <button @click="add">+N</button>
  </div>
 
  <!-- 1. 导入 Vue 的库文件，在 window 全局就有了 Vue 这个构造函数 -->
  <script src="./lib/vue-2.6.12.js"></script>
  <!-- 2. 创建 Vue 的实例对象 -->
  <script>
    // 创建 Vue 的实例对象
    const vm = new Vue({
      // el 属性是固定的写法，表示当前 vm 实例要控制页面上的哪个区域，接收的值是一个选择器
      el: '#app',
      // data 对象就是要渲染到页面上的数据
      data: {
        count: 0
      },
      methods: {
        add(e, n) {
          this.count += n
          console.log(e) //打印出来的是一个MouseEvent对象
 
          // 判断 this.count 的值是否为偶数
          if (this.count % 2 === 0) {
            // 偶数
            e.target.style.backgroundColor = 'red'
          } else {
            // 奇数
            e.target.style.backgroundColor = ''
          }
        }
      },
    })
  </script>
</div>

如果传了实参，则该事件对象会被覆盖，也就不存在target属性。
vue 提供了内置变量，名字叫做 $event，它就是原生 DOM 的事件对象 e，可以用于传实参。
<button @click="add($event, 1)">+n</button>
就可以获得事件对象的target了。
实际开发中用的较少。

事件修饰符
原生js中常用事件对象的preventDefault()来阻止默认行为，stopPropagation()来阻止事件冒泡

Vue的简写：在绑定事件的同时加给事件加事件修饰符。语法格式如下：
<div id="app">
    <a href="http://www.baidu.com" @click.prevent="show">跳转到百度首页</a>
 
    <hr>
 
    <div style="height: 150px; background-color: orange; padding-left: 100px; line-height: 150px;" @click="divHandler">
      <button @click.stop="btnHandler">按钮</button>
    </div>
</div>

按键修饰符
clearInput(e) {
  console.log('触发了 clearInput 方法')
  e.target.value = ''
},
commitAjax() {
  console.log('触发了 commitAjax 方法')
}

1.4 双向绑定指令
在提交表单或采集数据时，不需要再操作DOM，只要修改了表单文本框，就会修改数据源，直接用this. 就能获得最新的数据源。

v-bind: value="username" 与 v-model="username"的区别：前者是单向绑定，数据源改变驱动页面数据改变，但页面上的改动不会同步到数据源；后者是双向绑定。

只有表单元素使用v-model才有意义。如
（1）input: type="radio"; type="checkbox"; type="xxx"
（2）texytarea
（3）select

vue内部会判断v-model绑定的元素类型，有选择地控制元素的属性（例如input type="checkbox"绑定check属性，input type="text"绑定value属性）
<div id="app">
  <select v-model="city">
      <option value="">请选择城市</option>
      <option value="1">北京</option>
      <option value="2">上海</option>
      <option value="3">广州</option>
  </select>
</div>
<script src="./lib/vue-2.6.12.js"></script>
<!-- 2. 创建 Vue 的实例对象 -->
<script>
// 创建 Vue 的实例对象
const vm = new Vue({
  // el 属性是固定的写法，表示当前 vm 实例要控制页面上的哪个区域，接收的值是一个选择器
  el: '#app',
  // data 对象就是要渲染到页面上的数据
  data: {
	username: 'zhangsan',
	city: '2'
  }
})
</script>

v-model指令的修饰符
<input type="text" v-model.number="n1" />

（1）.number
用户输入表单的是字符串，不能进行数值的计算（只能字符串拼接），可以用.number转换成数值类型再计算
（2）.trim
在例如输入用户名的场景时我们希望去掉用户可能因为误操作在首尾输入的空格，可以使用.trim，它不会处理输入内容中间的空格。
（3）.lazy
因为使用v-model时表单输入框和数据源中的数据是实时更新的，在改变输入框内的内容过程中没有必要更新数据源中的数据，会引起性能上的问题，所以可以使用.lazy，在表单输入内容修改完成（"change"时，如失去焦点）再同步数据源的数据。

<div id="app">
    <input type="text" v-model.number="n1"> + <input type="text" v-model.number="n2"> = <span>{{ n1 + n2 }}</span>
    <hr>
    <input type="text" v-model.trim="username">
    <button @click="showName">获取用户名</button>
    <hr>
    <input type="text" v-model.lazy="username">
  </div>
 
  <!-- 1. 导入 Vue 的库文件，在 window 全局就有了 Vue 这个构造函数 -->
  <script src="./lib/vue-2.6.12.js"></script>
  <!-- 2. 创建 Vue 的实例对象 -->
  <script>
    // 创建 Vue 的实例对象
    const vm = new Vue({
      // el 属性是固定的写法，表示当前 vm 实例要控制页面上的哪个区域，接收的值是一个选择器
      el: '#app',
      // data 对象就是要渲染到页面上的数据
      data: {
        username: 'zhangsan',
        n1: 1,
        n2: 2
      },
      methods: {
        showName() {
          console.log(`用户名是："${this.username}"`)
        //``是模板字符串，可以括住多行字符串
        //${}是占位符，将数量较多的变量插入字符串的写法，简化 "xxx"+变量+"xxx" 的写法
        }
      },
    })
  </script>
</div>

1.5 条件渲染指令
用来辅助开发者按需控制DOM的显示与隐藏。有2个：
v-if
v-show

区别：v-if是通过动态添加或移除元素标签达到显示或隐藏的功能，而v-show是给元素标签添加或删除style="display: none" 样式属性达到。

可以看出频繁切换元素显示状态，v-show对性能优化更好；如果刚进入页面时某些元素默认不需要被展示，而且以后也很可能不需要被展示，v-if对性能更好。

实际开发中v-if用的更多，因为性能影响不是很大。

使用脚手架vue-cli时会先将文件编译成js（包括html）再运行，即先执行js逻辑再渲染标签，所以v-if并不是在html时就会被创建后再在script里被false掉。

v-else（v-if配套）
v-else-if（v-if, v-else配套）
必须配合v-if一起使用，否则不识别。

1.6 列表渲染指令
v-for

v-for中的索引
<tr v-for="(item, index) in list" :key="item.id">
  <td>{{ index }}</td>
  <td>{{ item.id }}</td>
  <td>{{ item.name }}</td>
</tr>

官方建议：只要用到了 v-for 指令，那么一定要绑定一个 :key 属性，而且，尽量把 id 作为 key 的值，官方对 key 的值类型，是有要求的：字符串或数字类型，key 的值是千万不能重复的，否则会终端报错：Duplicate keys detected


```
![[Pasted image 20220618094125.png]]
![[Pasted image 20220618094210.png]]
![[Pasted image 20220618094254.png]]
![[Pasted image 20220618094932.png]]
![[Pasted image 20220618095020.png]]
![[Pasted image 20220618095321.png]]
![[Pasted image 20220618095414.png]]
![[Pasted image 20220618095500.png]]
![[Pasted image 20220618095647.png]]

```text
2. 过滤器
（Vue3中已砍）

定义过滤器
capitalize是自定义的首字母大写的函数，message作为参数传给这个函数，花括号得到的是函数的返回值

过滤器函数必须被定义到filters节点下（与methods平级）

过滤器一定要有return返回值

过滤器的形参中，val代表管道符前面的那个值（不一定要叫val，形参名合法就行，只是可以通过形参获取管道符前面那个待处理的值）

示例代码：
 <div id="app">
    <p>message 的值是：{{ message | capitalize }}</p>
  </div>
 
  <script src="./lib/vue-2.6.12.js"></script>
  <script>
    const vm = new Vue({
      el: '#app',
      data: {
        message: 'hello vue.js'
      },
      // 过滤器函数，必须被定义到 filters 节点之下
      // 过滤器本质上是函数
      filters: {
        // 注意：过滤器函数形参中的 val，永远都是“管道符”前面的那个值
        capitalize(val) {
          // 字符串有 charAt 方法，这个方法接收索引值，表示从字符串中把索引对应的字符，获取出来
          // val.charAt(0)
          const first = val.charAt(0).toUpperCase()
          // 字符串的 slice 方法，可以截取字符串，从指定索引往后截取
          const other = val.slice(1)
          // 强调：过滤器中，一定要有一个返回值
          return first + other
        }
      }
    })
  </script>
</div>

私有过滤器和全局过滤器

实际开发中更多的是定义全局过滤器 
如果全局过滤器和私有过滤器名字一致，会根据就近原则调用私有过滤器

例子：
<td>{{ item.time | dateFormat }}</td>
...
<!-- 只要导入了 dayjs 的库文件，在 window 全局，就可以使用 dayjs() 方法了 -->
  <script src="./lib/dayjs.min.js"></script>
...
<script>
// 声明格式化时间的全局过滤器
    Vue.filter('dateFormat', function (time) {
      // 1. 对 time 进行格式化处理，得到 YYYY-MM-DD HH:mm:ss
      // 2. 把 格式化的结果，return 出去
 
      // 直接调用 dayjs() 得到的是当前时间
      // dayjs(给定的日期时间) 得到指定的日期
      const dtStr = dayjs(time).format('YYYY-MM-DD HH:mm:ss')
      return dtStr
    })
...
</script>

连续调用多个过滤器

过滤器传参

过滤器的兼容性

```
![[Pasted image 20220618100415.png]]



# 3 vue组件和生命周期

# 4 组件的高级用法

# 5 路由（vue-router）
