# 简介
Three.js 是一款运行在浏览器中的 3D 引擎，你可以用它创建各种三维场景，包括了摄影机、光影、材质等各种对象。关于Three.js的知识可以在文档中查阅。
[Three.js官网](https://threejs.org/)
[中文文档](http://techbrood.com/threejs/docs/)
## 准备工作
在写代码之前，你需要引入three.js框架包，可以在[这里](https://github.com/mrdoob/three.js)得到，当然也可以使用[我的](https://github.com/zzr716/Threehs_wave)
### 搭台
首先创建Three.js应用必须的:场景,相机,渲染器
```
// 照相机 PerspectiveCamera远景相机
camera=newTHREE.PerspectiveCamera(70,window.innerWidth/window.innerHeight,1,7000);
camera.position.z=600;
// 渲染器
renderer=newTHREE.WebGLRenderer();
renderer.setClearColor(0xffffff);
renderer.setSize(window.innerWidth,window.innerHeight);
container.appendChild(renderer.domElement);
// 场景
scene=newTHREE.Scene();
```
场景，相机，渲染器之间的关系
场景相当于舞台，可以放置需要的角色或物体。相机的作用就是在场景中取一个合适的景，把它拍下来。而渲染器就是将相机拍摄下来的图片，放到浏览器中去显示。他们三者关系如图：

![](http://upload-images.jianshu.io/upload_images/6734924-5c70167023e3532d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 设置光源
```
// 点光源
varlight=newTHREE.PointLight(0xffffff,1.2);
light.position.set(0,500,0);
scene.add(light);
```
在场景中的特定位置创建了一个光源。光线照射在各个方向上（好比一个灯泡）。
### 设置角色
舞台，摄像，灯光都已经准备好了，只差演员们上场
```
 var geometry = new THREE.PlaneGeometry(window.innerWidth - 500, 1600, 3, 50);
 geometry.applyMatrix(new THREE.Matrix4().makeRotationX(- Math.PI / 2 ))
 var material = new THREE.MeshPhongMaterial(
       color: 0x56baed
 });
 plane = new THREE.Mesh(geometry, material);
 plane.position.y = -30;
 scene.add(plane);
```
到这里我们就已经准备好了，就可以进行表演了。此时效果：

![](http://upload-images.jianshu.io/upload_images/6734924-3e8a20c1bb2e20ba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 波动的效果
要实现波动的效果，肯定要使用动画了，而波动的图像第一个浮现在脑海的肯定是正弦/余弦曲线，这里我用的是正弦曲线来实现的。**
**正弦曲线公式**：y=Asin(ωx+φ)+k
A:振幅,曲线最高位和最低位的距离
**ω**:角速度,用于控制周期大小，单位x中起伏的个数
K:偏距,曲线上下偏移量
φ:初相,曲线左右偏移量
曲线图如下：

![](http://upload-images.jianshu.io/upload_images/6734924-c9be35b0038e7ff3.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)y=sinx

```
function animate() {
      var timer = Date.now() - start; 
      for (var i = 0; i < plane.geometry.vertices.length; i++) {
        // vertices用来保存模型中所有顶点位置的数组
        plane.geometry.vertices[i].y = Math.sin( i + timer * y ) * x;
      } 
      // 如果顶点数组更新就设置为true
      plane.geometry.verticesNeedUpdate = true;
      renderer.render(scene, camera);
      requestAnimationFrame(animate);
    }
```
## 最后的效果

![](http://upload-images.jianshu.io/upload_images/6734924-a208f6e6d95e7a91.gif?imageMogr2/auto-orient/strip)

也可以点[这里](https://zzr716.github.io/Threehs_wave/wave.html)体验哦~
最后，本人也是初学者，如果有错误欢迎指出。如果觉得还行，给个小星星呗~
