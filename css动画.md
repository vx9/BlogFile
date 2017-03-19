# css3实现打字效果

用到是属性有：

animation:

transform:

transition:

组合情况：

transform+transition:

animation+transform:

animation:

1）animation-name
2）animation-duration
3）animation-timing-function
4）animation-delay
5）animation-iteration-count
6）animation-direction

最重要的就是animation-name，它规定了需要绑定的@keyframes名称。keyframes，我们用来定义几个关键节点帧。

实现打字效果：

```html
<p class="welcome">你好啊美女</p>
```

### 1.css方法：

#### animation:

注意`steps(number_of_steps, [start|end])` 阶跃函数

指的是动画一帧一帧的走，而不是连续的，如果连续了就看不到一个字一个字打印出来的效果了。

> W3C的解释：steps 函数指定了一个阶跃函数，第一个参数指定了时间函数中的间隔数量（必须是正整数）；第二个参数可选，接受 start 和 end 两个值，指定在每个间隔的起点或是终点发生阶跃变化，默认为 end。http://www.cnblogs.com/BATAKK/p/5301819.html

`number_of_steps`：该动画要走几步（帧），

`[start|end]`：end（默认值）是第一帧开始前 显示动画。start是第一帧结束后 显示动画。

> 打字效果的话，要让走的每一步宽度与字体宽度相同。按照下列书写来看，字体宽度是10px，有5个字，要走5步，那总体宽度就应该是50。

```css
.welcome{
            width:50px;
            font-size:10px;
            white-space:nowrap;/*强制一行显示*/
            overflow:hidden;/*溢出隐藏*/
  			/*如果没有以上注释两项，则效果非预期*/
            -webkit-animation: type 3s steps(5, end) ;
            animation: type 3s steps(5, end) ;
        }
@keyframes type{
            from{
                width:0;
            }
        }
```



#### transition:

不用计算了，就是看看试试时间怎么弄合适，

```css
.welcome{
            width:0;
            font-size:10px;
            white-space:nowrap;/*强制一行显示*/
            overflow:hidden;
            transition:all 2s linear 2s;
        }
.welcome_move{
            width:200px;
        }
```

```javascript
<script>
        $(function(){
        function show(){
            alert("hi");
                $(".welcome").addClass("welcome_move");
            }
            show();
        });
</script>
```

### 2.js方法

css什么都可以不用写，只写js即可（用jquery）

```javascript
$(function(){
            var $welcome =  $(".welcome");
            function show1() {
                var s = $welcome.text();
                var index = 0;

                $welcome.text('');
                function y() {
                    if (index < s.length) {
                        $welcome.append(s.charAt(index));
                        setTimeout(y, 100);
                    } else {
                        alert("done");
                    }
                    index++;
                }
                setTimeout(y, 500);
            }
            show1();
        });
```



用transiton还是animation？

按效果来说，





