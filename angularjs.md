## angularjs 是 mvc 还是 mvvm 框架?

首先阐述下你对mvc和mvvm的理解：

首先为什么我们会需要MVC？因为随着代码规模越来越大，切分职责是大势所趋，还有为了后期维护方便，修改一块功能不影响其他功能。还有为了复用，因为很多逻辑是一样的。而MVC只是手段，终极目标是模块化和复用。

mvvm的优点:

1. 低耦合：View可以独立于Model变化和修改，同一个ViewModel可以被多个View复用；并且可以做到View和Model的变化互不影响
2. 可重用性：可以把一些视图的逻辑放在ViewModel，让多个View复用
3. 独立开发：开发人员可以专注与业务逻辑和数据的开发（ViewModemvvmdi计人员可以专注于UI\(View\)的设计
4. 可测试性：清晰的View分层，使得针对表现层业务逻辑的测试更容易，更简单

在angular中MVVM模式主要分为四部分：

1. View：它专注于界面的显示和渲染，在angular中则是包含一堆声明式Directive的视图模板。
2. ViewModel：它是View和Model的粘合体，负责View和Model的交互和协作，它负责给View提供显示的数据，以及提供了View中Command事件操作Model的途径；在angular中
`$scop`
e对象充当了这个ViewModel的角色；
3. Model：它是与应用程序的业务逻辑相关的数据的封装载体，它是业务领域的对象，Model并不关心会被如何显示或操作，所以模型也不会包含任何界面显示相关的逻辑。在web页面中，大部分Model都是来自Ajax的服务端返回数据或者是全局的配置对象；而angular中的service则是封装和处理这些与Model相关的业务逻辑的场所，这类的业务服务是可以被多个Controller或者其他service复用的领域服务。
4. Controller：这并不是MVVM模式的核心元素，但它负责ViewModel对象的初始化，它将组合一个或者多个service来获取业务领域Model放在ViewModel对象上，使得应用界面在启动加载的时候达到一种可用的状态。

## angular 核心？

AngularJS是为了克服HTML在构建应用上的不足而设计的。 AngularJS有着诸多特性，最为核心的是：

* MVC
* 模块化
* 自动化双向数据绑定
* 语义化标签、依赖注入等等

## factory 和 service，provider是什么关系？

factory 把 service 的方法和数据放在一个对象里，并返回这个对象；service 通过构造函数方式创建 service，返回一个实例化对象；provider 创建一个可通过 config 配置的 service。

从底层实现上来看，service 调用了 factory，返回其实例；factory 调用了 provider，将其定义的内容放在 `$get` 中返回。factory 和 service 功能类似，只不过 factory 是普通 function，可以返回任何东西（return 的都可以被访问，所以那些私有变量怎么写你懂的）；service 是构造器，可以不返回（绑定到 this 的都可以被访问）；provider 是加强版 factory，返回一个可配置的 factory。

## ng-if 跟 ng-show/hide的区别有哪些？

1. ng-if 在后面表达式为 true 的时候才创建这个 dom 节点，ng-show 是初始时就创建了，用 display:block 和 display:none 来控制显示和不显示。
2. ng-if 会（隐式地）产生新作用域，ng-switch 、 ng-include 等会动态创建一块界面的也是如此。

## ng-repeat迭代数组的时候，如果数组中有相同值，会有什么问题，如何解决？

会提示 Duplicates in a repeater are not allowed. 加 `track by $index` 可解决。当然，也可以 trace by 任何一个普通的值，只要能唯一性标识数组中的每一项即可（建立 dom 和数据之间的关联）。

在写controlloer逻辑的时候 你需要注意什么？

1. 简化代码（这个是所有开发人员都要具备的）
2. 坚决不能操作dom节点 这个时候可能会问 为什么不能啊

你的回答是：DOM操作只能出现在指令（directive）中。最不应该出现的位置就是服务（service）中。Angular倡导以测试驱动开发，在service或者controller中出现了DOM操作，那么也就意味着的测试是无法通过的。当然，这只是一点，重要的是使用Angular的其中一个好处是啥，那就是双向数据绑定，这样就能专注于处理业务逻辑，无需关系一堆堆的DOM操作。如果在Angular的代码中还到处充斥着各种DOM操作，那为什么不直接使用jquery去开发呢。

测试驱动开发是什么呢？普及一下：

测试驱动开发，英文全称Test-Driven Development，简称TDD，是一种不同于传统软件开发流程的新型的开发方法。它要求在编写某个功能的代码之前先编写测试代码，然后只编写使测试通过的功能代码，通过测试来推动整个开发的进行。这有助于编写简洁可用和高质量的代码，并加速开发过程。

## angular 的数据绑定采用什么机制？详述原理。

1、每个双向绑定的元素都有一个watcher

2、在某些事件发生的时候，调用digest脏数据检测。 这些事件有：表单元素内容变化、Ajax请求响应、点击按钮执行的函数等。

3、脏数据检测会检测rootscope下所有被watcher的元素。

`$digest`函数就是脏数据监测



## 在使用angularjs项目开发中 你使用过那些第三方的插件?

AngularUi、ui-router、oclazyload、angular-material、ui-bootstrap、ui-grid、ng-dialog、等等



## controller之间怎么通讯?

1. event
这里可以有两种方式，一种是`$scope.$emit`，然后通过监听`$rootScope`的事件获取参数；另一种`$rootScope.$broadcast`

，通过监听`$scope`的事件获取参数。

这两种方法在最新版本的Angular中已经没有性能区别了，主要就是事件发送的方向不同，可以按实际情况选择。

1. service
可以创建一个专用的事件Service，也可以按照业务逻辑切分，将数据存储在相应的Service中
2. `$rootScope`
这个方法可能会比较dirty一点，胜在方便，也就是把数据存在`$rootScope`中，这样各个子`$scope`都可以调用，不过需要注意一下生命周期
3. 直接使用`$scope.$$nextSibling`及类似的属性 类似的还有`$scope.$parent`。这个方法的缺点就更多了，官方不推荐使用任何`$$`开头的属性，既增加了耦合，又需要处理异步的问题，而且scope的顺序也不是固定的。不推荐

另外就是通过本地存储、全局变量或者现代浏览器的postMessage来传递参数了，除非特殊情况，请避免这类方式。

## angular 中的 `$http`

`$http` 是 AngularJS 中的一个核心服务，用于读取远程服务器的数据。
我们可以使用内置的`$http`服务直接同外部进行通信。`$http`服务只是简单的封装了浏览器原生的XMLHttpRequest对象。

1. 链式调用
`$http`服务是只能接受一个参数的函数，这个参数是一个对象，包含了用来生成HTTP请求的配置内容。这个函数返回一个promise对象，具有success和error两个方法。
2. 返回一个promise对象
3. 快捷的get请求
4. 响应对象

## 单页应用有哪些优缺点？

单页Web应用（single page web application，SPA），就是只有一张Web页面的应用。单页应用程序 \(SPA\) 是加载单个HTML 页面并在用户与应用程序交互时动态更新该页面的Web应用程序。 浏览器一开始会加载必需的HTML、CSS和JavaScript，所有的操作都在这张页面上完成，都由JavaScript来控制。因此，对单页应用来说模块化的开发和设计显得相当重要。

* 速度：更好的用户体验，让用户在web app感受native app的速度和流畅，
* MVC：经典MVC开发模式，前后端各负其责。
* ajax：重前端，业务逻辑全部在本地操作，数据都需要通过AJAX同步、提交。
* 路由：在URL中采用\#号来作为当前视图的地址,改变\#号后的参数，页面并不会重载。

优点：

1. 分离前后端关注点，前端负责界面显示，后端负责数据存储和计算，各司其职，不会把前后端的逻辑混杂在一起；
2. 减轻服务器压力，服务器只用出数据就可以，不用管展示逻辑和页面合成，吞吐能力会提高几倍；
3. 同一套后端程序代码，不用修改就可以用于Web界面、手机、平板等多种客户端；

缺点：

1. SEO问题，现在可以通过Prerender等技术解决一部分；
2. 前进、后退、地址栏等，需要程序进行管理；
3. 书签，需要程序来提供支持；



## ng 内置的 filter 有？

* date（日期）
* currency（货币）
* limitTo（限制数组或字符串长度）
* orderBy（排序）
* lowercase（小写）
* uppercase（大写）
* number（格式化数字，加上千位分隔符，并接收参数限定小数点位数）
* filter（处理一个数组，过滤出含有某个子串的元素）



### 如何取消 `$timeout`, 以及停止一个`$watch()`?



停止 $timeout我们可以用cancel：

```js
var customTimeout = $timeout(function () {
  // your code
}, 1000);

$timeout.cancel(customTimeout);
```

停掉一个`$watch`：

```js
// .$watch() 会返回一个停止注册的函数
var unWatchFn = $rootScope.$watch(‘someGloballyAvailableProperty’, function (newVal) {
  if (newVal) {
    // we invoke that deregistration function, to disable the watch
    unWatchFn();
    ...
  }
});
```



## 有哪些措施可以改善Angular 性能

官方提倡的，关闭debug,

```js
myApp.config(function ($compileProvider) {
  $compileProvider.debugInfoEnabled(false);
});
```

* 使用一次绑定表达式即{{::yourModel}}
* 减少watcher数量
* 在无限滚动加载中避免使用ng-repeat,关于解决方法可以参考这篇[文章](http://www.williambrownstreet.net/blog/2013/07/angularjs-my-solution-to-the-ng-repeat-performance-problem/)
* 使用性能[测试](http://lib.csdn.net/base/softwaretest)的小工具去挖掘你的angular性能问题，我们可以使用简单的`console.time()`也可以借助开发者工具

## 你认为在Angular中使用jQuery好么？

这是一个开放性的问题，尽管网上会有很多这样的争论，但是普遍还是认为这并不是一个特别好的尝试。其实当我们学习Angular的时候，我们应该做到从0去接受angular的思想，数据绑定，使用angular自带的一些api，合理的路由组织和，写相关指令和服务等等。angular自带了很多api可以完全替代掉[jQuery](http://lib.csdn.net/base/jquery)中常用的api，我们可以使用`angular.element`，`$http`,`$timeout`,`ng-init`等。

我们不妨再换个角度，如果业务需求，而对于一个新人（比较熟悉jQuery）的话，或许你引入jQuery可以让它在解决问题，比如使用插件上有更多的选择，当然这是通过影响代码组织来提高工作效率，随着对于angular理解的深入，在重构时会逐渐摒弃掉当初引入jquery时的一些代码。

所以我觉得两种框架说完全不能一起用肯定是错的，但是我们还是应该尽力去遵循angular的设计。
