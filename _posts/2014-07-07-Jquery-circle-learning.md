---
layout: post
title:  "Jquery中的循环技巧"
date:   2014-07-07 13:25:07
categories: jquery
---

###1. 简单的for-in（事件）  
    for ( type in events ) {  
      
    }

### 2. 缓存length属性，避免每次都去查找length属性，稍微提升遍历速度  
 但是如果遍历HTMLCollection时，性能提升非常明显，因为每次访问HTMLCollection的属性，HTMLCollection都会内部匹配一次所有的节点
 
    for ( var j = 0, l = handlers.length; j < l; j++ ) {  
      
    } 

###3. 不比较下标，直接判断元素是否为true（强制类型转换） 

    var elem;  
    for ( var i = 0; elems[i]; i++ ) {  
        elem = elems[i];  
        // ...  
    } 

###4. 遍历动态数组（事件），不能缓存length属性，j++之前先执行j--，保证不会因为数组下标的错误导致某些数组元素遍历不到  
    for ( j = 0; j < eventType.length; j++ ) {  
        eventType.splice( j--, 1 );  
    }  
    for ( var i = 1; i < results.length; i++ ) {  
        if ( results[i] === results[ i - 1 ] ) {  
            results.splice( i--, 1 );  
        }  
    }

###5. 迭代过程中尽可能减少遍历次数（事件），如果你能知道从哪里开始遍历的话，这里是pos  
    for ( j = pos || 0; j < eventType.length; j++ ) {  
      
    }

###6. 倒序遍历（事件），减少了几个字符：循环条件判断，合并i自减和i取值，倒序遍历会有浏览器优化，稍微提升遍历速度  
    for ( var i = this.props.length, prop; i; ) {  
        prop = this.props[ --i ];  
        event[ prop ] = originalEvent[ prop ];  
    }

###7. 倒序遍历，中规中矩，倒序会有浏览器优化，稍微提升遍历速度  
    for ( j = tbody.length - 1; j >= 0 ; --j ) {  
        if ( jQuery.nodeName( tbody[ j ], "tbody" ) && !tbody[ j ].childNodes.length ) {  
            tbody[ j ].parentNode.removeChild( tbody[ j ] );  
        }  
    }

###8. 不判断下标，直接判断元素（选择器）  
    for ( i = 0; checkSet[i] != null; i++ ) {  
        if ( checkSet[i] && (checkSet[i] === true || checkSet[i].nodeType === 1 && Sizzle.contains(context, checkSet[i])) ) {  
            results.push( set[i] );  
        }  
    }  
    for ( ; array[i]; i++ ) {  
        ret.push( array[i] );  
    }

###9. 不判断下标，取出元素然后判断元素（选择器）  
    for ( var i = 0; (item = curLoop[i]) != null; i++ ) {  
      
    }

###10. 遍历DOM子元素  
    for ( node = parent.firstChild; node; node = node.nextSibling ) {  
        if ( node.nodeType === 1 ) {  
            node.nodeIndex = ++count;  
        }  
    }

###11. 动态遍历DOM子元素（DOM遍历），dir参数表示元素的方向属性，如parentNode、nextSibling、previousSibling、lastChild和firstChild  
    for ( ; cur; cur = cur[dir] ) {  
        if ( cur.nodeType === 1 && ++num === result ) {  
            break;  
        }  
    }

###12. while检查下标i  
    var i = promiseMethods.length;  
    while( i-- ) {  
        obj[ promiseMethods[i] ] = deferred[ promiseMethods[i] ];  
    }

###13. while检查元素  
    while( (type = types[ i++ ]) ) {  
      
    }

###14. while遍历动态数组（AJAX），总是获取第一个元素，检查是否与特殊值相等，如果相等就从数组头部移除，直到遇到不相等的元素或数组为空  
    while( dataTypes[ 0 ] === "*" ) {  
        dataTypes.shift();  
        if ( ct === undefined ) {  
            ct = s.mimeType || jqXHR.getResponseHeader( "content-type" );  
        }  
    }

###15. while遍历动态数组（异步队列），总是获取第一个元素，直到数组为空，或遇到值为undefined的元素  
    while( callbacks[ 0 ] ) {  
        callbacks.shift().apply( context, args );  
    }

###16 while反复调用RegExp.exec（AJAX），能够否反复调是exec比re.test、String.match更加强大的原因，每次调用都将lastIndex属性设置到紧接着匹配字符串的字符位置  
    while( ( match = rheaders.exec( responseHeadersString ) ) ) {  
        responseHeaders[ match[1].toLowerCase() ] = match[ 2 ]; 
        // 将响应头以key-value的方式存在responseHeaders中  
    }