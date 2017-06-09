# 翻译@Auto Layout Guide（自动布局指南）

- 原文：[Auto Layout Guide](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/AutolayoutPG/index.html#//apple_ref/doc/uid/TP40010853)
- 作者：[Apple](https://developer.apple.com/library/content/navigation/)

---

## Getting Started（新手上路）

### Anatomy of a Constraint（约束详解）

视图结构的布局通过一系列线性等式来定义。每个约束代表一个等式。你的目标就是定义这些等式，其有且仅有唯一解。

等式范例如下：

![图5](http://ohqrsnfvu.bkt.clouddn.com/auto-layout-guide/%E5%9B%BE5.png)

上述约束规定红色视图的前部位于蓝色视图后部后方8pt的位置。代表这个约束的等式由如下部分组成：

- **Item 1（元素1）**。等式中的第一个元素——在这里，即红色视图。等式中的元素必须是视图或布局参照（layout guide）。
- **Attribute 1（属性1）**。元素1被约束的属性——在这里，即红色视图的前部。
- **Multiplier（倍数）**。属性2的值会乘以这个浮点数。在这里，倍数是1.0。
- **Item 2（对象2）**。等式中的第二个元素——在这里，即蓝色视图。与元素1不同，其可以为空。
- **Attribute 2（属性2）**。元素2被约束的属性——在这里，即蓝色视图的后部。如果元素2为空，此处必须是`Not an Attribute`。
- **Constant（常量）**。一个浮点数偏移量——在这里，即8.0。这个值会和属性2的值相加。

大多数约束定义界面上两个元素之间的关系。这些元素不是视图，就是布局参照。约束还可以定义同一个元素两个属性之间的关系。例如，为一个元素的宽和高设定比例关系。设置可以直接为元素的宽或高设置常量。设定常量时，第二个元素为空，第二个属性为`Not An Attribute`，倍数为0.0。

### Auto Layout Attributes（自动布局属性）

对于自动布局来说，属性就是可以被约束的特征。总体上，这包括视图的四边（前部，后部，上部，下部），以及宽，高，水平和垂直方向上的中心。文字视图还会有一到两个基准线。

![图6](http://ohqrsnfvu.bkt.clouddn.com/auto-layout-guide/%E5%9B%BE6.png)

所有属性，详见枚举[NSLayoutAttribute](https://developer.apple.com/documentation/uikit/nslayoutattribute)。

>虽然OS X和iOS都使用这个枚举，但它们对某些值的解释不同。所以，查看文档时，要注意区分平台。

### Sample Equations（等式样例）

不同的参数和属性，创造出代表不同约束的等式。例如，定义两个视图之间的间距，让视图一边对齐，定义两个视图之间的相对大小关系，甚至定义视图自身的宽高比。然而，并非所有的属性都可以互相配对使用。

属性分为两类。尺寸属性（例如，宽和高）以及位置属性（例如前部，左部，和上部）。尺寸属性用于定义元素的大小，与位置无关。位置属性用于定义元素的相对位置，与尺寸无关。

了解了它们的区别之后，要记住下述规则：

- 不能根据位置属性约束尺寸属性。
- 不能为位置属性设置常量。
- 不能对位置属性使用"无意义"的倍数（除了1.0以外的值）。
- 不能在垂直位置属性和水平位置属性之间定义约束。
- 同样是位置属性，不能在前部/后部，和左部/右部之间定义约束。

例如，没有其他参考，单单设置一个元素的上部为常量20.0毫无意义。约束位置属性时，必须相对于其他元素定义。再例如，相对于父视图上部下方20pt。然而，将一个元素的高度设定为20.0就完全可行。更多信息，详见[常量的解释]()。

代码3-1给出了一组常见约束的等式。

>本章所有等式都使用伪代码表示。要查看代码实例，详见[代码创建约束](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/AutolayoutPG/ProgrammaticallyCreatingConstraints.html#//apple_ref/doc/uid/TP40010853-CH16-SW1)或[自动布局手册](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/AutolayoutPG/LayoutUsingStackViews.html#//apple_ref/doc/uid/TP40010853-CH3-SW1)。

**代码3-1** 代表常见约束的等式

```
// 设置高度约束
View.height = 0.0 * NotAnAttribute + 40.0
 
// 设置两个按钮之间的间距
Button_2.leading = 1.0 * Button_1.trailing + 8.0
 
// 对齐两个按钮的前部
Button_1.leading = 1.0 * Button_2.leading + 0.0
 
// 让两个按钮宽度相同
Button_1.width = 1.0 * Button_2.width + 0.0
 
// 让一个视图的中心与其父视图对齐
View.centerX = 1.0 * Superview.centerX + 0.0
View.centerY = 1.0 * Superview.centerY + 0.0
 
// 固定视图的宽高比
View.height = 2.0 * View.width + 0.0

```

### Equality, Not Assignment（相等，而非赋值）

请注意，上例中等式两端是相等关系，而非赋值。

自动布局解析这些等式时，并不会将右边的值赋给左边的值。相反，像解方程一样，它会计算等式成立时属性1和属性2的值。这意味着等式中元素的顺序无关紧要。例如，代码3-2同3-1中相应的示例完全一致。

**代码3-2** 颠倒等式中元素的顺序

```
// 设置两个按钮之间的间距
Button_1.trailing = 1.0 * Button_2.leading - 8.0
 
// 对齐两个按钮的前部
Button_2.leading = 1.0 * Button_1.leading + 0.0
 
// 让两个按钮宽度相同
Button_2.width = 1.0 * Button.width + 0.0
 
// 让一个视图的中心与其父视图对齐
Superview.centerX = 1.0 * View.centerX + 0.0
Superview.centerY = 1.0 * View.centerY + 0.0
 
// 固定视图的宽高比
View.width = 0.5 * View.height + 0.0
```

>调整元素顺序时，确保同时调整倍数和常量的值。例如，常量8.0变为-8.0；倍数2.0变为0.5。常量0.0和倍数1.0无需调整。

在自动布局中，同一个问题的解决方式有很多种。理想情况下，选择最能够体现你意图的方式。然而，不同开发者对此的选择毫无疑问会有所不同。不管怎样，选择一种，坚持使用，如此，会避免很多问题。例如，本指南使用下述规则：

1. 倍数最好为整数，而非分数。
2. 常量最好为正数，而非负数。
3. 如果可能，元素应该按照布局中的顺序出现：自上至下，自左至右，自前至后。

### Creating Nonambiguous, Satisfiable Layouts（创建明确，可满足的约束）

自动布局的关键在于代表约束的等式有且仅有唯一解。模糊的约束有一个以上解。无法满足的约束没有解。

总之，视图的尺寸和位置都必须被约束。假设父视图的尺寸已经确定（例如，父视图是iOS中一个场景（scene）的根视图），对于每个子视图来说，要创建一个明确，可满足的布局，每个方向上各需要两条约束（不包括父视图的约束）。然而，至于使用哪些约束，可以有很多选择。例如，下面的三个布局都是明确，可满足的（这里只显示了水平方向上的约束）。

![图7](http://ohqrsnfvu.bkt.clouddn.com/auto-layout-guide/%E5%9B%BE7.png)

- 第一个布局相对于父视图的前部约束视图的前部，并给了视图一个固定宽度。因此，尾部的位置可以根据这两条约束确定。
- 第二个布局相对于父视图的前部约束视图的前部，并相对于父视图的尾部约束视图的尾部。因此，视图的宽度可以根据这两条约束，以及父视图宽度确定。
- 第三个布局相对于父视图的前部约束视图的前部，并将视图和父视图的中心点对齐。因此，视图的宽度和尾部可以根据这两条约束，以及父视图的宽度确定。

注意，每个布局都有一个子视图，两条水平约束。各例中，都完整约束了视图的宽度及水平位置。这意味着水平方向上的布局是明确，可满足的。然而，这些布局的效用并不相同。想象一下如果父视图的尺寸改变，各个布局中视图的尺寸和位置会如何变化。

对于第一个布局，视图的宽度不会改变。大多数时候，这不是我们想要的效果。实际上，一般来说，不应该将视图的尺寸写死（即设定常量）。自动布局是为创建动态布局而生的。如果写死，我们就是去了使用它的意义。

也许很难一眼看出，但第二个和第三个布局的行为是相同的：随着父视图的尺寸变化，视图与其的左右间距不变。然而，这两个布局使用场景并不一定相同。第二个布局一般来说更好理解，但第三个布局可能更有用，特别需要中心对齐一组视图时。再说一次，根据效果选择布局方式。

下面看看复杂一点的情况。假设iPhone上有两个视图要并排显示。二者四周都有留空，并且二者同宽。而且，即使设备旋转，布局不变。

下图显示了它们在设备呈垂直和水平方向时的效果：

![图8](http://ohqrsnfvu.bkt.clouddn.com/auto-layout-guide/%E5%9B%BE8.png)

该怎样添加约束呢？下图的方式简单明了：

![图9](http://ohqrsnfvu.bkt.clouddn.com/auto-layout-guide/%E5%9B%BE9.png)

所使用的约束如下：

```
// 垂直约束
Red.top = 1.0 * Superview.top + 20.0
Superview.bottom = 1.0 * Red.bottom + 20.0
Blue.top = 1.0 * Superview.top + 20.0
Superview.bottom = 1.0 * Blue.bottom + 20.0
 
// 水平约束
Red.leading = 1.0 * Superview.leading + 20.0
Blue.leading = 1.0 * Red.trailing + 8.0
Superview.trailing = 1.0 * Blue.trailing + 20.0
Red.width = 1.0 * Blue.width + 0.0
```

根据之前提到的规则，此布局包含两个视图，意味着有四个水平约束，四个垂直约束。虽然这条规则并不绝对，但能够让你迅速上手。更重要的是，两个视图的尺寸和位置都有且仅有唯一约束，从而让布局明确，可满足。去除任意一条约束，布局会变得模糊。添加额外约束，可能会产生冲突。

当然，布局的方式不仅一种。下述方式同样有效：

![图10](http://ohqrsnfvu.bkt.clouddn.com/auto-layout-guide/%E5%9B%BE10.png) 

没有根据父视图约束蓝色视图的上部和下部，而是根据红色视图。具体约束如下：

```
// Vertical Constraints
Red.top = 1.0 * Superview.top + 20.0
Superview.bottom = 1.0 * Red.bottom + 20.0
Red.top = 1.0 * Blue.top + 0.0
Red.bottom = 1.0 * Blue.bottom + 0.0
 
//Horizontal Constraints
Red.leading = 1.0 * Superview.leading + 20.0
Blue.leading = 1.0 * Red.trailing + 8.0
Superview.trailing = 1.0 * Blue.trailing + 20.0
Red.width = 1.0 * Blue.width + 0.0
```

仍然是两个视图，每个方向上四条约束。生成的布局仍然是明确，可满足的。 

>哪个更好？
>
>两种方式产生的布局都没问题。但哪个更好呢？
>
>不幸的事，无法客观的评判孰优孰劣。每种方式都有其优缺点。
>
>对于第一种方式，如果移除子视图，对布局影响较小。从视图结构中移除视图会同时移除与其相关的所有约束。所以，如果移除了红色视图，蓝色视图就还剩三条约束，只需再添加一条约束即可。而对于第二种方式，红色视图的移除意味着蓝色视图只剩下一条约束。
>
>另一方面，对于第一中方式，如果想要对齐所有子视图的上部和下部，就要保证相对于父视图，所使用的偏移常量都相同。修改一处，其他地方也要修改。

### Constraint Inequalities（不等关系型约束）

目前为止，例子中的所有约束都是相等关系，但这并不是全部。约束也可以通过不等关系定义。所以，约束的关系可是相等，大于等于，或小于等于。

例如，定义视图的尺寸的最大值和最小值（代码 3-3）。

**代码 3-3** 通过约束设置尺寸的最大值和最小值

```
// 设置最小宽度
View.width >= 0.0 * NotAnAttribute + 40.0
 
// 设置最大宽度
View.width <= 0.0 * NotAnAttribute + 280.0
```

一旦使用不等关系，每个视图每个方向上两条约束的规则随之无效。此时，一个相等约束，等同于两个不等约束。代码3-4中，一个相等规则的效果和两个不等规则的效果相同。

**代码 3-4** 用两个不等约束代替一个相等约束

```
// 一个相等约束
Blue.leading = 1.0 * Red.trailing + 8.0
 
// 可以被两个不等约束替代
Blue.leading >= 1.0 * Red.trailing + 8.0
Blue.leading <= 1.0 * Red.trailing + 8.0
```

这种替代方式并不一定可行，因为两个不等约束并不总是等同于一个相等约束。例如，[代码3-3](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/AutolayoutPG/AnatomyofaConstraint.html#//apple_ref/doc/uid/TP40010853-CH9-SW10)中的不等约束限制了视图宽度的范围——但其本身并没有定义宽度的具体值。此时，仍需要定义额外的水平约束，为视图的宽度设置一个上述范围内的具体值。

### Constraint Priorities（约束优先级）



