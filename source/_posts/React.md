---
layout: title
title: React
date: 2026-01-03 15:38:45
categories: web
tags: ["react"]
---
{% cq %} react学习记录 {% endcq %}

<!--more-->
## 学习资料
**<font style="color:rgb(23, 43, 77);">react源码解读</font>**

+ [<font style="color:rgb(0, 82, 204);">React技术揭秘</font>](https://react.iamkasong.com/)

**<font style="color:rgb(23, 43, 77);">性能优化</font>**

+ [<font style="color:rgb(0, 82, 204);">内存泄露排查</font>](https://mp.weixin.qq.com/s/DX4ry8oGHQ6fz1hauWpfzA)

**<font style="color:rgb(0, 0, 0);">React Hooks基础</font>**

+ [<font style="color:rgb(0, 82, 204);">React Hooks基础</font>](https://juejin.cn/post/6888668676711841800)

**<font style="color:rgb(23, 43, 77);">react-hooks三部曲</font>**

+ [<font style="color:rgb(0, 82, 204);">第一部： react-hooks如何使用</font>](https://juejin.cn/post/6864438643727433741)<font style="color:rgb(23, 43, 77);"> </font><font style="color:rgb(23, 43, 77);"> </font>
+ [<font style="color:rgb(0, 82, 204);">第二部：玩转react-hooks,自定义hooks设计模式及其实战</font>](https://juejin.cn/post/6890738145671938062)<font style="color:rgb(23, 43, 77);"> </font><font style="color:rgb(23, 43, 77);"> </font>
+ [<font style="color:rgb(0, 82, 204);">第三部：「react进阶」一文吃透react-hooks原理</font>](https://juejin.cn/post/6944863057000529933)<font style="color:rgb(23, 43, 77);"> </font><font style="color:rgb(23, 43, 77);"> </font>

**<font style="color:rgb(23, 43, 77);">react进阶系列</font>**

+ [<font style="color:rgb(0, 82, 204);">「react进阶」年终送给react开发者的八条优化建议</font>](https://juejin.cn/post/6908895801116721160)<font style="color:rgb(23, 43, 77);"> </font><font style="color:rgb(23, 43, 77);"> </font>
+ [<font style="color:rgb(0, 82, 204);">「react进阶」一文吃透React高阶组件(HOC)</font>](https://juejin.cn/post/6940422320427106335)<font style="color:rgb(23, 43, 77);"> </font><font style="color:rgb(23, 43, 77);"> </font>

**<font style="color:rgb(23, 43, 77);">react源码系列</font>**

+ [<font style="color:rgb(0, 82, 204);">「源码解析 」这一次彻底弄懂react-router路由原理</font>](https://juejin.cn/post/6886290490640039943)<font style="color:rgb(23, 43, 77);"> </font><font style="color:rgb(23, 43, 77);"> </font>
+ [<font style="color:rgb(0, 82, 204);">「源码解析」一文吃透react-redux源码（useMemo经典源码级案例）</font>](https://juejin.cn/post/6937491452838559781)<font style="color:rgb(23, 43, 77);"> </font>

**<font style="color:rgb(23, 43, 77);">开源项目系列</font>**

+ [<font style="color:rgb(0, 82, 204);">「react缓存页面」从需求到开源（我是怎么样让产品小姐姐刮目相看的）</font>](https://juejin.cn/post/6922340460136513549)<font style="color:rgb(23, 43, 77);"> </font>
+ [<font style="color:rgb(0, 82, 204);">「前端工程化」从0-1搭建react，ts脚手架（1.2w字超详细教程）</font>](https://juejin.cn/post/6919308174151385096)<font style="color:rgb(23, 43, 77);">  </font>

**<font style="color:rgb(23, 43, 77);">TypeScript教程</font>**

+ [<font style="color:rgb(0, 82, 204);">第一部： TypeScript入门教程</font>](https://ts.xcatliu.com/)

## JSX的本质
<!-- 这是一张图片，ocr 内容为： -->
<img src="/img/imageReact.png" alt="JSX的本质" />

## 受控组件和非受控组件
### 受控组件
<!-- 这是一张图片，ocr 内容为： -->
<img src="/img/imageReact1.png" alt="受控组件" />

### 非受控组件
<!-- 这是一张图片，ocr 内容为： -->
<img src="/img/imageReact2.png" alt="非受控组件" />

## 生命周期(旧)
<!-- 这是一张图片，ocr 内容为： -->
<img src="/img/imageReact3.png" alt="生命周期(旧)" />

```jsx
import { Component } from "react";

export default class Example extends Component{
  constructor(props){
    super(props)
    console.log("constructor ==> 第1个被执行")
  }
  UNSAFE_componentWillMount(){
    console.log("UNSAFE_componentWillMount ==> 第2个被执行")
  }
  componentDidMount(){
    console.log("componentDidMount ==> 第4个被执行")
  }
  render(){
    console.log("render ==> 第3个被执行")
    return <>测试生命周期函数</>
  }
}
```

```jsx
import { Component } from "react";

export default class Example extends Component {
  state = {
    name: "来自父组件"
  }
  hanldClick = ()=>{
    this.setState({
      name:"我是被父组件修改的数据"
    })
  }
  render() {
    return (
      <>
        <div>我是父组件</div>
        <button onClick={this.hanldClick}>我要修改子组件的数据</button>
        <br></br>
        <B name={this.state.name}></B>
      </>
    );
  }
}

class B extends Component {
  componentWillReceiveProps(){
    console.log("componentWillReceiveProps ==> 被执行")
  }
  // 控制组件更新的阀门
  shouldComponentUpdate(){
    console.log("shouldComponentUpdate ==> 被执行")
    return true
  }
  //组件将要更新的钩子
  componentWillUpdate(){
    console.log("componentWillUpdate ==> 不改变状态强制更新")
  }
  // 组件强制更新
  hanldClick = () =>{
    this.forceUpdate()
  }
  render() {
    return <>
      <h2>我是子组件下面的数据</h2>
      <div>{this.props.name}</div>
      <button onClick={this.hanldClick}>强制更新</button>
    </>;
  }
}
```

## 生命周期（新）
<!-- 这是一张图片，ocr 内容为： -->
<img src="/img/imageReact4.png" alt="生命周期（新）" />

## 深度学习
Es6 中 class 语法和继承

## ref 的使用
<!-- 这是一张图片，ocr 内容为：*/ 受控组件:基于修改数据/状态,让视图更新,达到需要的效果 [推荐] 非受控组件:基于REF获取DOM元素,我们操作DOM元素,来实现需求和效果[偶尔] 基于REF获取DOM元素的语法 1,给需要获取的元素设置REF-'XX',后期基于THIS.REFS.XX去获取相应的DOM元素[不推荐使用:在REACT. STRICTMODE模式下会报错 <H2 REF-"TITLEBOX">...</H2> 获取:THIS.REFS.TITLEBOX 2,把REF属性值设置为一个函数 REF{X>THIS.XXXX_X} +X是函数的形参:存储的就是当前DOM元素 +然后我们获取的DOM元素"X"直接挂在到实例的某个属性上(例如:BOX2) 获取:THIS.XXX 3,基于REACT.CREATEREF()方法创建一个REF对象 THIS.XXX-REACT.CREATEREF(); //>> THIS.XXX-{CURRENT:NULL> REF{REF对象(THIS.XXX)} 获取:THIS.XXX.CURRENT 原理:在RENDER渲染的时候,会获取VIRTUALDOM的REF属性 ]+如果属性值是一个字符串,则会给HIS.REFS增加这样的一个成员,成员值就是当前的DOM元素 +如果属性值是一个函数,则会把函数执行,把当前00M元素传递给这个函数[X->00M元素],而在函数执行的内部. 我们一般都会把DOM元素直接挂在到实例的某个属性上 ,如果属性值是一个REF对象,则会把DOM元素赋值给对象的CURRENT属性 * -->
<img src="/img/imageReact5.png" alt="ref 的使用" />

## setstate 方法
异步执行

<!-- 这是一张图片，ocr 内容为：在REACT18中,SETSTATE在任何地方执行,都是"异步操作" CLASS DEMO EXTENDS REACT.COMPONENT +REACT18中有一套更新队列的机制 STATE 三 +基于异步操作,实现状态的"批处理" X:10, 好处: 减少视图更新的次数,降低渲染消耗的性能 Y:5, 让更新的逻辑和流程更清晰&稳健 Z:0 子 更新队列UPDATER 在产生的私有上下文中,代码自上而下执行 @1会把所有的SETSTATE操作,先加入到更新队列 HANDLE () > { 只对当前上下文,同步要做的事情做处理 LET { X, Z 7 THIS.STATE; 不会立即更新状态和视图 @2当上下文中的代码都处理完毕后,会让更新 任务1:修改X THIS.SETSTATE({ X: X + 1 H); 而是加入到队列中 队列中的任务,统一渲染/更新一次批处理 CONSOLE.LOG(THIS.STATE.X);//10 THIS.SETSTATE({ Y: Y + 1 ; 任务2:修改Y 更新一次 SHOULDUPDATE 修改:X/Y/Z CONSOLE.LOG(THIS.STATE.Y);//5 WILLUPDATE THIS.SETSTATE({ Z: Z + 1 }); 修改状态 任务3:修改Z RENDER CONSOLE.LOG(THISSTATE.Z);//0 DIDUPDATE CALLBACK RENDER() { CONSOLE.LOG('视图渲染:RENDER'); LET { X, Y, Z } 三 THIS.STATE; RETURN <DIV> X:{X}-Y:{Y}-Z:{Z:{Z} <BR/> <BUTTON ONCLICK-{THIS.HANDLE}>按钮</BUTTON> </DIV>; -->
<img src="/img/imageReact6.png" alt="setstate 方法" />



