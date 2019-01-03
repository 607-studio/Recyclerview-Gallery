# README #

[![Download](https://img.shields.io/badge/Download-V1.1.2-blue.svg)](https://bintray.com/ryanlijianchang/maven/RecyclerView-Gallery)
[![License](https://img.shields.io/badge/license-Apache2.0-green.svg)](https://github.com/ryanlijianchang/Recyclerview-Gallery)
[![Build](https://img.shields.io/circleci/project/github/RedSparr0w/node-csgo-parser.svg)](https://github.com/ryanlijianchang/Recyclerview-Gallery)

使用RecyclerView实现Gallery效果。实现效果如下：

![小清新的Gallery水平滑动效果](https://user-gold-cdn.xitu.io/2017/12/13/1604f61b7219464a?w=201&h=358&f=gif&s=3031397)

![小清新的Gallery垂直滑动效果](https://user-gold-cdn.xitu.io/2017/12/13/1604f61b781841cc?w=206&h=366&f=gif&s=2045166)


# 用法 #

首先，在你的`build.gradle`中添加依赖。

    compile 'com.ryan.rv_gallery:rv-gallery:1.1.2'

第二，在你的layout文件中使用`GalleryRecyclerView`。

	<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:app="http://schemas.android.com/apk/res-auto"
	    android:id="@+id/rl_container"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent"
	    android:background="@color/colorPrimary"
	    android:fitsSystemWindows="true"
	    android:gravity="center"
	    android:orientation="vertical">
	
		<!-- PagerSnapHelper一次只能滑动一页 -->
		<!-- LinearSnapHelper一次能滑动多页 -->
	    <com.ryan.rv_gallery.GalleryRecyclerView
	        android:id="@+id/rv_list"
	        android:layout_width="match_parent"
	        android:layout_height="480dp"
        	app:helper="PagerSnapHelper/LinearSnapHelper" />

	</RelativeLayout>

第三，在代码中像使用普通的RecyclerView一样，初始化你的GalleryRecyclerView。LayoutManager必须使用LinearLayoutManager，同时在创建LinearLayoutManager需要指定你的方向为`HORIZONTAL`或者`VERTICAL`，让`GalleryRecyclerView`水平或者垂直方向滑动。

	GalleryRecyclerView mRecyclerView = findViewById(R.id.rv_list);
	RecyclerAdapter adapter = new RecyclerAdapter(getApplicationContext(), getDatas());
	mRecyclerView.setLayoutManager(new LinearLayoutManager(this, LinearLayoutManager.HORIZONTAL, false));
    mRecyclerView.setAdapter(adapter);

最后，指定`GalleryRecyclerView`的参数（非必须，不指定的话会使用默认值），最后必须调用setUp()方法才生效。

    mRecyclerView
            // 设置滑动速度（像素/s）
            .initFlingSpeed(9000)
            // 设置页边距和左右图片的可见宽度，单位dp
            .initPageParams(0, 40)
            // 设置切换动画的参数因子
            .setAnimFactor(0.1f)
            // 设置切换动画类型，目前有AnimManager.ANIM_BOTTOM_TO_TOP和目前有AnimManager.ANIM_TOP_TO_BOTTOM
            .setAnimType(AnimManager.ANIM_BOTTOM_TO_TOP)
            // 设置点击事件
            .setOnItemClickListener(this)
            // 设置自动播放
            .autoPlay(false)
            // 设置自动播放间隔时间 ms
            .intervalTime(2000)
            // 设置初始化的位置
            .initPosition(1)
            // 在设置完成之后，必须调用setUp()方法
            .setUp();

在你需要释放资源的地方调用release()方法，释放资源：


    @Override
    protected void onDestroy() {
        super.onDestroy();
        if (mRecyclerView != null) {
            // 释放资源
            mRecyclerView.release();
        }
    }

# API #

**Java API**

1. `initFlingSpeed(int speed)`：修改滑动速度（像素/s）
2. `setAnimFactor(float factor)`：修改切换动画的参数因子
3. `setAnimType(int type)`：配置动画类型 //ANIM_BOTTOM_TO_TOP、ANIM_TOP_TO_BOTTOM
4. `setOnItemClickListener(OnItemClickListener mListener)`：设置点击事件
5. `initPageParams(int pageMargin, int leftPageVisibleWidth)`：动态配置页边距和左右页可视宽度/高度
6. `getScrolledPosition()`：获取当前位置
7. `getLinearLayoutManager()`：获取LayoutManager
8. `getOrientation()`：获取当前的滑动方向 HORIZONTAL:0 VERTICAL:1
9. `autoPlay(boolean)`：是否自动播放
10. `intervalTime(int interval)`：自动播放间隔时间，单位ms
11. `initPosition(int position)`：开始处于的位置

**XML API**

1. `app:helper="PagerSnapHelper/LinearSnapHelper"`：PagerSnapHelper一次只能滑动一页，LinearSnapHelper一次滑动多页。

# 实现 #

具体实现过程已在掘金上发布了，如果你感兴趣，可以跳转到[这里](https://juejin.im/post/5a30fe5a6fb9a045132ab1bf)。如果你觉得可以帮助到你，不妨点个Star。

# 版本特性 #

查看更多，请转移至[Releases](https://github.com/ryanlijianchang/Recyclerview-Gallery/releases)。

**V1.1.2**
1. BUG FIX。Fix By @[tingshuonitiao](https://github.com/tingshuonitiao)修复第一次进入时第0张图片leftPageVisibleWidth不展示的bug

**V1.1.1**

1. BUG FIX。修复第一次初始化时设置默认位置不对的问题。提供接口initPosition(int pos)设置初始位置，初始后可以直接调用smoothScrollToPosition(int pos)移动到需要的位置。
2. 更换设置ScrollListener时机。

**V1.1.0**

1. BUG FIX。修复在Fragment上使用出现的异常问题。
2. 增加自动播放接口。

**V1.0.9**

1. BUG FIX。修复滑动动画不顺畅问题。

**V1.0.8**

1. BUG FIX。修复从其他图片切换到第一张图片时抖动的问题。

**V1.0.7**

1. BUG FIX。修复横竖屏切换时UI异常问题。

**V1.0.6**

1. BUG FIX By @[jefshi](https://github.com/jefshi)。去除单例，修复前后台切换后，位置不对的问题。
2. BUG FIX By @[jefshi](https://github.com/jefshi)。修复接受到微信、QQ等消息通知后，图片高度不对问题。
3. Gradle升级V4.4。

**V1.0.5**

1. BUG FIX。修复了GalleryRecyclerView从前台切换到后台时闪退，位置错乱等问题。

**V1.0.4**

1. 增加helper属性，包括LinearySnapHelper和PagerSnapHelper。

**V1.0.3**

1. 修复了移动一页理论消耗距离应该是图片宽度加上2倍页边距。
2. 修复了修改页边距和可视宽度之后，没有生效。

**V1.0.2**

1. BUG FIX。修复LayoutManager使用非LinearyLayoutManager时不抛出异常。 

**V1.0.1**

1. BUG FIX。首次打开，获得焦点后滑动至第0项，避免第0项的margin不对。

**V1.0.0**

1. GalleryRecyclerview支持实现Gallery效果。
2. 支持动态修改滑动速度（像素/s）。
3. 支持动态修改切换动画的参数因子。
4. 支持配置动画类型。
5. 支持点击事件。
6. 支持动态配置页边距和左右页可视宽度/高度。



# License #

    
    Copyright 2017 ryanlijianchang
    
    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at
    
       http://www.apache.org/licenses/LICENSE-2.0
    
    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.