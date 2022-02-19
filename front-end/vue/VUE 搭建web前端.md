### VUE 搭建web前端
Vue的版本选择是否关键，比如和Java与MySQL一样版本影响蛮大，不同版本间。https://www.cnblogs.com/syingBlog/p/12580017.html 目前vue使用的版本
```shell
vue -V
@vue/cli 4.5.13
```
卸载安装新版本，可以参考上述的链接：
于是就先通过 npm uninstall vue-cli -g卸载vue，然后再安装，但是vue -V时依然是2.9.6版本：
第一步：
```shell
npm config get registry
```
第二步：
```shell
npm config set registry https://registry.npm.taobao.org
```
第三步：
```shell
npm i -g @vue/cli
```
#### 学习资源
1.vue官网，有doc文档 https://v3.cn.vuejs.org/ ,这个是最基础的
2.vue-element-admin，是一个后台templete, doc文档也有 https://panjiachen.gitee.io/vue-element-admin-site/zh/ ，可以在这个基础上进行后台前端的二次开发
3.Github 开源项目，一个大佬TyCoding，已经follow，可以去GitHub，看相关的开源项目，其中该网址 https://tycoding.cn/ 有介绍其博客系统，两个实现，其中一个前后端分离Tomu-vue；而且有相关的开发文档可以阅读，写的也是很好
4.npm初始化创建vue项目 https://blog.csdn.net/cywtd/article/details/105559542 可做参考
发现还是得学一下vue的知识，如果打算自己从0到1的开发自己的博客网站。可以去菜鸟上速成一下，主要是不能心急，想一口吞大象，规划好步骤和进度即可，水到渠成。菜鸟教程上的vue应该是已经够用了。

#### 前端搭建
前后端分离，是通过API或者说接口进行的数据的交互。那就有几个问题要考虑：如何搭一个前端，同时支持后端分离？MVC和MVVM有什么区别，各自的技术实现？分离中使用的axios、Ajax和fetch有什么区别？在部署的时候前端的部署和后端的部署是分开部署？如何部署在同一台服务器上？Restful API 和SOAP的服务架构？DOM是什么？
##### MVC和MVVM

##### 前端支持分离
在使用API获取后端服务提供的数据的时候，采用axios来将得到的JSON数据进行处理。使用axios的过程：
###### axios
axios的安装：
```shell
npm install axios
```
也有使用cnpm进行安装的，在安装完成后提示 npm aduit fix说是让修复一些东西，和之前下载使用前端的工程时候遇到的问题一样。
先从简单的demo开始，没有复杂的组件，然后API也只是有一个这种。然后就是把这种直接使用的情况进行封装一层。
###### 跨域问题
果然一上来就遇到了问题，CORS 跨域问题，跨域可以理解，然后基于axios+spring boot怎么解决这个问题呢？也有很多方案，我自己试了两种确实可行的。
1.axios前端通过代理解决
```javascript
module.exports = {
    //axios域代理，解决axios跨域问题
    
    devServer: {
        proxy: {
            '/greeting': {
                target: 'http://127.0.0.1:8080',
                changeOrigin: true,
                ws: true,
                pathRewrite: {

                }
            }
        }
    }
}
```
2.后端通过配置@CrossOrigin来解决

```java
	//@CrossOrigin
	@GetMapping("/greeting")
	public Greeting greeting(@RequestParam(value = "name", defaultValue = "World") String name) {
		return new Greeting(counter.incrementAndGet(), String.format(template, name));
	}
```
两种跨域的解决方案，使用前端跨域对自己是够用的。内部原理，前端解决跨域是将/api的请求加以拦截，然后通过代理来解决跨域。后端spring boot的注解解决跨域。

https://www.jianshu.com/p/8bc48f8fde75 axios、Ajax和fetch
https://www.cnblogs.com/nogodie/p/9853660.html https://www.cnblogs.com/ouyangkai/p/11713271.html https://blog.csdn.net/qq_32584661/article/details/89495964 axios使用