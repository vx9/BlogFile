[TOC]

### 第一题：

给定数组，求项相加。

```javascript
<script type ="text/javascript">
     //声明一个函数，求任意个数的和，测试你声明的函数
    document.write (sum(2,3 ,7)+"<br />" );
    document.write (sum(2,3 ,7,4,3 ,1)+"<br />" );
    document.write (sum("Hello" ," ","Tom" )+"<br />");
    
     function sum () {
         var result = 0;
         //函数实际调用执行时传入的参数，可以从 arguments伪数组中获取
         for(var i = 0 ; i < arguments.length ; i++) {
            result += arguments [i];
         }
         return result;
    }
</script>

```

>sum这个方法不理想，与字符串相加例子结果非预期。第三个变成：0Hello Tom，其他数字正常。
>
>**因为字符串与数字相加时，会默认把2非字符串变成字符串再进行字符串拼接。**



```javascript
 function sum () {
         var result = '';
         //函数实际调用执行时传入的参数，可以从 arguments伪数组中获取
         for(var i = 0 ; i < arguments.length ; i++) {
            result += arguments [i];
         }
         return result;
    }
```

> sum这个方法不理想，与数字类型相加结果非预期，会变成字符串拼接。
>
> result初始化为null也不行，与数字类型可以（因为后台**转换null转成0**），与字符串相加例子不过，nullHello Tom。
>
> result只声明不赋值也不行（就是undefined），上述例子都不过，数字类型就是NaN（因为在后台转换时，**undefined就转成NaN**，他无法转成数字类型），字符串就是undefinedHello Tom。 



#### 正解一：

```javascript
 
function sum () {    
	var result = arguments[0];
     //函数实际调用执行时传入的参数，可以从 arguments伪数组中获取
     for(var i = 1 ; i < arguments.length ; i++) {
        result += arguments [i];
     }
     return result;
}
```

#### 正解二：arr.reduce()

```javascript
        function sum(arr){
            return arr.reduce(function(pre,cur,index,arr){
                return pre+cur;
            });
        }

        var arr = ["Tom","Mary"];
        document.write(sum(arr)+"<br>");
        arr = [2,3,4];
        document.write(sum(arr));
```

### 第二题：

arr =[1,2,3,4,5,6,7,8,9]
从数组每次取出3个数    如：123  456  789  123  456  789依次**循环**下去。

#### 方法一：

将数组每三项z作为一项存入新数组中，再循环打印（用setTimeout超时调用打印，不推荐用setInterval间歇调用打印）

> 实际应用中最好不用setInterval，因为后一次执行可能会在前一个结束之前启动。

```javascript
var arr = [1, 2, 3, 4, 5, 6, 7, 8,9];
        var newArr = [];

        for (var i = 0; i < arr.length; i++) {
            if(i%3 == 0 ) {
                if( i+3 < arr.length){
                    newArr.push(arr.slice(i,i+3));
                }else{//这样做是为了如果数组不是3的倍数，也可以打印出来。arr = [1, 2, 3, 4, 5, 6, 7, 8],但不是循环;是123 456 781 123 456 ...
                    newArr.push(arr.slice(i,arr.length).concat(arr.slice(0,i+3-arr.length)));
                }
            }
        }

        var num = 0, max = 3;

        function type() {
            num++;
            newArr.forEach(function(item,index,newArr) {
                console.log(num+'次 '+item.join(''));
            });
            if(num < max){
                setTimeout(type ,500);
            }else{
                alert('done');
            }
        }
        setTimeout(type,500);
```

上面代码也提到了如上做法有问题，当arr = [1, 2, 3, 4, 5, 6, 7, 8],是123 456 781 123 456 ...

#### 正解:队列

利用队列先进先出的特征，把数组元素依次放进去，每次取三个，依次放回去。循环就可以了。

```javascript
var arr = [1, 2, 3, 4, 5, 6, 7, 8], newStr='',num = 0, max = 3;

function loop() {
    num++;
    newStr = '';

    for(var i=0;i<3;i++){
        var item = arr.shift();
        newStr +=item;
        arr.push(item);
    }
    console.log(num+'次 '+newStr);
    if(num > max) {
        alert('done');
    }else {
        setTimeout(loop,500);
    }
}

setTimeout(loop,500);
```

当arr = [1, 2, 3, 4, 5, 6, 7, 8],是123 456 781 234 678 123 456 781 ...



