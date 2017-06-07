# 翻译@Auto Layout Guide（自动布局指南）

- 原文：[Auto Layout Guide](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/AutolayoutPG/index.html#//apple_ref/doc/uid/TP40010853)
- 作者：[Apple](https://developer.apple.com/library/content/navigation/)

---

## Getting Started（新手上路）

### Auto Layout Without Constraints（无约束的自动布局）

（译者：这里"无约束"指的是不需手动定义约束 🤔）

借助堆叠视图（Stack View），无需手动定义约束，我们就能感受自动布局的强大。堆叠视图适用于定义成行或列布局的界面元素。其根据设置排列元素。

- [axis](https://developer.apple.com/documentation/uikit/uistackview/1616223-axis)：（仅适用于[UIStackView](https://developer.apple.com/documentation/uikit/uistackview)）定义堆叠视图的方向，水平或垂直。
- [orientation](https://developer.apple.com/documentation/appkit/nsstackview/1488950-orientation)：（仅适用于[NSStackView](https://developer.apple.com/documentation/appkit/nsstackview)）定义堆叠视图的方向，水平或垂直。
- [distribution](https://developer.apple.com/documentation/uikit/uistackview/1616233-distribution)：定义既定方向上视图的分布方式。
- [alignment](https://developer.apple.com/documentation/uikit/uistackview/1616243-alignment)：定义与既定方向垂直的方向上的视图的布局方式。
- [spacing](https://developer.apple.com/documentation/uikit/uistackview/1616225-spacing)：定义两个相邻视图之间的间距。

堆叠视图的使用方式很简单：在界面编辑器（Interface Builder）中拖拽一个水平或垂直方向的堆叠视图到画布上。然后将要布局的内容拖入其中。

如果一个视图有固有尺寸（intrinsic content size），其在堆叠视图中保持这个尺寸不变。如果没有，界面编辑器会为其设定一个默认尺寸。你可以缩放视图的大小，界面编辑器会为其添加尺寸约束。

要进一步调整布局，可以使用"属性（Attributes）"栏，修改堆叠视图的属性。例如，下面的例子中，堆叠视图的间距为8pt，分布方式为均匀填充。

![图3]()

堆叠视图在布局内容时，还会参考子视图的内缩（content-hugging）和外扩（compression-resistance）属性。可以在"尺寸（Size）"栏中进行修改。

>通过直接为内容视图添加约束，可以进一步调整布局；然而，要注意避免产生冲突：一般来说，如果是图的尺寸xxxx（未完）。更多约束冲突的相关信息，详见[无法满足的约束](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/AutolayoutPG/ConflictingLayouts.html#//apple_ref/doc/uid/TP40010853-CH19-SW1)。

另外，堆叠视图中还可以嵌套其他堆叠视图，从而构建更为复杂的布局。

![ 图4]()

总而言之，如果可能，尽量使用堆叠视图进行布局。如若不能，再使用约束来达成目标。

更多堆叠视图的相关信息，详见[UIStackView Class Reference](https://developer.apple.com/documentation/uikit/uistackview)或[NSStack View Class Reference](https://developer.apple.com/documentation/appkit/nsstackview)。

>虽然巧妙使用堆叠视图可以创建出复杂界面，但我们仍然无法完全避免约束的使用。至少，你需要通过约束固定堆叠视图本身的位置。