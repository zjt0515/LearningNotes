## 物体的位置与变换

### 移动

二维坐标点(向量)：np.array([x,y,0])
8个方向的向量：UP、RIGHT、LEFT、DOWN、UR、UL、DR、DL
四边：TOP、LEFT_SIDE、RIGHT_SIDE、BOTTOM

默认FRAME_HEIGHT: 8
宽度由长宽比决定



移动：
相对移动`shift(*vector)`
绝对移动`move_to(*vector, aligned_edge: RIGHT)`对齐方式：将物体的右边与该点对齐
移动到角落`to_corner(UL,buff=2)`



对齐：
`align_to(mob, RIGHT)`

`next_to(mob, UP, aligned_edge)`

```
# 你可以通过.animate语法来动画化物件变换方法
        self.play(grid.animate.shift(LEFT))
```

### 缩放旋转翻转拉伸变换



## 几何

`Line(*vector1, *vector2)`

`Arrow(*vector1, *vector2)`



## VGroup()

`add()`
`add_to_back()`
`remove()`

可以对整体移动

arrange(DOWN, center=True, aligned_edge=LEFT)