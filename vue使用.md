Vue使用

# package.json文件

`package.json` 文件至少要有两部分内容：

1. “name” 
   - 全部小写，没有空格，可以使用下划线或者横线
2. “version” 
   - x.x.x 的格式
   - 符合“语义化版本规则”

#修改包名字

package.json

打包语句

最外层index.html

#端口号修改

config/index.js 

 dev:{ 

​	port:8088//修改生效

}

build/build.js 修改不生效，暂不知何意



# html-webpack-plugin

[插件 html-webpack-plugin 的详解]

https://segmentfault.com/a/1190000013883242



# DefinePlugin

`DefinePlugin`可以在编译时期创建全局变量。

HotModuleReplacementPlugin

热更新是webpack的一个特性，通过无刷新实现代码更新

html-webapck-plugin

最近在react项目中初次用到了[`html-webapck-plugin`](https://github.com/ampedandwired/html-webpack-plugin)插件，用到该插件的两个主要作用：

- 为html文件中引入的外部资源如`script`、`link`动态添加每次compile后的hash，防止引用缓存的外部文件问题
- 可以生成创建html入口文件，比如单页面可以生成一个html文件入口，配置**N**个`html-webpack-plugin`可以生成**N**个页面入口

# vue项目配置后端接口服务信息

[构建：vue项目配置后端接口服务信息]

https://www.cnblogs.com/zs-note/p/9118370.html

1、config/dev.env.js文件中配置baseURL

2、axios配置baseURL  utils/request.js

3、config/index.js文件中配置开发环境代理

可配置代理地址



webpack.base.config.js

entry入口



process何意？

文档：[http://nodejs.cn/api/process....](http://nodejs.cn/api/process.html)
**官方解释**：`process` 对象是一个 `global` （全局变量），提供有关信息，控制当前 `Node.js` 进程。作为一个对象，它对于 `Node.js` 应用程序始终是可用的，故无需使用 `require()`。

**官方**: `process.env`属性返回一个包含用户环境信息的对象。

文档：[http://nodejs.cn/api/process....](http://nodejs.cn/api/process.html#process_process_env)

# process.env信息包含

config/dev.env.js

config/index.js



1、登录页面

地址重定向http://localhost:9527/#/login?redirect=%2Fdashboard

返回页面views/login/index

地址为空，重定向dashboard

没有token，路由为空，不在白名单，指定为login?redirect=%2Fdashboard，下一次路由，为白名单登录，直接路由到登录页面。



如果有token，为登录页面，重定向/,则到dashboard



每个守卫方法接收三个参数：
to: Route: 即将要进入的目标 路由对象

from: Route: 当前导航正要离开的路由

next: Function: 一定要调用该方法来 resolve 这个钩子。执行效果依赖 next 方法的调用参数。

next(): 进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是 confirmed （确认的）。

next(false): 中断当前的导航。如果浏览器的 URL 改变了（可能是用户手动或者浏览器后退按钮），那么 URL 地址会重置到 from 路由对应的地址。

next('/') 或者 next({ path: '/' }): 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。

nprogress进度条，引入

权限permission.js

# directive

[Vue.directive( id, [definition\] )](https://cn.vuejs.org/v2/api/#Vue-directive)

- **参数**：

  - `{string} id`
  - `{Function | Object} [definition]`

- **用法**：

  注册或获取全局指令。

  ```
  // 注册
  Vue.directive('my-directive', {
    bind: function () {},
    inserted: function () {},
    update: function () {},
    componentUpdated: function () {},
    unbind: function () {}
  })
  
  // 注册 (指令函数)
  Vue.directive('my-directive', function () {
    // 这里将会被 `bind` 和 `update` 调用
  })
  
  // getter，返回已注册的指令
  var myDirective = Vue.directive('my-directive')
  ```





单点登录

综合服务

https://cas.sf-express.com/cas/login?service=http%3A%2F%2Fasps.sf-express.com%2Findex.pub&apptiket=e2854799b05a9ba213c249dc02491ad5a5393ed83c41b7b7

知识管理平台

https://asura.sf-express.com/cas/login?service=http%3A%2F%2Fkms.sf-express.com%2FKMS%2Findex%2Finit.action&apptiket=e2854799b05a9ba2653fabe3855be08218cf829000ed010c

动态配置中心

http://cas.sit.sf-express.com/cas/login?service=http://10.202.35.74:8080/login#

员工自助平台

https://cas.sf-express.com/cas/login?service=http%3A%2F%2Fsfhrss.sf-express.com%2Fsap%2Fplatform%2Fconsole%2FnewHome.ht&apptiket=d2c502dec1b18dc34ae3625889076421a5393ed83c41b7b7

IT服务中心

https://cas.sf-express.com/cas//login?service=http%3A%2F%2Fitil-itsm.sf-express.com%2Fbalantflow%2F

工时管理

https://asura.sf-express.com/cas/login?service=http%3A%2F%2Fitmp.sf-express.com%2Floginmgmt%2Fframe.action%3Fname%253Dtimemgmt%2526frameSize%253D1&apptiket=dbe979f1b41f6ea26c926ac27ad38c7318cf829000ed010c

岗位模板

https://cas.sf-express.com/cas/login?service=http%3A%2F%2Fsfecp.sf-express.com%2Fecp%2Fplatform%2Fconsole%2Ftoolbar.ht&apptiket=d2c502dec1b18dc37e7013dd727dc958a5393ed83c41b7b7

治水平台

https://cas.sf-express.com/cas/login?service=http://sdeis.sf-express.com/pcenter/shiro-cas

https://cas.sf-express.com/cas/login?service=http://localhost:8088/dashboard



Cookie:
app_uid=MYUU3xNmGWrZnijMC_rKwHi07pUR3uWlXyg_yoZlO_u2xEPpiyKfVMDBWUQIFSZtQ2oN-oqvX9dIQec0yHgo2U2YdTuJOZelGUAIs2s4J7mhe9Cfkgiq4UihsX84QmleroRBvPlyG_MUkfzgv5u4n_wrYDtczLNIaRl7SKF0g9gfQVZP7oRSO4PkQ1sGBHxEE8SpM_MT1Il379KH1k28YCQhq66yywjeDVNfKTcyxclxBfRDYhNmMNDaFr6vkHmyvV49FjCH9PAwPM-oUA0kSYe8kyuiuzXZxiOw4BCdbqAyq_PZavpa__EdPcaikoWH5jzksHof9Hdlwe73qb93TuO00o_IWce2INyeudonY-eAEhDdOJ9seeKcIAc7EPEqMijIOuGHnempZxElVKX8wLljB1vgZpGkFVXSGVoZ3uyOdFB_QFQYYNDnFjTovqdj; 
EPT_CNSZ17=EPT_CNSZ17_18_120_8080; 
ASURA_CAS_CNSZ17=ASURA_CAS_CNSZ17_18_113_8080;
JSESSIONID=2220CC749A22798C94D21E765A7820D1; 
ASURA_CAS_443_CNSZ17=ASURA_CAS_CNSZ17_16_40_8080



vuex

dispatch：含有异步操作，例如向后台提交数据，写法： this.$store.dispatch('mutations方法名',值)

commit：同步操作，写法：this.$store.commit('mutations方法名',值)



配置路由

1、router/index.js





 this.$route.query或者 this.$route.params 接收router-link传的参数

在路由跳转的时候除了用router-link标签以外需要在script标签在事件里面跳转，所以有个方法就是在script标签里面写this.$router.push('要跳转的路径名')，



$route为当前router跳转对象里面可以获取name、path、query、params等

$router为VueRouter实例，想要导航到不同URL，则使用$router.push方法

返回上一个history也是使用$router.back()方法





对于vue.js中的this.emit的理解：this.emit的理解：this.emit(‘increment1’,”这个位子是可以加参数的”)；其实它的作用就是触发自定义函数。



格式化语言

https://q.cnblogs.com/q/108194/



https://www.jianshu.com/p/0229211ea679

vue组件调用action



actions调用mutations

mutations设置state



REQUEST

1. GET /admin/user/selectUserInfoList?offset=0&pageSize=10&_=1540625625769 HTTP/1.1 
2. Host: localhost:3332 //指定请求的服务器的域名和端口号
3. Connection: keep-alive //表示是否需要持久连接。（HTTP 1.1默认进行持久连接）
4. Accept: application/json, text/javascript, */*; q=0.01  //指定客户端能够接收的内容类型
5. X-Requested-With: XMLHttpRequest  ////表明是AJax异步可以利用它，request.getHeader("x-requested-with"); 为 null，则为传统同步请求，为 XMLHttpRequest，则为 Ajax 异步请求。
6. User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/49.0.2623.112 Safari/537.36 
7. Content-Type: application/json  //请求的与实体对应的MIME信息
8. Referer: http://localhost:3332/admin/login/index //先前网页的地址，当前请求网页紧随其后,即来路
9. Accept-Encoding: gzip, deflate, sdch  //指定浏览器可以支持的web服务器返回内容压缩编码类型。
10. Accept-Language: zh-CN,zh;q=0.8 //浏览器可接受的语言
11. Cookie: JSESSIONID=dx53c71shl9x1w9myio28wc29; Admin-Token=admin // 浏览器当前域名下的所有连接



1. Access-Control-Request-Headers:

   accept, x-token  预检请求头部

2. Access-Control-Request-Method:

   GET

RESPONSE

1. Content-Type:

   application/json;charset=UTF-8 //返回内容的MIME类型

2. Date:

   Sat, 27 Oct 2018 07:33:56 GMT //原始服务器消息发出的时间

3. Server:

   Jetty(9.2.7.v20150116) //web服务器软件名称

4. Transfer-Encoding:

   chunked //文件传输编码





福彩后台，跳转到了cas登录页面了



自己定义的Springmvc后台加了跨域的mvc注解就能查询到结果



原来这个key属性是vue自带特殊属性,主要用在 Vue 的虚拟 DOM 算法，在新旧 nodes 对比时辨识 VNodes。如果不更新这个key的话,显示隐藏列的时候,部分DOM不会重新渲染,导致table变化时候没有动画过度,显得很生硬.





**关于.sync修饰符**  子件里面的值更新，父组件也更新

Vue官网上对[.sync](https://cn.vuejs.org/v2/guide/components-custom-events.html#sync-%E4%BF%AE%E9%A5%B0%E7%AC%A6%20%20.sync)也有解释，简言之，`:name.sync`就是`:name="name" @update:name="name = $event"`的缩写。
<child :name.sync="name"></child>

<child :name="name" @update:name="name = $event"></child>



登录设置权限



LoginByUsername

# CAS-Client客户端研究(四)-HttpServletRequestWrapperFilter

https://blog.csdn.net/yuwenruli/article/details/6602667



HttpServletRequestWrapperFilter其实作用很简单，就是在HttpServletRequest对象再包装一次，让其支持getUserPrincipal，getRemoteUser方法来取得登录的用户信息。



增加一个方法



 

# 全局样式，引入scss文件

1、main.js引入

import '@/styles/index.scss' // global css

2、需要引入解析scss文件

npm install sass-loader node-sass --save-dev

 https://segmentfault.com/a/1190000009802725



# 自定义主题样式修改Element-ui

table修表单头样式

 <el-table v-loading="listLoading" :data="list" :header-cell-style="headerClassHandler" border fit highlight-current-row style="width: 100%" >

methods增加方法
headerClassHandler(row, rowIndex) {
​      console.log('表头样式添加')
​      return {
​        'background-color': '#fafafa',
​        'color': 'rgb(103, 194, 58)',
​        'border-bottom': '1px rgb(103, 194, 58) solid'
​      }
​    }

style scoped 样式不能修改

整体修改样式 https://www.jianshu.com/p/e946456f3d09

自定义主题http://element-cn.eleme.io/#/zh-CN/component/custom-theme
第一步下载生成工具：
npm i element-theme -g
第二步下载安装白垩主题
npm i element-theme-chalk -D

第三步初始样式安装默认文件
et -i oms-element-variables.scss

第四步修改样式
第五步编译保存样式
et -c oms-element-variables.scss

默认生成的样式保存在./theme   你可以通过 -o 参数指定打包目录

第六步 引入自定义主题
import '../theme/index.css'
第七步 重启就行

## 

# 组件件通信

数组取值： 
1、数组编译方法https://cn.vuejs.org/v2/guide/list.html#变异方法
Vue 包含一组观察数组的变异方法，所以它们也将会触发视图更新。变异方法 (mutation method)，顾名思义，会改变被这些方法调用的原始数组
这些方法如下
push()
pop()
shift()
unshift()
splice()
sort()
reverse()
你打开控制台，然后用前面例子的 items 数组调用变异方法：example1.items.push({ message: 'Baz' }) 。

非变异 (non-mutating method) 方法，例如：filter(), concat() 和 slice() 。这些不会改变原始数组，但总是返回一个新数组

修改后端来的数据
response.data.items.forEach(item => {
​          console.log(item)
​          item.statusDesc = item.status
​          templist.push(item)
​        })
​        this.list = templist

const items = response.data.items
​        this.list = items.map(v => {
​          this.$set(v, 'edit', false) // https://vuejs.org/v2/guide/reactivity.html
​          v.originalTitle = v.title //  will be used when user click the cancel botton
​          return v
​        })

 this.list = response.data.items.map(v => {
​          this.$set(v, 'edit', false) // https://vuejs.org/v2/guide/reactivity.html
​          v.statusDesc = v.status //  will be used when user click the cancel botton
​          return v
​        })





# call和apply

继承



**定义:**

**call, apply是函数的方法, 只有函数才有这2个方法.**

**作用:**

**call, apply主要作用是改变函数赖以执行的作用域, 简言之就是改变函数中this的指向.**

**用法:**

**fn.call(obj, args1, args2, ...); //obj是指定函数赖以执行的对象, arg1等是传给函数的参数(假如有的话)** 
**fn.apply(obj, [args1, args2, ...]); //obj是指定函数赖以执行的对象, [arg1, ...]等是传给函数的参数数组(假如有的话)**



vue安装jquery

1、npm install --save jquery

2、 // 引入解析器

​    new webpack.optimize.CommonsChunkPlugin('common.js'),

​    new webpack.ProvidePlugin({

​      jQuery: "jquery",

​      $: "jquery"

​    })
