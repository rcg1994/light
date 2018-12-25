> 译文链接：
>
> 原文链接：[https://github.com/facebook/react/issues/11527#issuecomment-360199710](https://github.com/facebook/react/issues/11527#issuecomment-360199710)
>
> 原文作者：[Dan Abramov](https://github.com/gaearon)

关于这个问题，我有一些想法。但它算不上是一个完整的回答，然而总比什么都不说好。

首先，我们应该都同意这个观点：延迟合并对于批量更新是有益的。换个思路，即我们都认为大多数情况下同步得进行重新渲染会导致效率降低。因此，当我们确定会有几个更新一起触发时，那么合并之后更新会有更好的效果。

例如，我们在浏览器中有这样的一个点击场景：点击导致`Child`子组件和`Parent`父组件都调用`setState`方法，我们自然不希望`Child`子组件被渲染两次，因此我们先将它们的渲染标记为脏渲染，而只需在这个点击事件触发的逻辑运行结束前去重新渲染一次即可。

这时候你可能要问：为什么一定要使用`setState`方法等到最后合并结束才去触发渲染，而不是直接修改`this.state`。我觉得实现的方法很多，但通过权衡，我们还是决定使用这个机制，以下是我们这么做的原因。

#### 保证内部的一致性

即使`state`是同步更新，`props`也不是。（你只有在父组件重新渲染时才能知道`props`）

现在React提供的对象（state，props，refs）在内部彼此一致。这意味着如果你使用这些对象，则可以保证它们引用了完全协调的树（即使是旧版本的树）。为什么这很重要？

当你只使用`state`时，如果它是同步更新渲染的，以下代码会起作用：

```javascript
console.log(this.state.value) // 0
this.setState({ value: this.state.value + 1 });
console.log(this.state.value) // 1
this.setState({ value: this.state.value + 1 });
console.log(this.state.value) // 2
```

然而，当这个`state`需要在几个组件之间共享时，我们会将其移动到父级：

```javascript
-this.setState({ value: this.state.value + 1 });
+this.props.onIncrement(); // 在父组件中去修改
```

然而，这样就破坏了我们的代码

```javascript
console.log(this.props.value) // 0
this.props.onIncrement();
console.log(this.props.value) // 0
this.props.onIncrement();
console.log(this.props.value) // 0
```

这是因为，在我们假设的模型中，`this.state`会立即刷新，但`this.props`不会。而且我们无法在不去渲染父组件的情况下去及时的更新`this.props`，这就导致了我们不得不放弃合并渲染的机制（效率将会大大得降低）。

那么现在`React`如何解决这个问题呢？在`React`中，`this.state`和`this.props`仅在合并和处理之后更新，因此您将在重构之前和之后看到0被打印。这使升降状态更安全。

是的，在某些情况下这可能不方便。特别是对于来自更多面向对象编程背景的人来说，他们只是想多次改变状态，而不是想在某一个地方代表一个完整的状态更新。我可以理解这一点，尽管我认为从调试的角度来看，保持状态更新更加清晰。但是，您可以选择将要立即读取的状态移动到组件的对象属性中，尤其是那些与组件重新渲染无关的数据。

总而言之，`React`的模型和机制，虽然不能让你的代码时刻都显得很简洁，但它却能保证你的状态提升是安全的。

#### 启用并发更新

从概念上讲，`React`的行为机制就像每个组件只有一个更新队列一样。我们在讨论是否应该立即对`this.state`进行应用更新的前提是其将按照确切的顺序进行更新。但是，不一定是这种情况。

我们一直在推崇 "异步渲染" 的一种理由是`React`可以根据它们来自何处来为`setState`调用分配不同的优先级：事件处理程序，网络响应，动画等。

