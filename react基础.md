## react基础

### 零、安装

react不用另外安装，只需要把这个库拷贝到你的硬盘就行了。react安装包直接到官网下载。https://facebook.github.io/react/docs/installation.html

### 一、HTML模板

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>react</title>
	<script src="./build/react.js"></script>
	<script src="./build/react-dom.js"></script>
	<script src="./build/browser.min.js"></script>
</head>
<body>
	<div id="example"></div>
	<script type="text/babel">
	</script>
</body>
</html>
```

注意：

1. 最后一个 ` 标签的 `type` 属性为 `text/babel` 。这是因为 React 独有的 JSX 语法，跟 JavaScript 不兼容。凡是使用 JSX 的地方，都要加上 `type="text/babel" 。
2. `react.js` 、`react-dom.js` 和 `Browser.js` ，它们必须首先加载。其中，`react.js` 是 React 的核心库，`react-dom.js` 是提供与 DOM 相关的功能，`Browser.js` 的作用是将 JSX 语法转为 JavaScript 语法。

### 二、ReactDOM.render()

ReactDom.render()是React的最基本方法，用于将模板转换为HTML语言，并插入指定的DOM节点。

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>react</title>
	<script src="./build/react.js"></script>
	<script src="./build/react-dom.js"></script>
	<script src="./build/browser.min.js"></script>
</head>
<body>
	<div id="example"></div>
	<script type="text/babel">
		ReactDOM.render(
			<h1>hello,world!</h1>,
			document.getElementById('example')
		)
	</script>
</body>
</html>
```

### 三、JSX语法

html语言直接写在JavaScript语言中，不加任何引号，就是JSX语法。

```html
<div id="example1"></div>
<script type="text/babel">
            var arr = ['html','JavaScript','react']
            ReactDOM.render(
                <div>
                    {
                      arr.map(function(arr){
                        return <div>{arr}语言！</div>
                      })
                    }
  				</div>,
                document.getElementById('example1')
            )
</script>
```

JSX的基本语法规则：遇到html标签以<开头，就用html规则解析，遇到代码块以{开头，就用JavaScript规则解析。

JSX允许直接在模板插入JavaScript变量。

```html
<div id="example2"></div>
<script type="text/babel">
		var arr1 = [
			<h1>hello，world!</h1>,
			<h2>react</h2>
		]
		ReactDOM.render(
			<div>{arr1}</div>,
			document.getElementById('example2')
		)
</script>
```

### 四、组件

react允许将代码封装成组件，然后像HTML标签一样，在网页中插入这个组件。React.createClass方法就用于生成一个组件类。

```html
<div id="example2"></div>
<script type="text/babel">
      var Message = React.createClass({
        render: function(){
          return <h1>hello,{this.props.name}</h1>
        }
      })

      ReactDOM.render(
        <Message name="xuezhiqian"/>,
        document.getElementById('example2')
      )
</script>
```

注意：变量Message就是一个组件类。模板插入<Message />时，会自动生成Message的一个实例（下文的”组件“都指组件类的实例）。所有组件类都必须有自己的render方法，用于输出组件。

组件类的第一个字母必须大写，否则会报错，比如Message不能写成message。另外，组件类只能包含一个顶层标签，否则也会报错。

```javascript
var Message = React.createClass({
  render: function(){
    return <h1>
      hello {this.props.name}
    </h1><p>
      some text</p>
  }
})
```

上面代码会报错，因为Message组件包含了两个顶层标签：h2、p。

组件的用法与原生的HMTL标签完全一致，可以任意加入属性，比如<Message name="John">，就是Message组件加入一个name属性，值为John。组件的属性可以在组件类的this.props对象上获取，比如name属性就可以通过this.props.name读取。添加组件属性，有一个地方需要注意，就是class属性要写成className,for属性需要写成htmlFor，这厮因为class和for是javascript的保留字。

### 五、this.props.children

this.props对象的属性与组件的属性——对应，但是有一个例外，就是this.props.children属性。它标示组件的所有子节点。

```html
<div id="example4"></div>
<script type="text/babel">
      var PropsList = React.createClass({
      render: function(){
        return(
          <ol>
            {
              React.Children.map(this.props.children,function(child){
                return <li>{child}</li>
              })
            }
  		 </ol>
       )
      }
    })

    ReactDOM.render(
      <PropsList>
          <span>hello</span>
          <span>world</span>
      </PropsList>,
      document.getElementById('example4')
    )
</script>
```

上面代码的PropsList组件有两个span子节点，它们都可以通过this.props.children读取。

这里需要注意，this.props.children的值有3种可能：如果当前组件没有子节点，它就是undefined；如果有一个子节点，数据类型是object；如果有多个子节点，数据类型就是array。所以，处理this.props.children的时候要小心。

React提供一个工具方法，React.Children来处理this.props.children。我们可以用React.Children.map来遍历子节点，而不用担心this.props.children的数据类型是undefined还是object。

###六、PropTypes

组件的属性可以接受任意值，字符串、对象、函数等等都可以。有时，我们需要一种机制，验证别人使用组件时，提供的参数是否符合要求。

组件类的PropTypes属性，就是用来验证组件实例的属性是否符合要求。

```javascript
var Mytitle = React.createClass({
  propTypes: {
    title: React.PropTypes.string.isRequired,
  },
  render: function(){
    return <h1>{this.props.title}</h1>
  }
})
```

上面的Mytitle组件有一个title属性。PropTypes告诉React，这个title属性是必须的，而且它的值必须是字符串。现在，我们设置title属性的值是一个数值。

```javascript
var data = '123'
ReactDOM.render(
  <Mytitle title = {data}/>,
  document.getElementById('example5')
)
```

title属性通不过验证。控制台会报错。

此外，getDefaultProps方法可以用来设置的默认值。

```html
var Title = React.createClass({
  getDefaultProps: function(){
    return{
      title: 'my name is title'
    }
  },
  render: function(){
    return <h2>{this.props.title}</h2>
  }
})

ReactDOM.render(
  <Title />,
  document.getElementById('example6')
)
```

### 七、获取真实的DOM节点

组件并不是真实的 DOM 节点，而是存在于内存之中的一种数据结构，叫做虚拟 DOM （virtual DOM）。只有当它插入文档以后，才会变成真实的 DOM 。根据 React 的设计，所有的 DOM 变动，都先在虚拟 DOM 上发生，然后再将实际发生变动的部分，反映在真实 DOM上，这种算法叫做 DOM diff ，它可以极大提高网页的性能表现。
从组件获取真实 DOM 的节点，这时就要用到 ref 属性。

```html
<div id="example7"></div>
  <script type="text/babel">
    var MyComponent = React.createClass({
      handleClick: function() {
        this.refs.myTextInput.focus()
      },
      render: function() {
        return (
          <div>
          <input type="text" ref="myTextInput" />
          <input type="button" value="Focus the text input" onClick={this.handleClick} />
          </div>
        )
      }
    })

    ReactDOM.render(
      <MyComponent />,
      document.getElementById('example7')
    );
</script>
```

上面代码中，组件 MyComponent的子节点有一个文本输入框，用于获取用户的输入。这时就必须获取真实的 DOM 节点，虚拟 DOM 是拿不到用户输入的。为了做到这一点，文本输入框必须有一个 ref 属性，然后 this.refs.[refName] 就会返回这个真实的 DOM 节点。
需要注意的是，由于 this.refs.[refName] 属性获取的是真实 DOM ，所以必须等到虚拟 DOM 插入文档以后，才能使用这个属性，否则会报错。上面代码中，通过为组件指定 Click 事件的回调函数，确保了只有等到真实 DOM 发生 Click 事件之后，才会读取 this.refs.[refName]属性。
React 组件支持很多事件，除了 Click事件以外，还有 KeyDown、Copy、Scroll 等。