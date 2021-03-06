# SplitEditTextView
Android类似支付宝**密码输入框**，美团外卖**验证码输入框** ；</br> 
支持**下划线样式**，**单个输入框样式**，**带分割线输入框样式**；</br> 
支持**长按粘贴**；</br>
可设置**光标宽高**、**光标颜色**、**边框大小**、**边框颜色**、**圆角**、**下划线颜色**等等属性(具体可查看下方属性说明)，也可设置输入内容显示模式；</br> 
不能满足需求也可自行将library里面的源码下载下来进行修改。</br> 
源码里面的注释还是比较详细，另外写了一篇关于该库是如何实现的文章，有兴趣的可以阅读([文章链接](https://juejin.im/post/5efaddf25188252e397ec91d))
## 效果图
![image](https://github.com/Chen-keeplearn/SplitEditTextView/blob/other/screenshot/SplitEditTextView_Screenshot_Gif_02.gif)
![image](https://github.com/Chen-keeplearn/SplitEditTextView/blob/other/screenshot/SplitEditTextView_Screenshot_03.jpg)
## 如何使用(可查看demo)
**第一步: 依赖**

首先将SplitEditTextView引入到您的项目中，在build.gradle文件中添加依赖，如下：
``` groovy
dependencies {
   ...
   implementation 'com.open.keeplearn:SplitEditTextView:1.2.3'  
}
```
**第二步: xml中使用**

默认直接弹出键盘
``` xml
<com.al.open.SplitEditTextView
    android:id="@+id/splitEdit1"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_marginTop="20dp"
    android:inputType="number"
    android:textColor="@color/colorPrimary"
    android:textSize="30sp"
    app:borderColor="@color/colorPrimaryDark"
    app:borderSize="2dp"
    app:contentNumber="4"
    app:contentShowMode="password"
    app:cornerSize="10dp"
    app:cursorColor="@color/colorAccent"
    app:cursorWidth="6dp"
    app:inputBoxStyle="singleBox"
    app:spaceSize="20dp" />
```
若默认不弹出键盘，点击弹出键盘，需要按照以前的方式，在SliptEditTextView的parent布局，其它外层布局或者根布局添加两个属性：
``` xml
android:focusable="true"
android:focusableInTouchMode="true"
```
``` xml
<LinearLayout
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:focusable="true"
    android:focusableInTouchMode="true"
    android:gravity="center_horizontal"
    android:orientation="vertical"
    android:padding="16dp">
        
    <com.al.open.SplitEditTextView
        android:id="@+id/splitEdit2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="20dp"
        app:borderColor="@color/colorPrimary"
        app:borderSize="2dp"
        app:contentNumber="5"
        app:contentShowMode="text"
        app:cursorDuration="1000"
        app:inputBoxStyle="underline"
        app:spaceSize="20dp" />

</LinearLayout>
```
**第三步: 代码中实现对内容输入完毕的监听**

在kotlin代码中：
``` kotlin
splitEdit1.setOnInputListener(object : OnInputListener() {
    override fun onInputFinished(content: String?) {
        //内容输入完毕
        Toast.makeText(this@MainActivity,content,Toast.LENGTH_SHORT).show()
    }

    override fun onInputChanged(text: String?) {
        //可选择性重写该方法
    }
})
```
在java代码中：
``` java
splitEditTextView.setOnInputListener(new OnInputListener() {
    @Override
    public void onInputFinished(String content) {
        //内容输入完毕
    }

    @Override
    public void onInputChanged(String text) {
        //可选择重写该方法
    }
});
```
## 使用注意
> #### 1.
> - 使用setInputBoxStyle()动态设置输入框样式时，切换下换线样式，需要添加 </br>
`app:underlineNormalColor="#A8A8A8" app:underlineFocusColor="@android:color/black"`两个属性，避免切换后下划线高亮状态无效 </br>
绘制下划线样式的时，下划线高度使用的是borderSize的大小，颜色是使用这两个属性的颜色值
> #### 2.
> - 长按事件，目前没有做控制
长按时，默认会弹出系统的`textSelectHandleLeft`和`textSelectHandleRight`(复制粘贴时选择文本的图标样式) </br> 
若不需要“复制粘贴”的功能，直接将长按事件屏蔽掉： </br> 
`android:longClickable="false"` 或 `setLongClickable(false);` </br> 
若需要，可将两个属性设置成： </br> 
`android:textSelectHandleLeft="@android:color/transparent"` `android:textSelectHandleRight="@android:color/transparent"`


## 关于属性
| 属性名称 | 属性说明 | 默认值 |
|----------|---------|--------|
| borderSize| 边框宽度 | 1dp |
| borderColor| 边框颜色 | Color.BLACK |
| corner_size| 边框圆角大小 | 0 |
| divisionLineSize| 分割线宽度 | 1dp |
| divisionLineColor| 分割线颜色 | Color.BLACK |
| circleRadius| 实心圆半径 | 5dp |
| contentNumber| 输入内容数量 | 6 |
| contentShowMode| 内容显示模式 | password |
| spaceSize| 输入框间距 | 10dp |
| inputBoxStyle| 输入框样式 | connectBox |
| inputBoxSquare| 输入框是否正方形 | true |
| cursorWidth| 光标宽度 | 2dp |
| cursorHeight| 光标高度 | 输入框高度的一半 |
| cursorColor| 光标颜色 | Color.BLACK |
| cursorDuration| 闪烁时长 | 500 |
| underlineNormalColor| 下划线normal颜色 | Color.BLACK |
| underlineFocusColor| 下划线focus颜色 | 未设置 |

## 更新日志
> #### 1.2.3
> - 修改了与谷歌com.google.android.material:material包属性命名冲突，将cornerSize重新命名为corner_size
> - 修复了一开始不能长按粘贴，需要在输入框内输入内容后，才能有长按粘贴效果的问题
> - 去掉了光标的TextSelectHandle默认样式，长按“选择”的textSelectHandleLeft和textSelectHandleRight样式可查看使用注意项
> #### 1.2.2
> #### 1.2.1
> - 下划线输入框样式下，仿美团外卖，可设置下划线normal和focus两种颜色的区分
> #### 1.2.0
> - 新增有关光标的业务逻辑：光标闪烁、光标颜色、光标宽高

## License
```
Copyright 2020 Chen-keeplearn

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
