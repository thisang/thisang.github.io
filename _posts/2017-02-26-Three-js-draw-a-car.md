---
layout: post
title: Sample Post
description: "Just about everything you'll need to style in the theme: headings, paragraphs, blockquotes, tables, code blocks, and more."
modified: 2014-12-24
tags: [sample post]
image:
  feature: abstract-3.jpg
  credit: dargadgetz
  creditlink: http://www.dargadgetz.com/ios-7-abstract-wallpaper-pack-for-iphone-5-and-ipod-touch-retina/
---

# Three.js入门-画一辆小车车 #

Three.js是一个3D JavaScript库。使用Three.js可以很轻松的实现三维场景的渲染。

在画一辆小车车之前，你需要先知道了解这些：

**一个典型的Three.js程序至少需要包括渲染器（Renderer）、场景（Scene）、照相机（Camera），以及你在场景中创建的物体。**

## 渲染器 ##

渲染器和Canvas元素绑定。你可以选择自己手动在HTML中定义一个`id`为`mainCanvas`的Canvas元素，也可以让Three.js帮你生成一个。

    // 前者的代码
	var renderer = new THREE.WebGLRender({
		canvas:document.getElementById('mainCanvas')
	})

画小车车的时候我们使用的是前者的方式。
	
	// 后者的代码
	var renderer = new THREE.WebGLRender();
	document.getElementsByTagName('body')[0].appendChild(renderer.doElement);

## 场景（Scene） ##

你在Three.js中创建的物体都需要添加在场景中才能被你看到。

	// 创建场景
	var scene = new THREE.Scene();
	// 把你创建的物体 your_cube 添加到场景
	scene.add(your_cube）;
## 照相机（Camera） ##

你可以把Camera比作你的眼睛，Camera的视角就是你的视角。Three.js和WebGL一样使用的是右手坐标系。

	// 创建一个透视投影照相机并且把它添加到场景中
	var camera = new THREE.PerspectiveCamera(45,400/300,1,10);
	camera.position.set(0,0,5);
	scene.add(camera);

如果你因为调整了Camera的位置之后看不到物体了，你应该加上这么一条语句，让你的Camera一直盯着物体：

	camera.lookAt(new THREE.Vector3(0,0,0));

### 创建一个物体 ###

现在我们来创建一个x、y、z方向长度分别为`1`、`2`、`3`的长方体，并将其设置为红色。

	var cube = new THREE.Mesh(
		new THREE.CubeGeometry(1,2,3),
		new THREE.MeshBasicMaterial({
			color:0xff0000
		})
	);
	// 不要忘记了把它添加到场景中
	scene.add(cube);

### 渲染 ###

前面的工作做好了之后，你就可以渲染创造的三维世界了。

	renderer.render(scene,camera);

### 完整代码 ###

html

	<canvas id="maincanvas"></canvas>
	<script type="text/javascript" src="three.js"></script>

javaScript

	window.onload = function(){
		// 渲染器和canvas元素绑定
		var renderer = new THREE.WebGLRenderer({
			canvas:document.getElementById('mainCanvas')
		})
		renderer.setSize(400,300);
		renderer.setClearColor(0x000000);
		// 创建场景
		var scene = new THREE.Scene();
		// 设置透视投影相机
		var camera = new THREE.PerspectiveCamera(45,400/300,1,10);
		// 设置相机的坐标
		camera.position.set(0,0,5);
		scene.add(camera);
		// 创建物体
		ver cube = new THREE.Mesh(
			new THREE.CubeGeometry(1,2,3),
			new THREE.MeshBasicMaterial({
				color:0xff0000
			})
		);
		scene.add(cube);
		// 渲染
		renderer.render(scene,camera);
	}

好了，在浏览器里打开你的文件，你看到的应该是这个样子。

![](https://mmbiz.qlogo.cn/mmbiz_png/4aY8G03IYiaBicDQ1o7yK3gnMSqAN4RuUNSVZJfSDS6QSysJuxicRaZIibqibemfBXGtKcEFGQ6mbdicGqAFDpBibIhtQ/0?wx_fmt=png)

虽然看起来好像和你想象中的不太一样，但是，它确确实实是3d的。

## 练手-画小车车 ##

现在，让我们来画一个这样的小车车。

![](https://mmbiz.qlogo.cn/mmbiz_png/4aY8G03IYiaBicDQ1o7yK3gnMSqAN4RuUNIwg4dlTnACUAfKDBfFibdLaXmKdcV3ISW0T1x491VzHURCNicxxqsGeg/0?wx_fmt=png)

小车车的车身是一个长方体，四个轮子是个圆环。

长方体的构造函数是`THREE.CubeGeometry()`

	// 车身
	var cube_carBody = new THREE.Mesh(
			new THREE.CubeGeometry(8,4,4),
			// 给你的物体设置Lamber材质，这样你的物体就可以反射光了
			new THREE.MeshLambertMaterial({
			 		color:0xeeeeee
		})
	)
	scene.add(cube_carBody);

设置了物体材质和物体颜色之后，在浏览器中查看，仍然是一片黑，因为你没有添加光源，想象一下，你在一片没有光的环境里能看到什么？你只能看到一片黑。

	// 环境光
	var light = new THREE.AmbientLight(0x666666);
	scene.add(light);

添加完环境光之后，你就可以看到你的物体了，但是并没有3D的感觉，你需要再添加平行光。

	// 平行光
	var directional_light = new THREE.DirectionalLight(0x989898);
	directional_light.position.set(5,6,4);
	scene.add(directional_light);

如果你的物体看起来边缘有锯齿，你需要在你的渲染器中加入下面的语句：

	antialias:true, // 开启消除锯齿，默认false
	precision:"highp" // 渲染精度 highp/mediump/lowp

圆环面的构造函数是`THREE.TorusGeometry(radius, tube, radialSegments, tubularSegments, arc)`

下面是第一个车轮：

	var cube_torusOne = new THREE.Mesh(
			new THREE.TorusGeometry(0.9,0.5,12,30),
			new THREE.MeshLambertMaterial({
			 	color:0x666666
		})
	)
	scene.add(cube_torusOne);

调整车轮的位置：

	cube_torusOne.position.set(2.5,-2.5,2)

接下来的事，你只需要创建其他三个车轮，然后反复调整camera的位置来调整车轮的位置。

	camera.position.set(5,3,5);

[源代码点这里](https://github.com/thisang/ife2017/blob/master/ECharts_and_WebVR/WebGL/01.html "源代码")

[预览点这里](http://htmlpreview.github.io/?https://github.com/thisang/ife2017/blob/master/ECharts_and_WebVR/WebGL/01.html)

改变camera的位置和角度：

![](https://mmbiz.qlogo.cn/mmbiz_png/4aY8G03IYiaBicDQ1o7yK3gnMSqAN4RuUN478OmX2p0VcOAqCVjVic1wP3FiaVaEYMNtCvj2zFSwxSX5g7c7NCTKfg/0?wx_fmt=png)

![](https://mmbiz.qlogo.cn/mmbiz_png/4aY8G03IYiaBicDQ1o7yK3gnMSqAN4RuUNXxc7ibUNPeiar2oWR8GjnBSvqNkJhRaPD7eOV6hW7manj0h3K8ibLicu6A/0?wx_fmt=png)