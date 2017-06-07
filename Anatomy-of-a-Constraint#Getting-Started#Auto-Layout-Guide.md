# 翻译@Auto Layout Guide（自动布局指南）

- 原文：[Auto Layout Guide](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/AutolayoutPG/index.html#//apple_ref/doc/uid/TP40010853)
- 作者：[Apple](https://developer.apple.com/library/content/navigation/)

---

## Getting Started（新手上路）

### Anatomy of a Constraint（约束详解）

视图结构的布局通过一系列线性等式来定义。每个约束代表一个等式。你的目标就是定义这些等式，其有且仅有唯一解。

等式范例如下：

![图5]()

上述约束规定红色视图的前部位于蓝色视图后部后方8pt的位置。代表这个约束的等式由如下部分组成：

- **Item 1（对象1）**。等式中的第一个对象——在这里，即红色视图。等式中的对象必须是视图或布局参照（layout guide）。
- **Attribute 1（属性1）**。对象1被约束的属性——在这里，即红色视图的前部。
- **Multiplier（倍数）**。属性2的值会乘以这个浮点数。在这里，倍数是1.0。
- **Item 2（对象2）**。等式中的第二个对象——在这里，即蓝色视图。与对象1不同，其可以为空。
- **Attribute 2（属性2）**。对象2被约束的属性——在这里，即蓝色视图的后部。如果对象2为空，此处必须是`Not an Attribute`。
- **Constant（常量）**。