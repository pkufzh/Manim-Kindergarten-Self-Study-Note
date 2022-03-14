# manim学习笔记

## 第一个 Scene

manim 通过实例化 *Scenes* 来生成视频。这些 *Scenes* 实际上是一系列特殊 *类* 的实例，它们都拥有一个用以描述动画的 `construct` 方法。(为了方便本教程的描述，这里没有顾及读者是否熟悉 Python 或诸如 *类* 、 *方法* 等面向对象语言的专业术语，不过如果读者想继续学习 manim，有必要先学习一下 Python 编程基础。)

运行下面的例子，查看输出的视频。

```python
%%manim -v WARNING -qh CircleToSquare

class CircleToSquare(Scene):
    def construct(self):
        blue_circle = Circle(color=BLUE, fill_opacity=0.5)
        green_square = Square(color=GREEN, fill_opacity=0.8)
        self.play(Create(blue_circle))
        self.wait()
        self.play(Transform(blue_circle, green_square))
        self.wait()
```

首先，

```python
%%manim -v WARNING -qm CircleToSquare
```

是一个 *魔术指令 (magic command)*，该指令仅对 `Jupyter notebook` 有效。这条命令等价于在终端调用 manim 命令。

`-v WARNING` 将隐去所有暂时用不到的输出信息 (如果你想看这些信息，可以将命令中的这部分代码去掉)。标志 `-qm` 控制输出视频的画面质量，它是 `--quality=m` 的缩写，即中等质量。这意味着输出的视频将为 30帧 720p。(可以尝试将其改为 `-qh` 或 `-ql` 分别代表 *高 (high)* 、 *低 (low)* 质量。)

`CircleToSquare` 为 *Scene* 类实例化之后的名称。之后几句分别为：

```python
class CircleToSquare(Scene):
    def construct(self):
        [...]
```

`def` 后面的这个整体定义了这个名为 `CircleToSquare` 的 manim 实例，该实例中的 `construct` 方法中精确描述了将展现怎样的动画内容及效果，其可被看做是一张描绘着该动画内容及效果的 *蓝图*。

```python
blue_circle = Circle(color=BLUE, fill_opacity=0.5)
green_square = Square(color=GREEN, fill_opacity=0.8)
```

前两行分别创建了名为 `Circle` 和 `Square` 的两个 Mobject 对象，它们分别拥有各自的颜色及填充透明度。 但是，它们尚未加入到动画场景中。为了展现，需要用到 `self.add`，或者 ...

```python
self.play(Create(blue_circle))
self.wait()
```

... 将一个 manim 对象 (*Mobject*) 添加到场景中的过程以动画形式展现出来。在该方法中 `self` 代表的是当前场景，`self.play(my_animation)` 可以解释为："*该场景中需要演示我的动画*" 。

`Create` 就是一个动画，不过这里还有很多其他类型的动画 (例如 `FadeIn`，或者 `DrawBorderThenFill`，可以自行尝试!)。调用 `self.wait()` 的目的就如你想的那样，让动画画面暂停一会儿 (默认是 1 秒)。如果改为 `self.wait(2)`，将暂停两秒。

最后两行，

```python
self.play(Transform(blue_circle, green_square))
self.wait()
```

用于展现从蓝色圆圈到绿色方块的变化过程 (并加上 1 秒的暂停)。