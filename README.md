# Sort-the-animation
# 携程前端模拟排序动画,效果如下
![图](https://user-gold-cdn.xitu.io/2018/9/5/165a9b666b23e820?w=750&amp;h=120&amp;f=gif&amp;s=1675334)
---

<br>

[第一种实现方式预览](https://dcpnonstop.github.io/Sort-the-animation/paixu.html)
<br>

[第二种实现方式预览](https://dcpnonstop.github.io/Sort-the-animation/paixu1.html)
<br>

[第三种实现方式预览](https://dcpnonstop.github.io/Sort-the-animation/paixu2.html)
<br>

[第四种实现方式预览](https://dcpnonstop.github.io/Sort-the-animation/paixu3.html)

---
赞一下携程的这道题目，这才是前端该做的题目，既有意思，又考察了排序算法，还考察了部分动画及 dom 操作。话不多说，分析一下这道题目。
---

### 实现思路
1. 通过排序把每一步的交换序列放入 sortDetail 中（后续位置会发生变化，所以要用 mapper 数组记录当前对应的是第几个元素）
2. 用定时器的方式触发动画（我这里是通过改 marginLeft 属性，然后设置 transition）
