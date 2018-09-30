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

## 页面基本结构

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>排序动画演示</title>
</head>
<body>
	<div class="container">

	</div>
</body>
</html>

```
## 给待排序元素加个样式
```
.container{
    text-align:center;
}
.sort{
    position:absolute;
    width:50px;
    height:50px;
    line-height:50px;
    border:1px solid black;
    transition:1s; 
}

```

> 为什么绝对定位呢，首先绝对定位可以让元素脱离文档流，能够尽量减少重排，而且绝对定位的位置方便计算，所以这里采用绝对定位，当然 fixed 也是可以的。然后最重要的就是动画的持续时间设置为 1s

## 将待排序元素输出到界面

```
var arr = [5,4,8,9,6,5,4,12,3,6,7,8,56];
var container = document.querySelector('.container');
var fragment = document.createDocumentFragment(); // 创建文档片段，尽量减少重绘和重排
var len = arr.length;
for(let i = 0; i < len; i++ ){
    var node = document.createElement('div');
    node.className = 'sort';
    node.id = i; // 这个后面移动位置的时候需要用到
    node.style.left = i * 60 + 'px';
    fragment.append(node);
}
container.append(fragment);

```
>至此，待排序元素输出到页面如下
![图](https://raw.githubusercontent.com/dcpnonstop/Sort-the-animation/master/sort.jpg)

## 采用冒泡排序处理排序
```
for(let i = 0; i < len; i++){
    for(let j = 0; j < len - i; j++){
    	if(arr[j] > arr[j+1]){
    	    // 这里使用了 ES6 的解构赋值，即交换两个元素的值
    		[arr[j],arr[j+1]] = [arr[j+1],arr[j]];
    		// 也可以这样
    		/*
    		    var temp = arr[j];
    		    arr[j] = arr[j+1];
    		    arr[j+1] = temp;
    		*/
    	}
    }
}

```
>解构赋值还是很好用的，推荐使用结构赋值那么我们要实现冒泡排序的动画该怎么办呢。
首先我们要获取交换的两个元素距离左边长度，然后交换这两个元素的位置，还记得我们之前给元素赋值了 ID 吗，我们可以通过 ID 来找到这两个元素。
```
var x = document.getElementById(j)	
var y = document.getElementById(j+1);
// 这里同样采用解构赋值
[x.style.left,y.style.left] = [y.style.left,x.style.left];
// 记得 id 也要交换
[x.id,y.id]=[y.id,x.id];

```

>至此，我们做完了该做的一切，但是直接把这段代码加入到冒泡排序里面的话那我们直接看到的就是排序完成的效果了，看不到中间的过程，那要怎么样才能看到排序的过程呢，这个时候我们可以使用 setTimeout。

**冒泡部分的代码如下**
```
var time = 1;
for(let i = 0; i < len; i++){
	for(let j = 0; j < len - i; j++){
		if(arr[j] > arr[j+1]){
			[arr[j],arr[j+1]] = [arr[j+1],arr[j]];
			setTimeout(function(){
				var x = document.getElementById(j)	
				var y = document.getElementById(j+1);
				[x.style.left,y.style.left] = [y.style.left,x.style.left];
				[x.id,y.id] = [y.id,x.id];
			},time * 1000)
			time++;
		}

	}
}

```
>time 是为了让每次的效果都显示出来，如果只是 1000 的话，那么这个动画 1s 之内就会完成，如果不清楚可以复习一下事件循环的相关知识。

---

## 完整代码

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>排序</title>
	<style>
		.container{
			text-align: center;
		}
		.sort{
			transition: 1s;
			height: 50px;
			width: 50px;
			border: 1px solid black;
			line-height: 50px;
			position: absolute;
		}
	
	</style>
</head>
<body>
	<div class="container">
	</div>
	<script>
		var arr = [5,4,8,9,6,5,4,12,3,6,7,8,56];
		var container = document.querySelector('.container');
		var fragment = document.createDocumentFragment();
		var len = arr.length;
		for(let i = 0; i < len;i++){
			var temp = document.createElement('div');
			temp.className = 'sort';
			temp.style.left = i*60 +'px';
			temp.id = i;
			temp.innerHTML = arr[i];
			fragment.append(temp);
		}
		container.append(fragment);
		var time = 1;
			for(let i = 0; i < len; i++){
				for(let j = 0; j < len - i; j++){
					if(arr[j] > arr[j+1]){
						[arr[j],arr[j+1]] = [arr[j+1],arr[j]];
						setTimeout(function(){
							var x = document.getElementById(j)	
							var y = document.getElementById(j+1);
							[x.style.left,y.style.left] = [y.style.left,x.style.left];
							[x.id,y.id] = [y.id,x.id];
						},time * 1000)
						time++;
					}
				}
			}
	</script>
</body>
</html>

```
----

<br>

[在线预览](https://dcpnonstop.github.io/Sort-the-animation/paixu3.html)
