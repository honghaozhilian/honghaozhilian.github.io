---
layout:     post
title:      "前端框架"
subtitle:   "面试题归纳"
date:       2018-07-30
author:     "honghaozhilian"
header-img: "img/post-bg-js-module.jpg"
tags:
    - 前端开发
---

## 前端框架

> 主要是前端的框架的相关问题

## 目录
- [前端路由的实现原理](#前端路由的实现原理)
- [react和angular以及vue的设计思想](#react和angular以及vue的设计思想)
- [react的单向数据原理与vue的双向数据绑定原理](#react的单向数据原理与vue的双向数据绑定原理)
- [react与vue各自的组件生命周期](#react与vue各自的组件生命周期)
- [react与vue各自的组件通信](#react与vue各自的组件通信)
- [react的事件绑定this的解决方案](#react的事件绑定this的解决方案)
- [react的批量更新机制](#react的批量更新机制)
- [react的优化原理](#react的优化原理)
- [react_router3和react_router4的区别](#react_router3和react_router4的区别)
- [redux的优缺点](#redux的优缺点)
- [redux是如何做到可预测呢](#redux是如何做到可预测呢)
- [redux将react组件划分为哪两种](#redux将react组件划分为哪两种)
- [什么是高阶组件及应用场景](#什么是高阶组件及应用场景)
- [受控组件与非受控组件](#受控组件与非受控组件)
- [vue的computed的依赖缓存](#vue的computed的依赖缓存)
- [angular的脏值检测原理](#angular的脏值检测原理)
- [backbone的mvc实现方式](#backbone的mvc实现方式)
- [webpack热更新实现原理](#webpack热更新实现原理)
- [webpack的loader的原理](#webpack的loader的原理)

## 前端路由的实现原理
- hash路由:利用锚点技术改变页面url,监听onhashchange事件去改变视图
- history路由：利用html5的history.pushState进行路由控制，前进后退按钮会触发onPopState事件，a标签则要为所有a标签绑定click事件

## react和angular以及vue的设计思想
- react:
    - 特点：单向数据流，虚拟DOM，组件为核心，组件生命周期
    - 单向数据流：通过设置setState去更新视图，父组件到子组件的通过prop传值
    - 虚拟DOM：利用state和props模拟DOM结构，包装成具有DOM结构的js对象，再通过diff算法比较每次产生的虚拟DOM和上一次渲染的虚拟DOM，最后修改真正的DOM树
- vue：
    - 特点：双向数据绑定，以Vue实例为核心进行扩展，包括组件（生命周期，模板，方法），指令，过滤器
    - 双向数据绑定：通过object.definePropoty设置setter和getter（数据劫持）,当数据发生变化，监听到相关组件的改变从而更新相关组件的视图，当改变的数据是较高层次的，影响多个组件就会虚拟DOM比较更新
    - 
- angular1.X:
    - 特点：双向数据绑定，以模块，指令为核心，依赖注入，脏值检测，作用域
    
## react的setState原理与vue的双向数据绑定原理
- react:调用setState将对象合并到原本的state中，构建一个react元素树（具有DOM结构的对象树），通过Diff算法比较不同，精确地更新UI，setState本身的实现是同步的，即在原生事件和setTimeout之中总能拿到最新的state，而react内部（合成事件和钩子函数）则因为事务和队列而导致异步的结果
    <img src="{{ site.baseurl }}/img/setState.png" alt="setState"/>
- vue : 双向数据绑定通过Object.defineProperty设置obj的getter和setter去实现（自己的理解：vue源码中有一个用于生成监听实例的Watcher构造函数和管理依赖的Dep构造函数，在getter中调用dep添加watcher到dep中的数组,在setter调用dep的通知函数通过watcher更新）
    - vue将data初始化为一个Observer并对对象中的每个值，重写了其中的get、set，data中的每个key，都有一个独立的依赖收集器。
    - 在get中，向依赖收集器添加了监听
    - 在mount时，实例了一个Watcher，将收集器的目标指向了当前Watcher
    - 在data值发生变更时，触发set，触发了依赖收集器中的所有监听的更新，来触发Watcher.update
    <img src="{{ site.baseurl }}/img/vueData.png" alt="vueData"/>

## react与vue各自的组件生命周期
- react：装载，更新，卸载
    - 装载：constructor,getInitialState,getDefaultProps,componentWillMount,render,componentDidMount;多个组件加载componentDidMount是放在一起执行的
    - 更新：componentWillReceiveProps(或static getDerivedStateFromProps),shouldComponentUpdate,componentWillUpdate,render,componentDidUpdate;componentWillReceiveProps在父元素的render被调用时触发，setState的过程不会触发
    - 卸载：componentWillUnmount，用来清楚定时器等
- 注意：react16.3之后的生命周期和之前的有所不同，具体如图：
    <img src="{{ site.baseurl }}/img/react.png" alt="react"/>
    <img src="{{ site.baseurl }}/img/react16.png" alt="react16"/>
- vue: 在装载，更新，卸载，还多了beforeCreate(),created()
    - beforeCreate()：在实例初始化之后，数据观测 (data observer) 和 event/watcher 事件配置之前被调用(无法获取 data中的数据、methods中的方法)
    - created()：完成了data数据的初始化，可以调用methods中的方法、改变data中的数据；但是没有完成el的初始化(发送请求获取数据)
    - beforeMount():在挂载开始之前被调用：相关的 render 函数首次被调用。完成了el的初始化(虚拟DOM)
    - mounted():vue实例已经挂载到页面中，可以获取到el中的DOM元素，进行DOM操作
    - beforeUpdate():数据更新时调用，发生在虚拟 DOM 重新渲染和打补丁之前。你可以在这个钩子中进一步地更改状态，这不会触发附加的重渲染过程。
    - updated():由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。当这个钩子被调用时，组件 DOM 已经更新，所以你现在可以执行依赖于 DOM 的操作。
    - beforeDestroy():用来清楚定时器等，destroyed()
    <img src="{{ site.baseurl }}/img/vue.png" alt="vue"/>

## react与vue各自的组件通信
- react:
    - 父组件向子组件通信: props
    - 子组件向父组件通信: 回调函数/自定义事件
    - 跨级组件通信: 层层组件传递props/context,
        - context:上级组件声明支持context(声明childPropTypes),提供一个函数返回代表context的对象(getChildContext)，下级组件声明需要context(声明contextTypes),然后通过this.context
    - 没有嵌套关系组件之间的通信: 自定义事件(如何实现Event Bus)
    - 更复杂可以用redux等状态管理库
- vue: 
    - 父子组件用Props通信
    - 子组件向父组件用$emit(event)
    - 非父子组件用Event Bus通信
    - 如果项目够复杂,可能需要Vuex等全局状态管理库通信

## react的事件绑定this的解决方案
- 绑定事件时调用bind,onclick={this.handleclick.bind(this,'test')}
- 在构造函数中声明，this.handleClick = this.handleClick.bind(this)(容易造成性能问题，每次调用父组件的render，都会执行bind导致props不一样，每次都会重新执行子组件的render)
- 直接声明箭头函数,需要babel转译，babel-preset-stage-2中支持，env不支持

## react的批量更新机制
- 在 setState 的时候react内部会创建一个 updateQueue ，通过 firstUpdate 、 lastUpdate 、 lastUpdate.next 去维护一个更新的队列，在最终的 performWork 中，相同的key会被覆盖，只会对最后一次的 setState 进行更新（[批量更新](https://juejin.im/post/5ad60c2cf265da23a142696c)）

## react的优化原理
- react通过shouldComponentUpdate的结果，再通过DOM Diff算法计算比较
- 利用shouldComponentUpdate(nextProps,nextState)比较前后props和state,避免无效更新
    - {...this.props} (不要滥用，请只传递component需要的props，传得太多，或者层次传得太深，都会加重shouldComponentUpdate里面的数据比较负担，因此，也请慎用spread attributes（<Component {...props} />）)。
    - ::this.handleChange()。(请将方法的bind一律置于constructor)
    - onClick=this.handleChange.bind(this,id)(父组件每次render都会执行bind，导致props不一样)
    - 复杂的页面不要在一个组件里面写完。
    - 请尽量使用const element。
    - map里面添加key，并且key不要使用index（可变的）。具体可参考 使用Perf工具研究React Key对渲染的影响
    - 尽量少用setTimeOut或不可控的refs、DOM操作。
    - 数据尽可能简单明了，扁平化。
- 解决方案：
    - PureRenderMixin(es5)

            var PureRenderMixin = require('react-addons-pure-render-mixin');
            React.createClass({
                mixins: [PureRenderMixin],
                render: function() {
                    return <div className={this.props.className}>foo</div>;
                }
            });

    - Shallow Compare (es6)

            var shallowCompare = require('react-addons-shallow-compare');
            export class SampleComponent extends React.Component {
                shouldComponentUpdate(nextProps, nextState) {
                    return shallowCompare(this, nextProps, nextState);
                }
                render() {
                    return <div className={this.props.className}>foo</div>;
                }
            }

    - es7装饰器的写法

            import pureRender from "pure-render-decorator"
            @pureRender
            class Person  extends Component {
                render() {
                    const {name,age} = this.props;
                    return (
                        <div><span>{name}</span><span>{age}</span></div>
                    )
                }
            }

    - PureComponent类(react15.3之后新增的)，我们知道组件的state和props的改变就会触发render，但是组件的state和props没发生改变，render就不执行，只有PureComponent检测到state或者props发生变化时，PureComponent才会调用render方法,一般纯组件才能启作用，复杂组件的shallowEqual检查一般过不了

            class Example extends PureComponent{}

    - immutable.js：以上的方法都是浅比较，对于对象类型无效，在shouldComponentUpdate使用deepCopy又十分耗性能，采用immutable.js可解决以上问题。原理是 Persistent Data Structure （持久化数据结构），也就是使用旧数据创建新数据时，要保证旧数据同时可用且不变。同时为了避免 deepCopy 把所有节点都复制一遍带来的性能损耗，Immutable 使用了 Structural Sharing （结构共享），即如果对象树中一个节点发生变化，只修改这个节点和受它影响的父节点，其它节点则进行共享。

            import { is } from 'immutable';

            shouldComponentUpdate: (nextProps = {}, nextState = {}) => {
                const thisProps = this.props || {}, thisState = this.state || {};
                if (Object.keys(thisProps).length !== Object.keys(nextProps).length ||
                    Object.keys(thisState).length !== Object.keys(nextState).length) {
                    return true;
                }
                for (const key in nextProps) {
                    if (!is(thisProps[key], nextProps[key])) {
                        return true;
                    }
                }
                for (const key in nextState) {
                    if (thisState[key] !== nextState[key] || !is(thisState[key], nextState[key])) {
                        return true;
                    }
                }
                return false;
            }

## react_router3和react_router4的区别
- 4.X的核心代码放在react-router-dom中，3.X放于react-router中
- 4.X的Router容器下可以放任意标签，3.X只能放Route
- 4.X的history直接内置BrowserRouter，HashRouter,MemoryRouter三种Ruoter,3.X通过createhistory创建history转入Router中

## redux的优缺点
- 先说说flux的缺点：多个store容易造成状态不同步，涉及多个store之间需要等待，store之间存在依赖，数据存储和处理都在store中
- 优点：单一数据源，保持状态只读，改变状态只能通过action完成，保证了数据的可靠性，数据处理放于reducer函数中，与存储分开，不同页面的数据不存在依赖
- 缺点：需要每个页面都引入一个store,都需要监听store的改变（store.subscribe）,api较为较为繁琐，需要依赖第三方插件才能高效地获得局部的状态

## redux是如何做到可预测呢
- 单一数据源Store,所有状态都保存在一个对象树中，保证了各个组件的状态的同步
- 制定严格的规则，保持状态是只读的，避免产生副作用，阻止外部文件修改state，所有对state的改动都被集中于一个地方，并且被严格地依次触发，更改state的唯一方式是派发相应的action，以描述所需的更改
- 状态的改变只能通过纯函数reducer,不会产生副作用，产生不可预测的数据

## redux将react组件划分为哪两种
- UI组件和容器组件，
- UI组件即所谓的纯组件，只负责 UI 的呈现，不带有任何业务逻辑，没有状态，所有数据来源于props,不使用任何 Redux 的 API
- 容器组件：负责管理数据和业务逻辑，不负责 UI 的呈现,带有内部状态,使用 Redux 的 API


## 什么是高阶组件及应用场景

## 受控组件与非受控组件
- 受控组件与非受控组件主要对于input,textarea,select这类特定的DOM元素
- 受控组件也就是受状态控制的组件，必须绑定onChange，defaultValue可以设置默认值

        class Input extends Component{
            constructor(){
                super();
                this.state={val:'100'}
            }
            handleChange=(e)=>{//e是事件源
                let val=e.target.value;
                this.setState({val});
        }
            render(){
                return (<div>
                    <input type="text" value={this.state.val} onChange={this.handleChange}/>
                    {this.state.val}
                </div>)
            }
        }

- 非受控组件：输入框的值不与state实时同步，通过ref来获取输入框的值

        class Sum extends Component{
            constructor(){
                super();
                this.state={result:''}
            }
            //通过ref设置的属性，可以通过this.refs获取到对应的dom元素
            handleChange=()=>{
                let result=this.refs.a.value+this.b.value;
                this.setState({result});
            }
            render(){
                return (<div onChange={this.handleChange}>//运用冒泡的机制不在两个input中添加onchange事件
                    <input type="text" ref="a"/>
                    //x代表真实的dom  ，把元素挂载到了当前实例上
                    <input type="text" ref={x=>this.b=x}/>
                    {this.state.result}
                </div>)
            }
        }

## vue的computed的依赖缓存


## angular的脏值检测原理

## backbone的mvc实现方式

## webpack热更新实现原理
1. Webpack编译期，为需要热更新的 entry 注入热更新代码(EventSource通信)
2. 页面首次打开后，服务端与客户端通过 EventSource 建立通信渠道，把下一次的 hash 返回前端
3. 客户端获取到hash，这个hash将作为下一次请求服务端 hot-update.js 和 hot-update.json的hash
4. 修改页面代码后，Webpack 监听到文件修改后，开始编译，编译完成后，发送 build 消息给客户端
5. 客户端获取到hash，成功后客户端构造hot-update.js script链接，然后插入主文档
6. hot-update.js 插入成功后，执行hotAPI 的 createRecord 和 reload方法，获取到 Vue 组件的 render方法，重新 render 组件， 继而实现 UI 无刷新更新。

## webpack的loader的原理



---
