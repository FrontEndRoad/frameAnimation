### 三种方式实现html5逐帧动画


#### 一、CSS3帧动画
``` css
.monAni { -webkit-animation: monAni .6s steps(6); animation: monAni .6s steps(6); }
@-webkit-keyframes monAni {
	0% { background-position: 0 0; }
	100% { background-position: -812px 0; }
}
@keyframes monAni {
	0% { background-position: 0 0; }
	100% { background-position: -812px 0; }
}
```
**说明：** 使用keyframes定义一组动画，通过animiation引用，简洁高效，动画流畅。前提是需要先拼合图片，另一方面讲也减少了请求数。适用于帧数不多的动画效果。


#### 二、JS改变图片SRC
``` js
function tick(){
	tn++;
	if(tn<=num){
		requestAnimationFrame(tick);
		$('.monkey').attr('src', 'images/m'+tn+'.png');
	}else{
		return;
	}
}
tick();
```
**说明：** 定义一个基数，循环递增，将图片名字与之匹配，满足条件的改变图片src值。这里推荐使用requestAnimation循环，另外使用setTimeout递归做兼容处理。执行动画前，另外，记得先loading图片序列。适用于帧数较多的动画效果；


#### 三、canvas绘制
``` js
img = new Image();
img.src = "images/m1.png";
img.onload = function(){
    update();
};

function update() {
    var delta = Date.now() - lastUpdateTime;
    if (acDelta > msPerFrame){
        acDelta = 0;
        frame++; 
        if (frame >= 6){
            //设置循环
            //frame = 1; 
        }else{
            redraw();
            img.src='images/m'+frame+'.png';
        }
    } else {
        acDelta += delta;        
    }
    lastUpdateTime = Date.now();

    //循环
    requestAnimFrame(update);
}
```
**说明：** 原理同上面的方法二，都是改变图片SRC，这里使用canvas浸染。好处在于避免了图片的多次请求，代码量相对较多。适用于帧数较多的动画效果；


**总结：**个人比较喜欢css3的帧动画，非常轻便，具体还要视项目而定。有更好的方法或者见解，欢迎补充！