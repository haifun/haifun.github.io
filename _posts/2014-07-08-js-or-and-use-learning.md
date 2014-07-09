---
layout: post
title:  "Javascript中|| 和&&的用法技巧"
date:   2014-07-08 13:55:07
categories: jquery
---

你是否看到过这样的代码：a=a||""; 可能javascript初学者会对此感到茫然。今天就跟大家分享一下我的一些心得。
其实：
 
    a=a||"defaultValue";  
 
 与：
 
    if(!a){  
        a="defaultValue";  
    }  
 
 和：
 
    if(a==null||a==""||a==undefined){  
        a="defaultValue";  
    }  
 
是等价的！
为了弄清这个问题，首先我们必须了解一个问题：javascript中数据类型在转换为bool类型时发生了什么。
 
在javascript中，数据类型可以分为“真值”和“假值”。顾名思义，真值转换为bool时值为true；假值转换为bool时值为false。下表罗列了一些常见的数据类型转换为bool时的值：
 
数据类型|    转换为bool后的值
---|---
null    |	FALSE
undefined|	FALSE
Object	  |  TRUE
function|	TRUE
0	     |   FALSE
1	      |  TRUE
0、1之外的数字|	TRUE
字符串	       | TRUE
""(空字符串)	|FALSE
 
在if表达式中，javascript首先将条件表达式转换为bool类型，表达式为真值则执行if中的逻辑，否则跳过。
 
于是有了：
 
    if(!a){  
        a="defaultValue";  
    }  
 
下面我们再来看“&&”、“||”两个表达式。
由于javascript是弱类型语言，所以在javascript中这两个表达式可能跟其他语言（比如java）中不太一样。
在javascript中，“&&”运算符运算法则如下：
 
如果&&左侧表达式的值为真值，则返回右侧表达式的值；否则返回左侧表达式的值。
 
这就是说：
 
    var i=""&&"真值";//->i=""  
    i="真值"&&"其他真值";//->i="其他真值"  
    i="真值"&&"";//->i=""  
 
 
“||”运算符的运算法则如下：
 
如果||左侧表达式的值为真值，则返回左侧表达式的值；否则返回右侧表达式的值。
 
这就是说：
 
    var i=""||"真值";//->i="真值"  
    i="真值"||"其他真值";//->i="真值"  
    i="真值"||"";//->i="真值"  
 
 
于是，就可以理解：
 

    a=a||"defaultValue";  
    
的逻辑了。如果a为假值（等于null、空字符串……），则将"defaultValue"赋给a；否则将a的值赋给a本身。
 
 
下面我们运用||、&&来简化程序：
 
    var parameter="";  
    function test(parameter){  
        //return 真值  
        return true;  
    }  
      
    //真值操作  
    function operate1(parameter){  
        return "真值操作";  
    }  
      
    //假值操作  
    function operate2(parameter){  
        return "假值操作";  
    }  
      
    var result=test(parameter)&&operate1(parameter);  
    result=test(parameter)||operate2(parameter);  
      
    //等价于  
    result=test(parameter)?operate1(parameter):operate2(parameter);  
      
    alert(result);//真值操作  
      
    //也等价于  
    if(test(parameter)){  
        result=operate1(parameter);  
    }else{  
        result=operate2(parameter);  
    }  
      
    alert(result)//真值操作  
