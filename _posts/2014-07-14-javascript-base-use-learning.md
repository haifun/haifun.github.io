---
layout: post
title:  "javascript编写的基本技巧"
date:   2014-07-14 10:25:07
categories: javascript
---

做前端开发，每天都要接触大量的js代码和css代码，这里介绍一些如何编写高质量的js代码，介绍大牛们常用的一些模式和习惯。比如避免全局变量、使用单个变量var的声明、循环中的从小缓存长度变量length和符合代码约定等。

##编写可维护的代码

易维护的代码意味着代码具有如下特性：

* 阅读性好。
* 具有一致性
* 预见性好。
* 看起来如同一个人编写。
* 有文档。

## 尽量少用全局变量
Javascript 使用函数管理作用域。变量在函数内声明，只有在函数内有效，不能在外部使用。全局变量与之相反，在函数外部声明，在函数内部无需声明即可很简单的使用。

每一个Javascript环境的都有全局对象，可在函数外部使用this进行访问。创建的每一个全局变量都为全局对象所有。在浏览器中，为了方便，使用window表示全局对象本身。

    var myglobal = "hello";
    console.log(myglobal); // "hello"
    console.log(window.myglobal);  // "hello"
    console.log(window["myglobal"]); // "hello"
    console.log(this.myglobal); // "hello"



###未完待续