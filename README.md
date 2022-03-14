# manim-kindergarten 学习笔记 

## manim-kindergarten 介绍

 manim-kindergarten是活跃在b站、github等中国manim爱好者成立的一个技术交流组织，于2020年3月15日正式成立。

| 正经的manim知识增加了！

## 〔manim教程〕第一讲 物体的位置与坐标变换

### 序言

```python
UP = np.array([0, 1, 0])
DOWN = np.array([0, -1, 0])
RIGHT = np.array([-1, 0, 0])
LEFT = np.array([1, 0, 0])
OUT = np.array([0, 0, 1])
IN = np.array([0, 0, -1])
```

### shift + move_to（位移）

**<font color = blue>shift</font>(*vector)**

```python
self.add(mob)
mob.shift(LEFT * 5)
mob.shift(UP * 2 + RIGHT * 3)
mob.shift(DOWN * 2, RIGHT * 2)  # 将所有操作相加后一起位移
```

**<font color = blue>move_to</font>(point_or_mobject, aligned_edge = ORIGIN, coor_mask = np.array([1, 1, 1]))**

```python
self.add(logo)
logo.move_to(LEFT * 5 + UP * 2)
logo.move_to(LEFT * 3, aligned_edge = LEFT)
logo.move_to(LEFT * 3, aligned_edge = RIGHT)
logo.move_to(UP * 2, coor_mask = np.array([1, 0, 1]))
```

### scale（缩放）

```python
self.play(FadeIn(mob))
mob.scale(2)
mob.scale(0.5)
mob.scale(1.5, about_edge = UP)     # 以上边缘为基准缩放
mob.scale(0.3, about_edge = RIGHT)
mob.scale(2.0, about_point = np.array([-2, -2, 0]))

# 播放动画
self.play(mob.scale, 1.5)
self.play(mob.scale,{'scale_factor': 0.6, 'about_edge': LEFT})
```

### rotate（旋转）

**mob.rotate(angle, axis = OUT [, about_point])**

```python
# 旋转右手定则
# axis = OUT 逆时针旋转；axis = IN 顺时针旋转
mob.rotate(90 * DEGREES, axis = OUT)
mob.rotate(180 * DEGREES, axis = OUT)
mob.rotate(270 * DEGREES, axis = OUT)
mob.rotate(360 * DEGREES, axis = OUT)
mob.rotate(90 * DEGREES, axis = IN)
mob.rotate(180 * DEGREES, axis = IN)
mob.rotate(270 * DEGREES, axis = IN)、
mob.rotate(360 * DEGREES, axis = IN)

# 关于 about_point 的拓展应用
# 指定旋转中心点
mob.rotate(angle, axis = OUT, np.array([1, 0, 0]))
mob.rotate(angle, axis = IN, np.array([0.5, 0.5, 0]))

# 关于 rotate_about_origin 函数
# 实际是 about_point 参数取原点 ORIGIN(np.array([0, 0, 0]))
# 旋转中心变化到(0, 0)
mob.rotate_about_origin(angle, axis = OUT)
```

### flip（翻转）

```python
self.add(mob)
mob.flip()
mob.flip(axis = RIGHT)   # 关于自身上下翻转
mob.flip(axis = RIGHT, about_point = ORIGIN)   # 关于屏幕 x 轴上下翻转
mob.flip(axis = UR, about_point = LEFT * 2.5)  # 关于任何指定对称轴翻转                                                  # (y = x - 2.5)
```

### stretch（拉伸变换）

**stretch(*factor, *dim)**   指定拉伸比例，拉伸维度

```python
self.play(FadeIn(mob))
mob.stretch(factor = 2, dim = 0)  # 沿 x 轴方向拉伸 2 倍
mob.stretch(factor = 2, dim = 1)  # 沿 y 轴方向拉伸 2 倍
mob.stretch(factor = 3, dim = 2)  # 沿 z 轴方向拉伸 3 倍，对(x,y)二维无效

# 更多精细操作
mob.stretch(factor = 2, dim = 1, about_point = np.array([-3, -3, 0]))
mob.stretch(factor = 0.5, dim = 1, about_point = LEFT_SIDE + TOP)
mob.stretch(factor = 2, dim = 1, about_edge = UP)
mob.stretch(factor = 0.5, dim = 1, about_edge = DOWN)

# 注意，以上所有关于 mod 的变换均为瞬间完成
# 但为了演示变换过程，视频中执行的是将变换放入
# self.play() 后对应的动画过程
```

### to_corner（移至角落）

```python
mob.to_corner(UL, buff = 0)
for i in range(1, 7):
	mob[i].to_corner(UL, buff = i * 0.5)  # buff 指定距离边界距离
for i in range(0, 7);
	mob[i].to_corner(DL, buff = i * 0.5)

# 注意，所有对 mob 的移动操作，
# 需要 self.add(mob) 才可以显现
# 同理另外还有 UR 和 DR 两个方位
# 不填 buff, 默认为 0.5

# 推至边缘指令
mob.to_edge(UP, buff = 0)    # 将物体推至屏幕最上部
for i in range(1, 7):
	mob[i].to_edge(UP, buff = i * 0.5)
for i in range(0, 8):
	mob2[i].to_edge(LEFT, buff = i * 0.5)
	
# 同样地，所有对 mob 的移动操作，
# 需要 self.add(mob) 才可以显现
# 同理，另外还有 DOWN 和 RIGHT 两个方位
# 不填 buff，默认为 0.5
```

### align_to（对齐操作）

**mob.align_to(moject_or_point, direction)**

moject_or_point 表示对齐的参照物 / 点；

direction 表示对齐的方向，平面上有八个方向（上下左右和四个对角线），采用三维你 ndarray 表示，默认为不做任何对齐。

```python
self.add(mob)
self.add(logo)
mob.align_to(logo)
mob.align_to(logo, RIGHT)  # 将 mob 沿 logo 右边缘对齐
mob.align_to(logo, UR)     # 将 mob 沿 logo 右上角对齐

# 其实 align_to() 还有一个参数 alignment_vect
# 但是这个参数并没有任何作用
```

### next_to（紧挨一个物体，快速安排位置）

```python
c = Circle(radius = 0.5)
sq = Square(side_length = 0.5)
c.next_to(sq)
c.next_to(sq, RIGHT)   # 指定相邻方向，UP / DOWN / RIGHT / LEFT
c.next_to(sq, DOWN, aligned_edge = LEFT)  # 指定相邻方向并沿指定边缘对齐，边缘可指定为 UP / DOWN / LEFT / RIGHT / ORIGIN

c.next_to(sq, DOWN, buff = 0.35)    # 加入一些缓冲区 buff，调整距离

# 对 VGroup 操作
A = VGroup(...)
B = VGroup(...)
B.next_to(A[2], DOWN, aligned_edge = LEFT)

# 另外属性
# submobject_to_align 是自己要对齐的位置
# index_of_submobject_to_align

# 实例：让 B[1] 和 A[2] 对齐
B.next_to(A[2], DOWN, 
	submobject_to_align = B[1], 
	aligned_edge = LEFT)
# 等价写法，传入 VMobject 的列表 A
# 相当于 A 的第 index_of_submobject_to_align 项与 submobject_to_align 对齐
B.next_to(A, DOWN,
	index_of_submobject_to_align = 2,
	submobject_to_align = B[1],
	aligned_edge = LEFT)
```

### set_width + set_height（设置对象尺寸）

```python
# 注意：没有加入参数 stretch 时，set_width 和 set_height 的效果和直接使用 scale 方法类似
img.set_height(height = 5)
img.set_width(width = 3)

# 加入参数 stretch = True / False，则对应地拉伸该对象
img.set_height(
	height = 5,
	stretch = True
	)
img.set_width(
	width = 5,
	stretch = True
	)
```

**万分感谢 manim-kindergarten 的大佬们！**