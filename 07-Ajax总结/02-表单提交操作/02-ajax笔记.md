# ajax02

## 机器人案例

+ 需求:

    1. 把用户说的内容渲染到页面并发送给服务器,服务器返回机器人的回复
    2. 把服务器返回的数据渲染到页面并再次发送给服务器，服务器返回一条语音数据
    3. 把语音数据渲染到网页

+ 步骤
  1. 导入样式布局，调用封装好的滚动条。
  2. 给发送按钮绑定点击事件，然后获取用户输入的内容，把它发送给  服务器接口 并渲染到页面，然后将服务器返回的内容渲染到页面。
  3. 再次将服务器返回的内容 发送 到语音接口，服务器会返回一条语音数据，将语音展示到页面

+ 接口

    > http://www.liulongbin.top:3006

    1. 获取机器人回应的内容

       - 接口URL：  /api/robot
       - 调用方式： GET

       | 参数名称 | 参数类型 | 是否必选 | 参数说明                         |
       | -------- | -------- | -------- | -------------------------------- |
       | spoken   | String   | 是       | 用户输入的内容(要跟机器人说的话) |

    2. 将文字转语音

       - 接口URL：  /api/synthesize
       - 调用方式： GET
       - 参数格式：

       | 参数名称 | 参数类型 | 是否必选 | 参数说明 |
       | -------- | -------- | -------- | -------- |
       | text     | String   | 是       |          |

    

    

    

## 表单相关操作

1. form标签可使用按钮提交，input中的sumbit和button按钮有默认提交行为,input的button没有默认提交行为
2. form的默认提交方式是GET
3. form默认提交会刷新页面
4. form常用的输入域input
5. 输入域中的name属性是必要的，且严格和后台相同

```js
<form>  method="提交方式"  get,post
		action="提交路径"	
        target="打开新页面"	_self _back
        enctype =""	默认是application/x-www-form-urlencoded
        			如果需要文件上传要修改:multipart/form-data
    <progress value="50" max="100"> //进度条标签，value是百分之多少，max是最大
</form>
```



## 基于ajax的提交

### 使用sumbit事件提交

```js
$('form').submit(function(e){ //submit事件，当表单提交时触发
    e.preventDefault() //阻止表单的默认行为
})
```

### 使用点击事件提交

+ 这种方法使用input的按钮也可以使用

```js
$('#btn').click(function(e){ //submit事件，当表单提交时触发
   var formData =	$('form').serialize()	//使用serialize()获取所有表单输入域的数据,输入域必须有name
})
```



## 案例

+ 需求：

  1. 获取评论列表，且渲染到页面
  2. 点击发表评论，提交数据

+ 接口：

  > http://www.liulongbin.top:3006

  1. 列表接口

     - 接口URL：  /api/cmtlist
     - 调用方式： GET
     - 参数格式：无
     - 响应格式：

     | 数据名称  | 数据类型 | 说明                     |
     | --------- | -------- | ------------------------ |
     | status    | Number   | 200 成功；500 失败；     |
     | msg       | String   | 对 status 字段的详细说明 |
     | data      | Array    | 评论列表                 |
     | +id       | Number   | 评论Id                   |
     | +username | String   | 评论人姓名               |
     | +content  | String   | 评论内容                 |
     | +time     | String   | 评论时间                 |

  2. 发表评论

     - 接口URL：  /api/addcmt
     - 调用方式： POST
     - 参数格式：

     | 参数名称 | 参数类型 | 是否必选 | 参数说明   |
     | -------- | -------- | -------- | ---------- |
     | username | String   | 是       | 评论人名称 |
     | content  | String   | 是       | 评论内容   |





## 模板引擎

+ 使用模板引擎，可读性会变好，方便后期维护

### art-template库介绍

+ art-template是一个简约，超快的模板引擎，采用作用域预声明的技术优化模板渲染速度，从而获得接近javascript极限运行性能，并同时支持服务器和浏览器。
+ template-web 是浏览器渲染，npm install art-template --save 是服务端渲染

### 浏览器中使用

1. 导入template-web.js

2. 定义script标签

   ```js
   <script type='text/html' id='自定义'>
       
   </script>
   ```

3. 准备数据

   ```js
   res = {
       data:[
           {name:'小白',age:18},
           {name:'小黑',age:18},
           {name:'小黄',age:18},
       ]
       status:200
   }
   ```

4. 将数据填充到模板中

   ````js
   var temp = template("id",res);
   //temp的返回值是一个静态的html模板
   ````

5. 将temp渲染到页面

   ```js
   ul.innerHTML = temp
   ```

## 语法

1. 循环

   ```js
   {{each data}}
   	{{$index}} //索引
   	{{$value}} //值
   {{/each}}
     
    //将res传进来后，循环data
   ```

2. 条件判断

   ```js
   {{ if name == '小白' }}
   	{{'true'}}
   {{else}}
    	{{'否则false'}}
    {{/if}}
   ```

3. 过滤器

   ```js
   {{ name || formName}} //使用过滤器
   
   <script>
       //定义过滤器
       template.dafaults.imports.formName = function(name){
       	return name
   	}
   </script>
   ```



## 回调函数

### 函数的特性

1. 函数是一个对象。
2. 函数可以存储在变量中
3. 函数可以做为函数的参数传递。
4. 函数可以在函数内部创建
5. 函数可以在函数中返回

###回调函数的作用

1. 作用
   + js代码会从上到下一行行执行，但有时候我们需要等到一个特定的操作结束后，再去执行下一个操作，这时候就需要回调函数
2. 定义
   + 当函数b作为参数传入函数a中，且a内部执行b，那么这个b就是回调函数

### 高阶函数

+ 满足条件
  1. 接受一个或多个函数作为输入
  2. return 返回另外一个函数

如果一个函数的参数是函数，或者返回值是函数，那么该函数就是高阶函数。


```js
//示例
function fun(fun2,value){
	fun2(value)
}
function fun2(a){ 
    console.log(a) //123
}

fun(fun2,123)//fun是高阶函数，fun2是回调函数
```



## new知识点

1. 进度条标签

   ```html
    <progress value="50" max="100"> 
    //进度条标签，value是百分之多少，max是最大
   ```

2. serialize()

   - jq中的方法

   ```js
       e.preventDefault() //阻止表单的默认行为
   var form = $('form').serialize()
   //获取表单所有输入域的数据，必须有name，返回值是字符串形式用&号隔开的键值对
   ```

3. 重置表单

   ```js
   document.getElementById("form").reset() //是原生方法,重置表单
   ```


## 单词

```js
serialize  //序列化
progress //进度
```





## ?

1. attribute和property？

   + 自定义属性包含在attributes对象中，其他的内置属性都是property

   + attributes是THML标签上的特新，它的值只能够是字符串

     设置节点用setAttribute

     删除用removeAttribute

     查看用getAttribute

   + property是DOM中的属性，是js里的对象

2. prop() 和 attr()？

   获取 DOM节点固有属性用prop()

   获取DOM自定义节点用attr()

3. 接口文档中的+id是什么意思





