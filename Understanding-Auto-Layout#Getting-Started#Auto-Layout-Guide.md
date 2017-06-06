# 翻译@Auto Layout Guide（自动布局指南）

- 原文：[Auto Layout Guide](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/AutolayoutPG/index.html#//apple_ref/doc/uid/TP40010853)
- 作者：[Apple](https://developer.apple.com/library/content/navigation/)

---

## Getting Started（新手上路）

### Understanding Auto Layout（理解自动布局）

自动布局（Auto Layout）能够视图的约束，动态计算视图结构中所有视图的尺寸和位置。例如，为一个按钮添加约束，使其水平对齐一个图片视图（image view）；且按钮上部（top）总是位于图片视图下部（bottom）下方8pt（point）的地方。如果图片视图的尺寸或位置发生了变化，按钮的位置会自动调整。

这种以约束为基础的布局方式，允许我们创造出可以动态响应内外部变化的用户界面。

#### External Changes（外部变化）

外部变化源自父视图（superview）的改变。每次变化，都需要更新视图结构，重新规划界面。常见的外部变化来源如下：

- 用户缩放窗口（window）（OS X）。
- 在iPad上，用户进入或离开分割视图（Split View）（iOS）。
- 设备旋转（iOS）。
- 通话或录音时，屏幕上方指示条的初现和消失（iOS）。
- 适配不同尺寸类别（size class）。
- 适配不同屏幕尺寸。

上述改变大多数都发生在运行时，要求app能够动态响应。剩下的，例如适配不同屏幕尺寸，表示app需要主动适应不同的显示环境。自适应界面可以确保app在iPhone 4S，iPhone 6 Plus甚至iPad上显示正常，即使屏幕尺寸不会在运行时改变。同时，自动布局是iPad上支持滑动和分割视图的关键所在。

#### Internal Changes（内部变化）

内部变化源自界面改变时引发的视图或控件尺寸的变化。

常见的外部变化来源如下：

- app展示的内容变化。
- app支持国际化（Internalization）。
- app支持动态字体（Dynamic Type）（iOS）。

app内容变化时，新内容所需的布局很可能与旧的不同，常见于展示文字或图片的app。例如，每篇新闻的长短都不相同，app需要不断根据内容调整布局。类似的，一个拼图软件必须能够处理不同尺寸，比例的图片。

国际化是指配置app，使其能够适应不同语言，地区，文化的过程。支持国际化的app必须考虑上述因素对布局的影响，从而正确显示自己的内容。

国际化对布局的影响主要有三点。其一，翻译界面时，标签（label）的内容会改变，从而改变所占的空间。例如，同一个意思，德语通常比英语更长，而日语更短。

其二，不同地区使用不同格式表示日期和数字，即使地区之间使用同一门语言。虽然相较于语言的区别，这些变化并不明显，用户界面仍然需要做些许调整，以适应变化。

其三，语言的改变不止影响文字尺寸，也会影响文字的排列方式。不同语言的排列方向不同。例如，英语从左至右排列，而阿拉伯语和希伯来语从右至左排雷。总之，界面元素的顺序需要和排列方向吻合。如果使用英语时按钮位于界面的右下角，那么使用阿拉伯语时它就应该在左下角。

最后，如果iOS app支持动态字体，则用户可以改变app使用的字体大小。这也会改变显示文字的界面元素的尺寸。如果用户在运行时改变了字体大小，app的布局必须响应变化。

#### Auto Layout Versus Frame-Based Layout（自动布局 vs. frame布局）

（译者：frame这个词我就不翻译了，没有好的词汇代替🤔）

布局一个界面的元素的方式主要有三种。frame布局，自动缩放（auto resizing）以及自动布局。

以前，app通过为视图结构中每个视图设定frame，排布它们。frame定义了视图在父视图坐标系中的位置，高度，宽度。

![图1](http://ohqrsnfvu.bkt.clouddn.com/auto-layout-guide/%E5%9B%BE1.png)

要准确布局，必须计算每个视图的位置和尺寸。如果布局需要改变，所有相关视图的frame就需要重新计算。

坦白来说，frame布局是最为灵活和强大的。布局可以变成任何你想要的效果。然而，伴随这种灵活和强大而来的，是你必须手动管理所有视图，结果就是即便是一个简单界面，也需要花费许多经历去设计，调试和维护。其难度会随着界面复杂程度增大成倍上升。

自动缩放的出现在一定程度上能够减轻这种负担。自动缩放规定了父视图frame发生变化时，视图本身的frame应该如何变化。如此一来，简化了布局为了响应外部变化所需的工作。

然而，自动缩放支持的布局方式很有限。对于复杂界面来说，自动缩放设置需要通过代码动态修改。此外，自动缩放仅响应外部变化，对于内部变化却无能为力。

尽管自动缩放仅仅是frame布局基础上的优化，自动缩放却代表一种全新的布局设计范式。frame的概念不复存在，要思考的，是视图与视图之间的关系。

自动布局通过约束定义界面。约束一般代表两个视图之间的关系。自动布局根据约束计算视图的位置和尺寸。这样的布局，能够动态响应内部或外部变化。

![图2](http://ohqrsnfvu.bkt.clouddn.com/auto-layout-guide/%E5%9B%BE2.png)

视图的行为通过约束定义，这背后的逻辑与面向过程或面向对象编程大相径庭。幸运的是，掌握自动布局和掌握其他编程技能并无不同。分为两个步骤：首先你必须理解约束布局背后的逻辑，然后熟练使用相关API。在学习其他编程技能的过程中，我们不断重复上述步骤。自动布局也不例外。

本文的剩余部分会帮助你逐步掌握自动布局。章节：[不使用约束的自动布局（Auto Layout Without Constraints）](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/AutolayoutPG/AutoLayoutWithoutConstraints.html#//apple_ref/doc/uid/TP40010853-CH8-SW1)列举一个基于自动布局的高级抽象界面元素，其大大简化了某些场景下自动布局的使用。章节：[Anatomy of a Constraint（约束解析）]()为理解和使用自动布局提供了理论支持。章节：[Working with Constraints in Interface Builder（在界面编辑器中创建布局）](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/AutolayoutPG/WorkingwithConstraintsinInterfaceBuidler.html#//apple_ref/doc/uid/TP40010853-CH10-SW1)讲解自动布局的可视化创建工具。章节：[ Programmatically Creating Constraints（代码创建约束）]()以及章节：[Auto Layout Cookbook（自动布局使用手册）](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/AutolayoutPG/LayoutUsingStackViews.html#//apple_ref/doc/uid/TP40010853-CH3-SW1)细致的讲解了相关API。最后，章节：[Auto Layout Cookbook（自动布局使用手册）](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/AutolayoutPG/LayoutUsingStackViews.html#//apple_ref/doc/uid/TP40010853-CH3-SW1)列举多个自动布局的使用范例，复杂程度各异，以供研究。最后，章节：[Debugging Auto Layout（调试自动布局）](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/AutolayoutPG/TypesofErrors.html#//apple_ref/doc/uid/TP40010853-CH22-SW1)提供了出问题时可用的建议和工具。
