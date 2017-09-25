---

layout: post

title:  AngularJs学习笔记

date:   2017-09-25 21:32:00 +0800

categories: document

tag: 学习笔记

---

*   content
{:toc}

AngularJs现在是风靡全球，博主为了不被淘汰，也花了些许事件去学习Angular。下面将一些基础的知识点PO出来，方便以后学习和使用。

AngularJs是通过**ng-drectives**来扩展HTML的，所有方法都是通过它来实现的，用户也可以自定义ng-drectives来实现自定义的方法。

# 基本指令

## ng-app

-  ng-app 是定义一个AngularJs的应用程序，一个页面只能有一个ng-app,具体的代码如下：ng-app="myApp"。当然比较简单的应用程序，我们也可以不知道ng-app的名称。

##ng-model
- ng-model是指把元素值绑定到应用程序上去。
```
    <div ng-app="myApp" >
        <input type="text" ng-model="myModel">
        你输入的值为：{{myModel}}
    </div>
```
在一个Angular应用程序中，如果有多个相同名字的model，修改其中一个的值，其他有相同名字的model值也会跟着改变。

##ng-bind

- ng-bind 相当于是将数据展示出来。当然可以进行简单的计算，将计算记过展示在前台页面。
在HTML中，用户可以通过**ng-bind="myModel"**来展示myModel的值，也可以通过**{{}}**两个大括号来展示。不过我还是推荐使用第二种方法，因为比较简单和明了。

##ng-init 

- ng-init主要是初始化数据，不过我们可以在JavaScript中初始化数据，得到的效果是一样的。

##ng-controller

- ng-controller相当于整个应用程序的控制器，一个应用程序可以有多个控制器。

##作用域

作用域分为两种，一个是局部作用域，一种是全局作用域。

###局部作用域--$scope

$scope 只能在当前控制器中使用，超出了控制器就无效了。不同控制器的$scope不能相互访问。

###全局作用域--$rootscope

$rootscope 可用于整个程序的全部位置，不同或者相同的控制器中都可以访问。

###使用范围
作用域可以定义**变量**：字符串、数字、数组、对象甚至数组对象。也可以定义**方法**，在方法中实现其他的逻辑业务，该方法可以是匿名方法。


## 来一段简单的代码
```
    <div ng-app="myApp" ng-controller="myCtrol">
        FirstName:<input type="text" ng-model="firstname">
        LastName:<input type="text" ng-model="lastname">
        姓名：<span ng-bind="firstname + ' '+lastname"
    </div>
    <script>
        var app=angular.module("myApp",[])
        app.controller("myCtrol",function($scope){
            $scope.firstname="Pan"
            $scope.lastname="Da"
        })
    </script>
```

# 为什么使用Angular

1. AngularJs使得开发现代的单一页面应用程序（SPAs）变得更加的容易。
2. AngularJs支持HTML5，我们可以使用**data-ng-****来实现h5的某些效果。
3. 实现了前端了MVC模式，更加直观易懂。 

4. 在相对比较复杂的页面渲染上，减少了数据交互的必要性，减轻了服务器的压力，使得应用更加的流畅，提高用户的体验度。


# ng-repeat和ng-options的差别

ng-reapeat一般用于循环数组等信息，使用方法为：
```
    <ul>
        <li data-ng-repeat="x in names">{{x}}</li>
    </ul>
    <script>
        var app=angular.module("myApp",[]).controller("myCtrol",function($scope){
        $scope.names=["Panda","Panzhu","github"];
        })
        
    </script>
```
上述代码中，就可以吧names中的数据遍历出来。但假如我们有一个对象数组的时候，类似于这样*$scope.names=[{name:"panda",city:"Singapore"}，{name:"panzhu",city:"chengdu"},{name:"Github",city:"New York"}]*的对象数组，当我们都把这两个信息都拿出来的时候，我们可以

```
    <select ng-model="mySelect" ng-options="x.name for x in names"/>
    <!--这是会出现一个下拉框，遍历names中的数据-->
    <h1>你选择的名字是：{{mySelect.name}}</h1>
    <p>所在的城市为：{{mySelect.city}}</p>
```
当然，如果是一个key-Value的对象的话，ng-options可以这样使用*ng-options="y.name for (x,y) in names"*。

# 自定义directive

前面我们说过，AngularJs是通过directive来驱动的，那我们可以自定义一个directive吗？当然可以啦
```
    app.directive("ownDirective",function(){
        return {
            template:"<a href='javascript:;'>自定义directive的链接"</a>
            restrict:"A"
        }
    })
```
在上述代码中，我们定义了一个directive，template钟是要显示的信息，restrict是指限制使用的方式。
在页面上我们可以有**4**中使用方式。
```
<!--
1. 元素名 <own-directive></own-directive>  对应的restrict是“E”
2. 属性 <span own-directive></span> 对应的restrict是“A”
3. 类名 <span class="own-directive"></span> 对应的restrict是“C”
4. 注释 <!-- own-directive> 对应是restrict是“M”
-->
```

#过滤器是使用

在Angular中，也有过滤器的说法， 使用方法是使用“**|**”。默认的方法有五种，分别是：1、currency 2、filter 3、lowercase 4、uppercase 5、orderBy。

##currency

currency是将数字转化为货币的格式，例如将100转化为“$100”。使用方法是：“ng-bind='Number | currency'”

##lowercase和uppercase

将字符串转换为小写（大写）格式。使用方法同上。

##orderBy

orderBy表示排序，可以指定一个数值，按照该值的大小顺序来排序，不过要排序的话，肯定是要有很多条数据才行，所以一般用于循环输出，类似于下面的代码：

```
    <div ng-app="myApp" ng-controller="namesCtrol">
        <p>输入过滤：<input type="text" ng-model="filterTest">
        <ul>
            <li ng-repeat="x in names | orderBy:'city' | filter:filterTest ">
            {{x}}
            </li>
        </ul>
    </div>
```

##filter

filter顾名思义就是过滤的意思啦，在上述代码中，我们指定了一个model为**filterTest**,在这个输入框中输入的信息，会自动在**ng-repeat**中去筛选。

##自定义过滤器

直接上代码，通俗易懂：
```
    var app=angular.module("myApp",[]);
    app.controller('myCtrl',function($scope){
        $scope.name="panda";
    });
    app.filter("reverse",function(){//可以依赖注入
        return function(text){
            return text.reverse().join("");
        }
    })
```

###2017年9月25日23:58:45未完待续
