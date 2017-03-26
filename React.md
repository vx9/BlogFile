# React



参考：

[https://segmentfault.com/a/1190000003691119](javascript:openUrl('https://segmentfault.com/a/1190000003691119'))

[https://segmentfault.com/a/1190000002767365](javascript:openUrl('https://segmentfault.com/a/1190000002767365'))

[http://www.ruanyifeng.com/blog/2015/03/react.html](javascript:openUrl('http://www.ruanyifeng.com/blog/2015/03/react.html'))  

官网：

[http://reactjs.cn/react/docs/getting-started.html](javascript:openUrl('http://reactjs.cn/react/docs/getting-started.html'))

用到了react.js， react-dom.js，browser.min.js。browser.min.js用来解析JSX语言变成js语言，JSX是React特有的语言，他允许html和js的结合。

### html模板

```html
<!doctype html>
<html>
<head>
    <script src="../build/react.js"></script>
    <script src="../build/react-dom.js"></script>
    <script src="../build/browser.min.js"></script>
  </head>
  <body>
    <div id="example"></div>
    <script type="text/babel">
      // ** Our code goes here! **
    </script>
  </body>
</html>

```

### ReactDom.render()

ReactDOM.render 是 React 的最基本方法，用于将模板转为 HTML 语言，并插入指定的 DOM 节点。

`ReactDOM.render(模板(html语言),要插入的dom节点(js语言));`

```jsx
<script type="text/babel">
	ReactDom.render(
	<h1>hello</h1>,
	document.getElementById('example')
	);
</script>
```

### JSX语法

1.JSX 的基本语法规则：遇到 HTML 标签（以 `<` 开头），就用 HTML 规则解析；遇到代码块（以 `{` 开头），就用 JavaScript 规则解析。

```jsx
var names = ['Alice', 'Emily', 'Kate'];

ReactDOM.render(
  <div>
  {
    names.map(function (name) {
      return <div>Hello, {name}!</div>
    })
  }
  </div>,
  document.getElementById('example')
);
```

2.在模板中插入js变量。{变量}，如果变量是数组，则会展开。

```jsx
var arr = [
  <h1>Hello world!</h1>,
  <h2>React is awesome</h2>,
];
ReactDOM.render(
  <div>{arr}</div>,
  document.getElementById('example')
);
```

### React组件

React.createClass是构建组件，组件tag名作为变量，是个组件类，模板插入<组件tag名/>时，会自动生成组件类的一个实例。

组件类可以有属性,跟原生的HTML标签完全一致，属性与this.props对象上一一对应。

```jsx
const MyMessage=React.createClass({
	render: function() {
		return <h1>
        	hi {this.props.name}<!--组件类使用name属性-->
        </h1>;
	}
});
 
ReactDOM.render(
	<MyMessage name='Hennry'/>，/*添加组件name属性*/
	document.getElementById('example')
);
```

注意事项：

1. 组件tag名必须是**首字母大写**。

2. 所有组件类必须有自己的render方法，用于输入组件。

3. 组件模板必须只能包含一个顶层标签。`<h1></h1><p></p>`就不行，有两个顶层标签。

4. 添加组件属性，`class` 属性需要写成 `className` ，`for` 属性需要写成 `htmlFor` ，这是因为 `class` 和 `for` 是 JavaScript 的保留字。

   #### this.props.children

   表示组件的所有字节点。如果孩子是空，就是undefined类型，如果是一个值，就是object类型，如果是数组，就是Array类型，所以对于遍历来说，用React.Children工具方法就不用担心类型的问题。

   #### React.Children

   用React.Children来处理this.props.children的数据问题。array ，undefined ，object都自动解决。

   [api参考][https://facebook.github.io/react/docs/react-api.html]

   1.React.Children.map方法，迭代孩子，child是每次迭代的孩子。可返回一个新数组（如果孩子是数组的话）。

   `React.Children.map(this.props.children, function(child){return <h1>{child}</h1>});`

   2.React.Children.forEach方法，

   `React.Children.forEach(this.props.children, function(child){return <h1>{child}</h1>});`

   3.React.Children.count

   等等

   #### propTypes属性

组件的属性可以是任何类型，但是有时需要验证别人使用组件是，参数是否符合要求。详见[api][https://facebook.github.io/react/docs/react-api.html#react.proptypes]

```jsx
var MyTitle = React.createClass({
  propTypes: {
    title:React.PropTypes.string.isRequired,/*验证必须是string且不能为空*/
  },
  
  render: funtion(){
    return <h1>{this.props.title}</h1>;
  }
});

var data = 123;/*依照验证条件，不能通过*/

ReactDOM.render(
	<MyTitle title={data} />,
	document.body
	);
```

#### getDefaultProps方法

设置组件属性的默认值

```jsx
var MyTitle = React.createClass({
  getDefaultProps: function(){
    return {
    	title: 'hi'
    	};
  },
  
  render: function(){
    return <h1>{this.porps.title}</h1>;
  }
});

ReactDOM.render{
  <MyTitle />,
  document.body
};
```

#### 获取真实DOM节点——ref属性

组件并不是真实的 DOM 节点，而是存在于内存之中的一种数据结构，叫做虚拟 DOM （virtual DOM）。只有当它插入文档以后，才会变成真实的 DOM 。根据 React 的设计，所有的 DOM 变动，都先在虚拟 DOM 上发生，然后再将实际发生变动的部分，反映在真实 DOM上，这种算法叫做 [DOM diff](http://calendar.perfplanet.com/2013/diff/) ，它可以极大提高网页的性能表现。

但是，有时需要从组件获取真实 DOM 的节点，这时就要用到 `ref` 属性。

```jsx
var MyComponent = React.createClass({
  render:function() {
    return (
    	<div>
    		<input type = "text" ref="myTextInput" />
    		<input type = "button" value = "Focus the text input" onClick = {this.handleClient} />
    	</div>);
  }
});
```

 