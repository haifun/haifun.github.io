---
layout: post
title:  "AngularJS的依赖注入方式"
date:   2014-07-18 10:03:52
categories: AngularJS
---

在定义controller,module,service,and directive时有两种方式，

###方式一：

    var myModule = angular.module('myApp', []);
    myModule.controller('myCtrl', ['$scope', 'Project', function($scope, Project) {

    }]);

###方式二：

    var myModule = angular.module('myApp', []);
    function myCtrl($scope, Project) {

    }
    myModule.controller('myCtrl', myCtrl);

这两种方式在本质上并没有什么区别，不过方式二会造成作用域的污染。

也许你还会意识到以上两种定义方式里的依赖注入的方式也有一些不一样，下面简单总结一下：

####1.简单注入方式(Simple injection method)

    function myCtrl($scope,Project){};
    myModule.controller('myCtrl', myCtrl);
    //或者
    myModule.controller(function($scope,Project){
            
    })

AngularJs会扫描function的参数，提取参数的名称(name)作为function的依赖，

所以这种方式要求保证参数名称的正确性，但对参数的顺序并没有要求；

但是这种注入方式有一个问题，当我们将项目发布到正式环境时都会压缩我们的代码，这时function的参数可能会变成a,b，这就会导致我们的代码出现问题，下面两种注入方式可以帮我们解决这个问题。

####2.数组注释法(array-style notation)

    myModule.controller('myCtrl', ['$scope', 'Preject', function($scope, Project) {
            
    }])

每一个依赖的参数值（字符串）都会以相同的顺序存放在一个数组里，数组的值与后面的function参数一一对应，这样即使压缩了也不会有什么问题。

如果你不喜欢这种数组注释的定义方式，下面还有一种方式。

####3.显示调用function的$inject

AngularJs提供了一种向injector server通知你想要注入的依赖的方式

    function myCtrl(a, b) {
        //$scope, Project,故意改成a,b模拟压缩后的情形
    }
    myCtrl.$inject = ['$scope', 'Project'];
    myModule.controller('PhoneDetailCtrl', myCtrl);

如上，通过设置funciton的$inject属性，可以达到依赖注入的效果；
