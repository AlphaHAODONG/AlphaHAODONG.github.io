# Js


  想了想还是决定简单了解一下，暂时不动前端的蛋糕了，毕竟这两年前端工作也不大好找.......

网页三剑客

- html:网页布局
- css:网页渲染的
- js:网页动态特效的

js但是浏览器直接解析执行，不需要编译。js和java没有一毛钱关系。

js直接写在html页面。

# 一、js入门

## js 代码

- js代码出现在html任何位置

- js代码自上而下的执行。前面的代码没法获取到后面的标签。（js写在后面）

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>js入门案例</title>
      <!--js代码需要用<script>-->
      <script>
          //弹框
          // alert("哈哈哈head1");
      </script>
  </head>
  <!--js代码需要用<script>-->
  <script>
      /*执行顺序的问题。*/
      var div1 = document.getElementById("div1");
      alert(div1);
      //弹框
      // alert("哈哈哈head2");
  </script>
  <body>
      <div id="div1">div</div>
  <!--js代码需要用<script>-->
  <script>
      //弹框
      // alert("哈哈哈body");
  </script>
  </body>
  <!--js代码需要用<script>   常用的-->
  <script>
      //弹框
      // alert("哈哈哈");
  </script>
  
  </html>
  ```

## 1.2js引入方式

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

</body>
<!--src：脚本的路径
    这个标签不可以自结尾-  早期版本的一个BUG
    这个标签里面不要写js代码
-->
<script src="js/scripts.js"></script>

<script>
alert("内部方式")
</script>
</html>
```



js脚本js/scripts.js

```js
alert("黑嘿嘿嘿外部引用");
```



# 二、js基础语法

## 2.1 语法  注释  注意事项、输出语句

```html
<script>
    /*多行注释*/
    //单行注释
    <!-- 单行注释 -->
    /*js中语句结束的;可以省略*/
    /*js中window调用的方法，window对象名可以省略*/
//    2.输出语句
//     2.1在页面上输出
    document.write("我是页面输出语句，嘿嘿嘿")
//    2.2弹框输出（测试）- 死亡弹框
//     alert("我是死亡弹框测试")
    window.alert("我是死亡弹框测试")
//    2.3控制台输出
    console.log("我是控制台日志输出")
</script>
```

## 2.2 变量

```html
<script>
// 定义变量
    var a = 1;
    var b = 1.1;
    var c = "我是帅哥";
    var d = 'a';
    // alert(a);
    // alert(b);
// alert(d);
// alert(c);
{
    var e = 10;//全局变量  js5
    let a1 = 1;//局部变量  js6
}
    // alert(e);
    // alert(a1);

//    js6常量
    const PI = 3.14;
    // PI=3.1415;//常量不能修改
    var a =20;//变量名使用两次，就是覆盖
    alert(a);

</script>
```



## 2.3 数据类型

number、string、boolean 、null  、undefined

```html
<script>

// 5种数据类型
    var n = 10;
    console.log(typeof n);//number
    var s = "黑呵呵";
    console.log(typeof s);//string
    var b = true;
    console.log(typeof b);//boolean
    var obj = null;
    console.log(typeof obj);//object  null是一个占位符
    var a;
    console.log(typeof a);//undefined  未初始化，默认值就是undefined
</script>
```



## 2.4 运算符

* 一元运算符：++，--
* 算术运算符：+，-，*，/，%
* 赋值运算符：=，+=，-=…
* 关系运算符：>，<，>=，<=，!=，\    ==，===…
* 逻辑运算符：&&，||，!
* 三元运算符：条件表达式 ? true_value : false_value 



```html
<script>

//  ++操作
    var a1 = 10;
    a1++;
    console.log("a1="+a1);

//   三元运算符
    var b1 = false;
    b1?console.log("我是true"):console.log("我是false");
//    ==  和 ===
    var a2 = 10;
    var s2 = "10";
    console.log(a2 == s2);//true  == 只比较值，不比较类型，类型强转
    console.log(a2 === s2);//false  === 比较值和类型

//    类型强转问题
//     1 其他类型-》number
    var s3 = +"100";// string->number
    console.log(s3+1);//101
    var s4 = "100";
    console.log(parseInt(s4)+1);//parseInt(s4) string->number

    var b3 = false;// 布尔值转化为number后，true  1   false  0
    console.log(b3+1);

    var obj3 = null;//不考虑类型转化问题
    console.log(obj3+1);


//    2. 其他类型-》布尔值
//     2.1 字符串---boolean
//             空字符串是false   有内容就是true
    var s5 = "";
    s5?console.log("==true==="):console.log("===false==");
//    2.2  number->boolean
//             0是false,  其他都是 true
    var n5 = 0;
    n5?console.log("=n5=true==="):console.log("=n5==false==");
//    2.3 undefined   null->boolean
//                         flase
    var un = null;
    un?console.log("=un=true==="):console.log("=un==false==");
    
    // 演示一个NaN
    var str1 = +"一百";//NaN
    var str2 = +"一百";
    console.log(str1);
</script>
```

总结类型转换：程序更加健壮

- 其他类型-》number
  - string->number      
    - +“100”；
    - paramInt(“100")
    - NaN
- **其他类型-》boolean**
  - 字符串-》boolean
    - 空字符串----- false
    - 有内容--------true
  - number->boolean
    - 0-------false
    - 其他都是true
  - undefined   null ---------   false

## 2.5  流程控制语句

JavaScript 中提供了和 Java 一样的流程控制语句，如下

* if 
* switch
* for
* while
* dowhile

#### 2.5.1  if 语句

```js
var count = 3;
if (count == 3) {
    alert(count);
}
```

#### 3.6.2  switch 语句

```js
var num = 3;
switch (num) {
    case 1:
        alert("星期一");
        break;
    case 2:
        alert("星期二");
        break;
    case 3:
        alert("星期三");
        break;
    case 4:
        alert("星期四");
        break;
    case 5:
        alert("星期五");
        break;
    case 6:
        alert("星期六");
        break;
    case 7:
        alert("星期日");
        break;
    default:
        alert("输入的星期有误");
        break;
}
```

#### 2.5.3  for 循环语句

```js
var sum = 0;
for (let i = 1; i <= 100; i++) { //建议for循环小括号中定义的变量使用let
    sum += i;
}
alert(sum);
```

#### 2.5.4  while 循环语句

```js
var sum = 0;
var i = 1;
while (i <= 100) {
    sum += i;
    i++;
}
alert(sum);
```

#### 2.5.5  dowhile 循环语句

```js
var sum = 0;
var i = 1;
do {
    sum += i;
    i++;
}
while (i <= 100);
alert(sum);
```

### 

## 2.6 函数

```html
<script>

    //函数方式一  function 函数名(变量1名,变量2名...){ 代码;  返回值  }
    // public int sum(int i,int j){ return x}
    function sum(i,j) {
        var s = i+j;
        return s;
    }

//    函数方式二   匿名函数方式      变量 = function(参数){代码}  不能额外调用
    var sum1 = function(i,j){
        alert(i+j);
    }
    
//    函数调用
    var sum1 = sum(2,3);
    alert(sum1);
</script>
```

## 2.7 定义对象

```html
<script>
// 字面量方式定义对象(属性    放法)
    var obj = {
        name:"张三",
        age:23,
        save:function (i) {
            alert("我今天吃饭了"+i)
        }
    }
    alert(obj.name)
    alert(obj.age)
    obj.save("大米饭")
</script>
```



## 2.8.1 API-Array(数组)

```js
    /*数组*/
    // 1.获取一个数组
    var arr1 = ["a","b","c"];
    var arr3 = ["a2","b2","c2"];
//    2.获取一个数组
    var arr2 = new Array("a","b","c");

//    获取
//     alert(arr1[1]);
//     alert(arr2[0]);
//    遍历1 fori
//     for (let i = 0; i < arr1.length; i++) {
//         console.log(arr1[i]);
//     }
//    遍历2 增强forof        arr1.forof
//     for (let s of arr1) {
//         console.log(s)
//     }
//    遍历3 forEach
//     arr1.forEach(function (item,index) {
//         console.log(item+"的索引为="+index)
//     })

//    concat() 连接两个或多个数组，并返回已连接数组的副本。
    var concat = arr1.concat(arr2,arr3);
    console.log(concat);

//    pop()    删除数组的最后一个元素，并返回该元素。
//     var s = arr1.pop();
//     console.log("s="+s);
//     for (let s of arr1) {
//         console.log(s)
//     }
//    push()   将新元素添加到数组的末尾，并返回新的长度。
//     var number = arr1.push("d","e");
//     for (let s of arr1) {
//         console.log(s)
//     }
//     console.log(number)

//    splice() 从数组中添加/删除元素
//             index  （必须）开始索引
//             deletemay  （可选）删除的个数
//             item...   （可选）新增的个数
    var arr = ["a","b","c"];
//            把1索引的元素替换为e
    arr.splice(1,1,"e","到");
    console.log(arr);
```





## 2.9Date

```js
//https://www.w3school.com.cn/jsref/jsref_obj_date.asp
    var d = new Date();
    console.log(d)
    var d1 = new Date("2022/02/02");
    console.log(d1)
    var d2 = new Date(0);//1970
    console.log(d2)
//    获取年
    var fullYear = d.getFullYear();
    console.log(fullYear)
    var month = d.getMonth();// month是月份 的索引  0-11 代表月份
```



## 2.10String

```js
// 获取一个字符串
    var s = "abc";
    var s2 = new String("嘿嘿嘿");

//    获取字符串的长度
    console.log(s.length)

//    charAt() 返回在指定位置的字符。
    var charAt = s.charAt(0);
    console.log(charAt)

//    concat() 连接字符串。
    var c = s.concat(s2);
    console.log(c)

//    split()  把字符串分割为字符串数组。
    var hoppy = "打篮球、踢足球、乒乓球";
    var split = hoppy.split("、");
    for (let str of split) {
        console.log(str)
    }

//substring()  提取字符串中两个指定的索引号之间的字符。
    var hoppy1 = "打篮球、踢足球、乒乓球";
    var substring = hoppy1.substring(4,7);//[)
    console.log(substring)

//    trim()    去掉字符串两端的空格
    var str = "   root  ";
    var s1 = str.trim();
    console.log(s1)
```



## 2.11json

```js
//定义json数据  {"键":"值","键":"值","键":"值"}
    var myObj = { "name":"Bill", "age":19, "city":"Seattle" };
    //获取数据
    console.log(myObj.name)
    //定义json数据  多个
    var myObj1 = [
        { "name":"小明", "age":19, "city":"北京" },
        { "name":"小王", "age":19, "city":"上海" }
    ]
//    获取数据
    console.log(myObj1[0].name)


//    parse()  解析 JSON 字符串->json
    var str = "{\"name\":\"小明\",\"age\":19}";
    var obj = JSON.parse('{"firstName":"Bill", "lastName":"Gates"}');
    var p = JSON.parse(str);
    console.log(p.name);

//     stringify() 将 JavaScript 对象转换为 JSON 字符串。
    var s = JSON.stringify(p);
    console.log(typeof s)
```



# 三、bom

 BOM 中包含了如下对象：

* Window：浏览器窗口对象
* Navigator：浏览器对象
* Screen：屏幕对象
* History：历史记录对象
* Location：地址栏对象

下图是 BOM 中的各个对象和浏览器的各个组成部分的对应关系

<img src="D:/讲义/assets/image-20210815194911914.png" alt="image-20210815194911914" style="zoom:70%;" />



## 3.1window

```js
    //window对象
// window.innerHeight - 浏览器窗口的内高度（以像素计）
// window.innerWidth - 浏览器窗口的内宽度（以像素计）
    var innerHeight = window.innerHeight;
    var innerWidth = window.innerWidth;
    console.log(innerHeight)
    console.log(innerWidth)
//    响应式的时候很常用！！！

//    1弹框  确定框
    window.alert("你爱我吗")
//    2确定取消框  参数就是弹框提示的内容.  返回值就是点确定返回true 点取消返回false
    var b = window.confirm("你爱我吗");
    // console.log(b)
//    3.定时器 一次定时器 setTimeout(fun,毫秒值)
    var time1 = window.setTimeout(function () {
        alert("过了3秒")
    },3000)
    //清除一次定时器
    window.clearTimeout(time1)

//    4.多次定时器 setInterval(fun,毫秒值)
    var time2 = window.setInterval(function () {
        alert("过3秒弹一次")
    },3000)

//  清除多次定时器
    window.clearInterval(time2)
```



## 3.2 location

```js
//location  href  页面跳转
window.location.href = "http://www.itcast.cn";
```

## 3.3 history

```js
//    回退
    window.history.back()
```







# 四、dom

DOM：Document Object Model 文档对象模型。也就是 JavaScript 将 HTML 文档的各个组成部分封装为对象。

DOM 其实我们并不陌生，之前在学习 XML 就接触过，只不过 XML 文档中的标签需要我们写代码解析，而 HTML 文档是浏览器解析。封装的对象分为

* Document：整个文档对象
* Element：元素对象
* Attribute：属性对象
* Text：文本对象
* Comment：注释对象

如下图，左边是 HTML 文档内容，右边是 DOM 树

![image-20210815231028430](D:\作业\img\image-20210815231028430.png)

**作用：**

JavaScript 通过 DOM， 就能够对 HTML进行操作了

* 改变 HTML 元素的内容
* 改变 HTML 元素的样式（CSS）
* 对 HTML DOM 事件作出反应
* 添加和删除 HTML 元素

```js
<body>
    <img id="light" src="../imgs/off.gif"> <br>

    <div class="cls">山东高合   <span>123</span></div>   <br>
    <div class="cls">山东高合</div> <br>

    <input type="checkbox" name="hobby"> 电影
    <input type="checkbox" name="hobby"> 旅游
    <input type="checkbox" name="hobby"> 游戏

<script>
    /*获取元素标签*/
    //1. 通过ID获取，获取一个标签
    var light = document.getElementById("light");
    // alert(light);
//    2. 通过class获取， 元素数组
    var cls = document.getElementsByClassName("cls");
    // alert(cls.length);
//    3. 通过标签获取，拿到的是元素数组
    var divs = document.getElementsByTagName("div");
    // alert(divs.length)
//    4. 通过name获取   元素数组
    var inps = document.getElementsByName("hobby");
    // alert(inps.length);

    /*获取属性*/
    /*!tips:  元素数组是没有办法直接拿属性和内容的。精确的哪一个上面 */
    // var t = inps[0].type;
    // console.log(t)
//    把高合信息改成红色字体
    cls[0].style.color = "red";
//    把旅游输入框改成默认选项
    inps[1].checked = true;

    /*获取文本内容*/
    // innerHTML  innerText
    var innerHTML1 = cls[0].innerHTML;//innerHTML 这个元素里的包含html的内容
    var innerText1 = cls[0].innerText;//innerText  这个元素里的文本内容（没有标签）
    // alert(innerHTML1)
    // alert(innerText1)
    // cls[0].innerHTML = "<h1>山东高合</h1>";//这个会解析标签
    cls[0].innerText = "<h1>山东高合</h1>";//这个不会

</script>
```



# 五、事件

### 7.2  常见事件

上面案例中使用到了 `onclick` 事件属性，那都有哪些事件属性供我们使用呢？下面就给大家列举一些比较常用的事件属性

| 事件属性名  | 说明                     |
| ----------- | ------------------------ |
| onclick     | 鼠标单击事件             |
| onblur      | 元素失去焦点             |
| onfocus     | 元素获得焦点             |
| onload      | 某个页面或图像被完成加载 |
| onsubmit    | 当表单提交时触发该事件   |
| onmouseover | 鼠标被移到某元素之上     |
| onmouseout  | 鼠标从某元素移开         |

```html
<body>
<button id = "btn1" onclick="on()">点击了1（了解即可）</button>
<button id = "btn2">点击了2</button>
<br>
<input type="text" value="123" id="inp">
<br>
<div id="div1">我是最棒的，你一划过就会变红色</div>
</body>
<script>
    /*方式一:侵入式开发 了解*/
    function on() {
        alert("我被点击了")
    }
    /*方式二：监听式*/
    var btn2 = document.getElementById("btn2");
    btn2.onclick = function () {
        alert("你点我干啥")
    }

    /*演示onblur  元素失去焦点*/
    document.getElementById("inp").onblur = function () {
        //获取输入框里面的值是value属性
        var value = document.getElementById("inp").value;
        alert(value)
    }

    /*演示onmouseover鼠标被移到某元素之上*/
    document.getElementById("div1").onmouseover = function () {
        document.getElementById("div1").style.color = "red";
    }

</script>
```





# 六、案例

略

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>欢迎注册</title>
    <link href="css/register.css" rel="stylesheet">
</head>
<body>

<div class="form-div">
    <div class="reg-content">
        <h1>欢迎注册</h1>
        <span>已有帐号？</span> <a href="#">登录</a>
    </div>
    <form id="reg-form" action="#" method="get">

        <table>

            <tr>
                <td>用户名</td>
                <td class="inputs">
                    <input name="username" type="text" id="username">
                    <br>
                    <span id="username_err" class="err_msg" style="display: none">用户名不太受欢迎</span>
                </td>

            </tr>

            <tr>
                <td>密码</td>
                <td class="inputs">
                    <input name="password" type="password" id="password">
                    <br>
                    <span id="password_err" class="err_msg" style="display: none">密码格式有误</span>
                </td>
            </tr>


            <tr>
                <td>手机号</td>
                <td class="inputs"><input name="tel" type="text" id="tel">
                    <br>
                    <span id="tel_err" class="err_msg" style="display: none">手机号格式有误</span>
                </td>
            </tr>

        </table>

        <div class="buttons">
            <input value="注 册" type="submit" id="reg_btn">
        </div>
        <br class="clear">
    </form>

</div>


<script>
    // 1.校验用户名
//    1.1获取用户名输入框元素
    var usernameInput = document.getElementById("username");
//    1.2各元素添加事件---失去焦点事件
    var usernameReg = false;
    usernameInput.onblur = function () {
//    1.3获取用户输入的信息(顺带把输入的前后空格干掉)
        var username = usernameInput.value.trim();

//    1.4判断这个信息满足要求不(6-12个字符)  满足就OK  不满足就提示
//         请输入6-12个字符的字符串，由字母数字组成
        var arg2 = /^[a-zA-Z0-9]{6,12}$/;
        if(arg2.test(username)){
        //    满足条件
            document.getElementById("username_err").style.display = "none";
            usernameReg = true;
        }else {
        //    不满足
            document.getElementById("username_err").style.display = "block";
        }
    }

    // 2.校验密码框
    //    1.1获取用户名输入框元素
    var passwordInput = document.getElementById("password");
    //    1.2各元素添加事件---失去焦点事件
    var passwordReg = false;
    passwordInput.onblur = function () {
//    1.3获取用户输入的信息(顺带把输入的前后空格干掉)
        var password = passwordInput.value.trim();

//    1.4判断这个信息满足要求不(6-12个字符)  满足就OK  不满足就提示
        var arg2 = /^[a-zA-Z0-9]{6,12}$/;
        if(arg2.test(password)){
            //    满足条件
            document.getElementById("password_err").style.display = "none";
            passwordReg = true;
        }else {
            //    不满足
            document.getElementById("password_err").style.display = "block";
        }
    }


    // 3.校验手机号
    //    1.1获取用户名输入框元素
    var telInput = document.getElementById("tel");
    //    1.2各元素添加事件---失去焦点事件
    var telReg = false;
    telInput.onblur = function () {
//    1.3获取用户输入的信息(顺带把输入的前后空格干掉)
        var tel = telInput.value.trim();

//    1.4判断这个信息满足要求不(11个字符)  满足就OK  不满足就提示
        var arg3 = /^1[35789]{1}[0-9]{9}$/;
        if(arg3.test(tel)){
            //    满足条件
            document.getElementById("tel_err").style.display = "none";
            telReg = true;
        }else {
            //    不满足
            document.getElementById("tel_err").style.display = "block";
        }
    }

    /*监听表单提交事件*/
    // 1.获取表单提交按钮  --  监听的是表单
    var regForm = document.getElementById("reg-form");

//      2.加监听事件
    regForm.onsubmit = function () {
        // alert("注册了")
        //上面三个条件都满足，就提交
        if(usernameReg && passwordReg && telReg){
            return true;
        }

    //   return true 提交表单  return  false   不提交
        return false;

    }
//    3. 加内容


</script>
</body>
</html>
```





# 总结：

- js引入方式
  - 内部方式
  - 外链式（框架）
- 基本语法
  - 大多数和java样
  - 定义变量var
  - 类型自动转换   其他类型-》boolean
  - API
    - 数组
    - Date
    - String
    - json
    - regexp
- bom操作
  - 窗口对象
  - 历史对象
  - 地址栏对象
- dom解析
  - 如何获取元素、操作
  - 如何获取属性、操作
  - 如何获取内容、操作
- 事件
  - 监听事件！！！
