

# 1. 什么是react

react是facebook在2011年开发前端的js 库

他遵循基于组件的方法，有助于构建可重复用的ui组件

他用于开发复杂和交互的Web和移动UI,开源时间是2015年

# 2. React有什么特点

##  2.1 react 特点

​	主要功能有：

​	1.它使用的是虚拟DOM而不是真正的DOM

​	2.它可以进行服务器的渲染

  	3.它遵循着单向数据流的原则

## 2.2 react的优点

​	1.提高了性能

​	2.可以方便在客户端和服务器使用

​	3.由于用的是jsx，代码的可读性很高

​	4.react很容易与meteor，angular等其他框架集成

​	5.使用react，编写ui测试用例会变得非常容易

## 2.3 react有哪些限制

​	1.react是一个库，不是一个完整的框架

​	2.它的库非常的庞大，需要用时间来理解

​	3.新手程序员可能很难理解

​	4.编码变得复杂，因为他使用了内联模板和jsx

## 2.4 什么是jsx

jsx是javascript XML的简写。是react使用的一种文件，他利用JavaScript的表现力和类似HTML的模板语法。这使得HTML文件非常容易理解

此应用文件非常可靠，并能够提高性能。

##  2.5 与 ES5 相比，React 的 ES6 语法有何不同？

1.require和import

2.export和exports

3.component和function

4.props

5.state

# 4. **React 组件**

## 4.1 你怎么理解在react中，一切都是组件这句话

组件是react应用UI的构建块，这些组件将整个UI分成小的独立并可重用的部分。每个组件的彼此都是独立的，而不会影响UI的其他部分。

## 4.2 怎么解释react中render()的目的

每个react组件强制要求必须有一个render().他返回一个react元素，是原生DOM组件的表示。如果渲染多个HTML元素，则必须将他们组合在一个封闭标记内。这个函数必须保持纯净，即必须每次调用时都返回相同的结果。

## 4.3 什么是props

props是React中属性的简写。他们是只读组件，必须保持纯，即不可变。他们总是在整个应用中从父组件传递到子组件。子组件永远不能将prop送回父组件。这样助于维护单向数据流，通常用于呈现动态生成的数据。

## 4.4 react中的状态是什么？它是如何使用的？

状态是react组件的核心，是数据的来源，必须尽可能的简单。基本上状态是确定组件呈现和行为的对象。与props不同，他是可变的，并创建动态和交互式组件。可以通过this.state()访问他们。

## 4.5 **区分状态和 props**

| **条件**                | **State** | **Props** |
| ----------------------- | --------- | --------- |
| 1. 从父组件中接收初始值 | Yes       | Yes       |
| 2. 父组件可以改变值     | No        | Yes       |
| 3. 在组件中设置默认值   | Yes       | Yes       |
| 4. 在组件的内部变化     | Yes       | No        |
| 5. 设置子组件的初始值   | Yes       | Yes       |
| 6. 在子组件的内部更改   | No        | Yes       |

## 4.6 如何更新组件的状态

可以用this.etState()更新组件的状态

## 4.7 **React 中的箭头函数是什么？怎么用？**

箭头函数（=>)是用于编写函数表达式的简短语法。这些函数允许正确绑定组件的上下文，因为在ES6中默认下不能使用自动绑定。使用高阶函数的时候，箭头函数非常有用。

##  4.8 **区分有状态和无状态组件。**

| **有状态组件**                                               | **无状态组件**                                  |
| ------------------------------------------------------------ | ----------------------------------------------- |
| 1. 在内存中存储有关组件状态变化的信息                        | 1. 计算组件的内部的状态                         |
| 2. 有权改变状态                                              | 2. 无权改变状态                                 |
| 3. 包含过去、现在和未来可能的状态变化情况                    | 3. 不包含过去，现在和未来可能发生的状态变化情况 |
| 4. 接受无状态组件状态变化要求的通知，然后将 props 发送给他们。 | 4.从有状态组件接收 props 并将其视为回调函数。   |

##  4.9 **React组件生命周期的阶段是什么？**

react生命周期分为三个阶段

1.初始化阶段：这个组件即将开始其生命之旅并进入DOM的阶段

2.更新阶段：一旦组件被添加到DOM，它只有在prop或状态发生时候才可能更新和重新渲染。这些只发生在这个阶段。

3.销毁阶段（卸载阶段）：这是组件生命周期的最后阶段，组件被销毁并从DOM中删除

## 4.10 **详细解释 React 组件的生命周期方法。**

一些最重要的生命周期方法是： 

1.componentWillMount()  在渲染之前执行，在客户端和服务器端都会执行

2.componentDidMount() 仅在第一次渲染后再客户端执行

3.componentWillReceiveProps() 当从父类接受到props并且再调用另一个渲染器之前调用

4.shouldComponentUpdate() 根据特定条件返回true或者false。如果你希望更新组件，请返回true，否则返回false。默认情况下返回false

5.componentWillUpdate() 在DOM中进行渲染

6.componentDidUpdate()  在渲染发生后立即调用。

7.omponentWillUnmount()  从 DOM 卸载组件后调用。用于清理内存空间。

## **React中的事件是什么？**

在react中，事件对鼠标悬停，鼠标单击，按键等特定操作触发反应。处理这些事件类似处理DOM元素中的事件。但是语法上有一些差异

​	1.用驼峰命名而不是仅用小写字母

​	2.事件作为函数而不是字符串传递 （因为有this指向的问题 所以触发的函数建议使用箭头函数）

## 如何在React中创建一个事件

```js
class Display extends React.Component({    
    show(evt) {
        // code   
    },   
    render() {      
        // Render the div with an onClick prop (value is a function)        
        return (            
            <div onClick={this.show}>Click Me!</div>
        );    
    }
});
```

## React中的合成事件是什么

合成事件是围绕浏览器原生事件充当跨浏览器包装器的对象。它们将不同浏览器的行为合并为一个 API。这样做是为了确保事件在不同浏览器中显示一致的属性。 

## **你对 React 的 refs 有什么了解？**

refs是react中引用的简写，他是有助于存储对特定的react元素或者组件的引用属性，他将由组件渲染配置函数返回。用于render()返回的特定元素或组件的引用

```js
class ReferenceDemo extends React.Component{
     display() {
         const name = this.inputDemo.value;
         document.getElementById('disp').innerHTML = name;
     }
render() {
    return(        
          <div>
            Name: <input type="text" ref={input => this.inputDemo = input} />
            <button name="Click" onClick={this.display}>Click</button>            
            <h2>Hello <span id="disp"></span> !!!</h2>
          </div>
    );
   }
 }
```

## **列出一些应该使用 Refs 的情况。**

1.需要管理焦点，选择文本或媒体播放时

2.触发动画

3.与第三方DOM集成

## **如何模块化 React 中的代码？**



## 如何在react中创建表单

React 表单类似于 HTML 表单。但是在 React 中，状态包含在组件的 state 属性中，并且只能通过 `setState()` 更新。因此元素不能直接更新它们的状态，它们的提交是由 JavaScript 函数处理的。此函数可以完全访问用户输入到表单的数据。 

```js
handleSubmit(event) {
    alert('A name was submitted: ' + this.state.value);
    event.preventDefault();
}
 
render() {
    return (        
        <form onSubmit={this.handleSubmit}>
            <label>
                Name:
                <input type="text" value={this.state.value} onChange={this.handleSubmit} />
            </label>
            <input type="submit" value="Submit" />
        </form>
    );
}
```

## 受控组件和非受控组件

| **受控组件**                                   | **非受控组件**           |
| ---------------------------------------------- | ------------------------ |
| 1. 没有维持自己的状态                          | 1. 保持着自己的状态      |
| 2.数据由父组件控制                             | 2.数据由 DOM 控制        |
| 3. 通过 props 获取当前值，然后通过回调通知更改 | 3. Refs 用于获取其当前值 |

## 什么是高阶组件

高阶组件是重用组件逻辑的高级方法，是一种源于react的组件模式。HOC是自定义组件，在他之内包含另外一个组件，他可以接受子组件提供的任何状态，但不会修改或者复制其输入的任何行为。你可以认为是HOC是纯组件

## **如何模块化 React 中的代码？**

可以使用export和import属性来模块化代码。他们有助于在不同的文件中单独编写组件

```js
//ChildComponent.jsx
export default class ChildComponent extends React.Component {
    render() {
        return(           
              <div>
                  <h1>This is a child component</h1>
              </div>
        );
    }
}
 
//ParentComponent.jsx
import ChildComponent from './childcomponent.js';
class ParentComponent extends React.Component {    
    render() {        
        return(           
             <div>               
                <App />          
             </div>       
        );  
    }
}
```

## **什么是纯组件？**

纯组件是可以编写的最简单，最快的组件。他们可以替换任何只有render()的组件。这些组件增强了代码的简单性和应用的性能。

## **React 中 key 的重要性是什么？**

key是用于识别唯一的Virtual DOM元素及其驱动UI的相应数据。他们是通过回收DOM中当前所有的元素来帮助React优化渲染的。这些key必须是唯一的数字或者字符串，react只是重新排序元素而不是重新渲染他们，这样可以提高应用程序的性能。解决了就地复用的BUG

# React路由

## 什么是React路由

react路由是构建在react之上的强大的路由库，他有助于向应用程序添加新的屏幕和流。这使得URL与网页上显示的数据保持同步，他负责维护标准化的结构和行为，并用于单页面web应用。react理由有一个简单的api

##  **为什么React Router v4中使用 switch 关键字 ？**

如果有多个规则匹配了，只会渲染第一个匹配的规则。

虽然 **<div>** 用于封装 Router 中的多个路由，当你想要仅显示要在多个定义的路线中呈现的单个路线时，可以使用 “switch” 关键字。使用时，**<switch>** 标记会按顺序将已定义的 URL 与已定义的路由进行匹配。找到第一个匹配项后，它将渲染指定的路径。从而绕过其它路线。 

## 为什么需要react 中的路由 

为了解决单页面应用程序的切换页面

## **列出 React Router 的优点。**

1.就像react基于组件一样，

1. 就像 React 基于组件一样，在 React Router v4 中，API 是 *'All About Components'*。可以将 Router 可视化为单个根组件（**<BrowserRouter>**），其中我们将特定的子路由（**<route>**）包起来。
2. 无需手动设置历史值
3. 包是分开的，共有三个包，分别用于 Web、Native 和 Core。这使我们应用更加紧凑。基于类似的编码风格很容易进行切换。 

##  **React Router与常规路由有何不同？**

| **主题**       | **常规路由**                                    | **React 路由**                   |
| -------------- | ----------------------------------------------- | -------------------------------- |
| **参与的页面** | 每个视图对应一个新文件                          | 只涉及单个HTML页面               |
| **URL 更改**   | HTTP 请求被发送到服务器并且接收相应的 HTML 页面 | 仅更改历史记录属性               |
| **体验**       | 用户实际在每个视图的不同页面切换                | 用户认为自己正在不同的页面间切换 |