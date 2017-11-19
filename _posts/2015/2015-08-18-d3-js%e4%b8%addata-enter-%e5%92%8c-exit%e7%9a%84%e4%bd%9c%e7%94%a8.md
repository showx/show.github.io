---
layout: post
title: d3.js中data(), enter() 和 exit()的作用
date: 2015-08-18 14:47
author: admin
comments: true
categories: []
---
我在刚接触使用d3.js的时候，最感到困惑的一个地方是data(), enter(), exit()这几个操作。

在我接触一段时间，有了一些了解之后，简单说说我的理解。

 

data()
先看一个例子：

<body>
    <p></p>
    <p></p>
    <p></p>
</body>
执行代码：

1
d3.select("body").selectAll("p").data([1, 2, 3])
这里，data()是用来绑定数据到选择的DOM元素上.这样以后，就可以针对这些数据做一些相关操作，比如设置元素宽度等。

从表面上，并不能看出什么变化。但在内部，它是在对应的DOM元素上添加了一个__data__属性，可以通过document.getElementsByTagName("p")[0].__data__看到。

 

enter()和exit()
这两个操作令人困惑是因为单从名字上看，很难推断出它们的作用。

在上面data()的例子中，我们的DOM元素和数据的个数是一样的。但如果不一样的话，我们该怎么办？

enter()和exit()就是用来处理这种情况的。

 

enter()
当DOM数量少于data的数量，或者压根一个都没有的时候，我们一般会希望让程序帮忙创建。

下面的例子，我们没有事先提供DOM元素：

<body>
</body>
仍旧执行：

1
d3.select("body").selectAll("p").data([1, 2, 3])
与上面例子不同的是，上面的例子中我们可以继续执行.style("width", "100px")等操作。但这里我们不能了，因为我们没有选择到DOM元素，需要先创建。

enter()是用来在绑定数据之后，选择缺少的那部分DOM元素。我们可能会疑惑，既然是缺少的部分，怎么选择呢？这里就需要我们发挥一点想象力，想象我们选择了一些不存在的东西。我们可以称之为“虚拟DOM”或“占位符(placeholder)”。

enter()只是进行选择，并未实际添加所需DOM元素。因此在enter()之后一般都会配合append()来进行DOM元素的实际创建。

由此以来，我们使用 d3.select("body").selectAll("p").data([1, 2, 3]).enter().append("p") 即可根据数据自动创建所需的DOM元素。

 

exit()
与enter()相反，exit()是用来选择那些与数据相比多出来的DOM元素。

在下面例子中，我们多提供了一个DOM元素：

<body>
    <p></p>
    <p></p>
    <p></p>
    <p></p>
</body>
这回就容易理解了，因为是多出来的，那么就是实际存在的，即最后一个<p>。

多出来的话，我们可以接着用.remove()移除这些元素，代码如下：

d3.select("body").selectAll("p").data([1, 2, 3]).exit().remove();
