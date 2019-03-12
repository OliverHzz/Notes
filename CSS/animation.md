## 动画的主要组成部分

##### CSS动画由两个基本部分组成：

- **关键帧** - 定义动画的阶段和样式
- **动画属性** - 将@keyframes分配给一个特定的CSS元素并定义它如何动画

#### 关键帧(Keyframes)

关键帧是CSS动画的基础。他们定义动画时间轴每个阶段的动画效果。每个@keyframes组成如下：

- **动画名称**：描述动画的名称，例如bounceIn
- **动画的阶段**：动画的每个阶段都以百分比表示。0％表示动画的开始状态。100％表示动画的结束状态。可以在两者之间添加多个中间状态。
- **CSS属性**：为动画时间轴的每个阶段定义的CSS属性。

```
@keyframes bounceIn {
  0% {
    transform: scale(0.1);
    opacity: 0;
  }
  60% {
    transform: scale(1.2);
    opacity: 1;
  }
  100% {
    transform: scale(1);
  }
}
```

#### 动画属性(Animation Properties)

> 一旦定义了@keyframes，就必须添加动画属性才能让动画起作用。

动画属性做两件事：

1. 将@keyframes分配给要动画的元素。
2. 定义它是如何动画的。

动画属性被添加到您想要动画的CSS选择器（或元素）。您必须添加以下两个动画属性才能使动画生效：

- animation-name：动画的名称，在@keyframes中定义。
- animation-duration：动画的持续时间，以秒（例如5s）或毫秒（例如200ms）为单位。

> 继续上面的bounceIn示例，我们将animation-name和animation-duration添加到想要动画的div上。

==默认div是块级元素，占满一整行，动画会对整个元素进行操作，所以效果可能会和预期不同==
```
div {
  display: inline-block;
  animation-duration: 2s;
  animation-name: bounceIn;
}
```

#### 关于前缀

截至2014年底，许多基于Webkit的浏览器仍然使用带-webkit前缀版本的animations、keyframes和转换。为了在这些浏览器上运行，您需要包含不带前缀和带WebKit前缀的代码。（为了简单起见，在示例中使用不带前缀的版本。）


```
div {
  -webkit-animation-duration: 2s;
  animation-duration: 2s;
  -webkit-animation-name: bounceIn;
  animation-name: bounceIn;
}

@-webkit-keyframes bounceIn { /* styles */ }
@keyframes bounceIn { /* styles */ }
```

为了让您的生活更轻松，请考虑使用[Bourbon](https://www.bourbon.io/)，这是一个Sass mixin库，其中包含适用于所有现代浏览器的前缀。以下是使用Bourbon生成浏览器特定前缀的动画和关键帧的简单方法：


```
div {
  @include animation(bounceIn 2s);
}
@include keyframes(bouncein) { /* styles */}
```

#### 其他动画属性

除了所需的动画名称和动画持续时间属性之外，还可以使用以下属性进一步自定义和创建复杂动画：

- animation-timing-function
- animation-delay
- animation-iteration-count
- animation-direction
- animation-fill-mode
- animation-play-state

#### Animation-timing-function

animation-timing-function定义动画的速度曲线或步长。您可以使用以下预定义时间选项指定时间：cubic-bezier(n,n,n,n), ease, linear, ease-in, ease-out, ease-in-out, initial, inherit。

- **linear**: 动画从头到尾的速度是相同的
- **ease**: 默认。动画以低速开始，然后加快，在结束前变慢
- **ease-in**: 动画以低速开始
- **ease-out**: 动画以低速结束
- **ease-in-out**: 动画以低速开始和结束
- **cubic-bezier(n,n,n,n)**: 在 cubic-bezier 函数中自己的值。可能的值是从 0 到 1 的数值
- **steps()**: 逐步运动，非连续性动画

```
linear      ：{-webkit-animation-timing-function:cubic-bezier(0,0,1,1);}
ease        ：{-webkit-animation-timing-function:cubic-bezier(0.25,0.1,0.25,1);}）
ease-in     ：{-webkit-animation-timing-function:cubic-bezier(0.42,0,1,1);}）
ease-out    ：{-webkit-animation-timing-function:cubic-bezier(0,0,0.58,1);}）
ease-in-out ：{-webkit-animation-timing-function:cubic-bezier(0.42,0,0.58,1);}
```

#### steps()

> steps(number, position)

##### number

数值。这个很好理解，表示把我们的动画分成了多少段

假设有如下CSS3动画keyframes，定义了一段从0~100px的位移

```
@keyframes move {
    0% { left: 0; }
    100% { left: 100px; }
}
```

![image](https://image.zhangxinxu.com/image/blog/201806/2018-06-11_214140.png)

##### position
关键字。表示动画是从时间段的开头连续还是末尾连续。支持**start**和**end**两个关键字，含义分别如下：

- **start**：表示直接开始。也就是时间才开始，就已经执行了一个距离段。于是，动画执行的5个分段点是下面这5个，起始点被忽略，因为==时间一开始直接就到了第二个点：==
![image](https://image.zhangxinxu.com/image/blog/201806/2018-06-11_223135.png)
- **end**：表示戛然而止。也就是时间一结束，当前距离位移就停止。于是，动画执行的5个分段点是下面这5个，==结束点被忽略==，因为等要执行结束点的时候已经没时间了：
![image](https://image.zhangxinxu.com/image/blog/201806/2018-06-11_223630.png)

> step-start = steps(1, start) & step-end = steps(1, end)

> eg: 假如steps(2, start) 从0-50分割两份 50-100分割两份 忽略开头的第一帧 从第二帧开始执行

```
steps(2, start)
@keyframes move {
  0% { left: 0; }
  50% { left: 50px; }
  100% { left: 100px; }
}
```

```
steps(1, start)
// 执行效果相同
@keyframes move {
  0% { left: 0; }
  25% { left: 25px; }
  50% { left: 50px; }
  75% { left : 75px; }
  100% { left: 100px; }
}
```

#### Animation-Delay

允许您指定动画（或动画片段）何时开始

```
animation-delay: 5s;
```

#### Animation-iteration-count

指定动画播放的次数

- **n**: 一个数字，定义应该播放多少次动画
- **infinite**: 指定动画应该播放无限次（永远）

```
animation-iteration-count: 2;
```

#### Animation-direction

指定动画是应该前进、后退还是交替循环播放

- **normal**(默认): 动画往前播放。在每个循环中，动画重置为开始状态（0％）并再次播放（至100％）
- **reverse**: 动画往后播放。在每个循环中，动画重置为结束状态（100％）并向后播放（至0％）
- **alternate**: 动画每个周期改变一次方向。在每个奇数循环中，动画往前播放（0％到100％），在每个偶数周期中，动画往后播放（100％至0％）
- **alternate-reverse**: 动画每个周期改变一次方向。在每个奇数周期中，动画都会向后播放（100％至0％），在每个偶数周期中，动画都往前播放（0％或100％）

```
animation-direction: alternate;
```

#### Animation-fill-mode

指定在动画播放之前或之后动画样式是否可见。（是否将开头或结尾的样式添加到元素中）

- **backwards**: 在动画之前（动画延迟期间），初始关键帧（0％）的样式应用于元素
- **forwards**: 动画完成后，最终关键帧中定义的样式（100％）由元素保留
- **both**: 动画将遵循向前和向后的规则，在动画之前和之后扩展动画属性
- **normal**(默认): 在动画之前或之后，动画不会将任何样式应用于元素

```
animation-fill-mode: forwards;
```

#### Animation-play-state

指定动画是播放还是暂停。恢复已暂停的动画会从动画暂停的地方开始

- **playing**: 动画正在运行
- **paused**: 动画当前已暂停

```
.div:hover {
  animation-play-state: paused;
}
```

### 简介写法
```
animation: [animation-name] [animation-duration] [animation-timing-function]
[animation-delay] [animation-iteration-count] [animation-direction]
[animation-fill-mode]
```
