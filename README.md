PathScroller
===============

A Scroller that can compute the "value - time" by a `Path` that extends from Point(0, ±infinite) to (1, ±infinite).
The x coordinate along the `Path` is the "time" and the y coordinate is the "value".
Aim to make a complex-velocity scroll effect.  
The core idea is from `android.support.v4.view.animation.PathInterpolatorCompat`.  
  
一个依据Path来映射“数值-时间”关系的Scroller。Path的X轴代表时间，Y轴代表竖直。通过PathScroller可轻松实现速率变化复杂的Scroll效果。

Example 
-----

![iOS_elastic_scrolling.png](https://raw.github.com/Mixiaoxiao/PathScroller/master/pics/iOS_elastic_scrolling.png)

* Create a `PathPointsHolder`. You are recommended to create a static `PathPointsHolder` and reuse it by different `valueFactor`, because creating the `PathPointsHolder` may cost some time and memory.
	```java
		
		private static final PathPointsHolder sElasticPathPointsHolder = buildElasticPathPointsHolder();
		
		private static PathPointsHolder buildElasticPathPointsHolder() {
			final float baseOverY = 1f; //baseOverY, to reuse the PathPointsHolder by different "valueFactor"(value = baseValue * valueFactor)
			Path path = new Path();
			path.moveTo(0f, 0f);
			path.cubicTo(0.05f, baseOverY * 0.8f, 0.09f, baseOverY * 1.20f, 0.21f, baseOverY * 0.88f);
			path.cubicTo(0.48f, baseOverY * 0.10f, 0.72f, baseOverY * 0.02f, 1f, 0f);
			return new PathPointsHolder(path);
		}
	```

* Use the PathScroller to scroll.
	```java
		PathScroller scroller = new PathScroller();
		final float velocityY = ...//the velocity you computed 
		final float overY = velocityY * 0.07f; //compute the max over-scroll y
		final float baseOverY = 1f;//the baseOverY when you created the PathPointsHolder
		final float valueFactor = overY / baseOverY;//the valueFactor to "scale" the baseOverY
		final int duration = 500;//milliseconds
		scroller.start(valueFactor, duration, sElasticPathPointsHolder); 
	```
	
* To track the changing value, use computeScrollOffset. Same as `android.widget.Scroller`, for example:
	```java
		if (scroller.computeScrollOffset()) {
			int value = mScroller.getCurrX();// get current value
			//handle the value...
		}
    ```

OverScroll-Everywhere
-----

* [OverScroll-Everywhere](https://github.com/Mixiaoxiao/OverScroll-Everywhere)


Developed By
------------

Mixiaoxiao(谜小小) - <xiaochyechye@gmail.com> or <mixiaoxiaogogo@163.com>



License
-----------

    Copyright 2016 Mixiaoxiao

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
