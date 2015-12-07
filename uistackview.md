#UIStackView--简单用法
####导语：
 > Stack View的核心便是方便垂直或水平排布多个subview，如果你做过Android开发，那它和LinearLayout 控件非常相似。
Stack View最有用的就是它会自动为每个subview创建和添加Auto Layout constraints。当然你可以控制subview的大小和位置。可以通过选项配置subview的大小、排布以及彼此间的间距。

##一、概述
UIStackView类大大简化了用户界面开发。这是好事，特别是随着硬件的改变。使用UIStackView，减少了开发者为简单场景设置枯燥的约束，把繁杂的工作交给了UIKit。
###1. 布局内容
打开Main.Storyboard，选择其中一个Stack View可以查看其选项，并选中一个Stack View。在 Attributes Inspector中，注意Stack View下面列出的选择。
![ww icon](http://cc.cocimg.com/api/uploads/20150623/1435027330575371.png)
Axis表示Stack View的subview是水平排布还是垂直排布。Alignment控制subview对齐方式。Distribution定义subview的分布方式。Spacing 为subview间的最小间距。

把术语简化一下，你可这样理解：Alignment 用于控制X 和 Y值，而Distribution 用于控制高度和宽度。另两个值都会影响对齐。如果选中Baseline Relative将根据subview的基线调整垂直间距。如果选中Layout Margins Relative 将相对于标准边界空白来调整subview位置。

另一个需要记住的是，Stack View会被当成Container View。所以它是一个不会被渲染的UIView子类。它不像其他UIView子类一样，会被渲染到屏幕上。这也意味着设置其backgroundColor属性或重载drawRect:方法都不会产生任何效果。
#### subView和arrangedSubView
开始使用Stack View前，我们先看一下它的属性subViews和arrangedSubvies属性的不同。如果你想添加一个subview给Stack View管理，你应该调用addArrangedSubview:或insertArrangedSubview:atIndex: arrangedSubviews数组是subviews属性的子集。

要移除Stack View管理的subview，需要调用removeArrangedSubview:和removeFromSuperview。移除arrangedSubview只是确保Stack View不再管理其约束，而非从视图层次结构中删除，理解这一点非常重要。
###2. 配置垂直布局的Stack View
打开Main.Storyboard选择上面的Stack View。对Attributes Inspector作如下改变：

Alignment 设为 Center
Distribution 设为 Equal Spacing
Spacing 设为 30
这告诉Stack View为subview添加约束，使其垂直居中并沿Stack View的轴线对称，然后为subview设置边距30。

如果subview大小和Stack View内容区不相符，将根据compression resistance priorities对subview进行拉伸或压缩。在运行时给Stack View添加subview也同样会如此。

布局有任何歧义Stack View都会根据subview在arrangedSubviews中index一步步回退去调整subview的大小，直至其刚好适应Stack View的大小。
###3. 给Stack View添加垂直布局的Subview
添加一个label，一个image view和一个button到上面的Stack View里。label在最上面，image view在中间，button在下面。添加完成后看起来是这样：
![picter icon](http://cc.cocimg.com/api/uploads/20150623/1435027439562460.png)
接下来，我们要在Attributes Inspector里修改一下刚才添加的subview的属性。把label的文本秘诀成“How do you likeour app?”， Text Alignment选择Center 。image view的Image 输入“logo”， Content Mode选Aspect Fit。最后把button的Text 设置成“Add Star!”。

运行app。能看到我们只做了很少工作，但已经添加了三个能响应方向、size class等改变的subview。事实上你并不需手动添加任何约束。

当app运行时，点击Xcode窗口底部Debug View Hierarchy按钮可以进行实时视图调试
![1 icon](http://cc.cocimg.com/api/uploads/20150623/1435027512407710.png)
选择先前添加的任意一个subview。查看size inspector，我们注意到Stack View已经自动为其添加了约束。下图显示的是为button添加的约束
![2 icon](http://cc.cocimg.com/api/uploads/20150623/1435027546248907.png)
###4. 添加
按钮事件还没和app界面关联，我们先关联起来。停止运行app，打开storyboard。创建一个名为addStar的IBAction 关联到按钮的Touch Up Inside事件。
![3 icon](http://cc.cocimg.com/api/uploads/20150623/1435027576854683.png)
给水平布局的Stack View里的星星image添加动画。记住，由于Stack View自动为我们管理Auto Layout constraints，我们只能调用layoutIfNeeded来实现动画。
这在添加更多的星星的时候会引起更多问题。我们希望星星居中显示，而不是被拉伸来适应Stack View的宽度。

修改Alignment 的值为Center ，修改Distribution 的值为Fill Equally。最后在addStar(_:)方法中设置image view的内容。
###5. 嵌套
打开storyboard，添加一个水平布局的Stack View到上面的Stack View里。把它放置在logo之下，按钮之上。
![4 icon](http://cc.cocimg.com/api/uploads/20150623/1435027786839852.png)
把“Add Star!”按钮拖到新添加的Stack View里，再添加一个按钮到新的Stack View里。改变按钮的title为“Remove Star”，文本颜色设为red。预览如下:
![5 icon](http://cc.cocimg.com/api/uploads/20150623/1435027795436834.png)
在Attributes Inspector中编辑新Stack View的属性，改变如下：
Alignment 设为 Center
Distribution 设为 Equal Spacing
Spacing 设为 10
![6 icon](http://cc.cocimg.com/api/uploads/20150623/1435027836351375.png)
###6. 删除
现在既可增加，也可删除了。改变模拟器方向或者旋转设备看看app会怎样调整其界面。我们并未添加一行约束就构建好了app的用户界面。

需要注意的是：removeStar(_:)里调用removeFromSuperview是把subview从视图层级中移除。再次调用removeArrangedSubview(_:)只是告诉Stack View不再需要管理subview的约束。而subview会一直保持在视图层级结构中直到调用removeFromSuperview把它移除。

##三、代码详情
[github](https://github.com/tutsplus/iOS-StackViewFinishedProject)
   
   



