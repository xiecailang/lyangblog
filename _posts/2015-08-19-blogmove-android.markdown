---
layout:     post
title:      "[博客搬家]android一个简单的自定义表盘"
subtitle:   " \"from csdn\""
date:       2015-08-19 12:00:00
author:     "cailang.X"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2015/move-post-bg.jpg?x-oss-process=image"
catalog: true
tags:
    - Tec
---


# 写在前面

最近在做一个Android软件的第二版本，需要用到一个数字量显示的控件，第一版本是用简单的TextView，想在新版本给用户更好的体验，网上看了一些UI设计，没有满意的，有很酷炫的但是不太符合整体风格，最后决定自己画一个View。

---

# 先图为敬

![](http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2015/pic1.png?x-oss-process=image")

# 自定义View类


```java
import java.text.DecimalFormat;

import com.cstx_railway_works_assistant.R;

import android.content.Context;
import android.content.res.TypedArray;
import android.graphics.Canvas;
import android.graphics.Color;
import android.graphics.Paint;
import android.graphics.RectF;
import android.util.AttributeSet;
import android.view.View;
import android.view.ViewTreeObserver;

/**表盘
 * @author xcl
 *
 */
public class ClockView extends View{

	float x;//圆心坐标x
	float y;//圆心坐标y

	float r;//圆半径
	float width;
	float height;
	double data;//显示数据
	float min;//表盘最小值
	float max;//表盘最大值
	String title;//表盘标题
	int color;
	public ClockView(Context context,AttributeSet attrs)
	{
		super(context, attrs);
		TypedArray a = context.obtainStyledAttributes(attrs,  
                R.styleable.ClockView);

		title = a.getString(R.styleable.ClockView_clockText);
		color = a.getColor(R.styleable.ClockView_clockColor,getResources().getColor(R.color.bg));
		min = a.getFloat(R.styleable.ClockView_clockMin, 0);
		max = a.getFloat(R.styleable.ClockView_clockMax, 0);
		data = a.getFloat(R.styleable.ClockView_clockData, 0);
		a.recycle();
		ViewTreeObserver vto =this.getViewTreeObserver();
		vto.addOnPreDrawListener(new ViewTreeObserver.OnPreDrawListener() {
		   @Override
		    public
		    boolean onPreDraw() {
		       height =ClockView.this.getMeasuredHeight();
		       width =ClockView.this.getMeasuredWidth();
		       return true;
		   }
		});
	}
	@Override
	protected void onDraw(Canvas canvas) {
		// TODO Auto-generated method stub
		super.onDraw(canvas);
		/*初始化位置和半径*/
        x = width/2;
        y = height/2;
        r = x>y?y:x;
        /*画大圆*/
		Paint p = new Paint();  
        p.setColor(getResources().getColor(R.color.bg2));
        p.setAntiAlias(true);// 设置画笔的锯齿效果。
        canvas.drawCircle(x, y, r, p);// 大圆

        /*画扇形*/
        p.setColor(color);         
        RectF oval1=new RectF(x-r*3/4,y-r*3/4,x+r*3/4,y+r*3/4);  
        canvas.drawArc(oval1, 180, 180, false, p);//小弧形


        /*画指针*/
        p.setColor(Color.WHITE);//
        p.setStrokeWidth(3);
        float r1 = r/2;
        double arc = Math.PI*(data - min)/(max - min);
        arc -= Math.PI;
        canvas.drawLine(x, y, (float)(x+r1*Math.cos(arc)), (float)(y+r1*Math.sin(arc)), p);// 斜线               

        /*画小圆*/
        p.setColor(getResources().getColor(R.color.bg2));
        p.setAntiAlias(true);// 设置画笔的锯齿效果。
        canvas.drawCircle(x, y, r/9, p);

        //文字
        p.setColor(Color.WHITE);//
        p.setTextSize(20);
        canvas.drawText(title, x-20, y+30, p);
        p.setTextSize(10);
        canvas.drawText("mm", x+20, y+30, p);
        DecimalFormat df = new DecimalFormat("#.##");
        String dataStr = df.format(data);
        //数值

        p.setTextSize(25);
        if(dataStr.length() <= 4)
        {
        	canvas.drawText(dataStr, x-20, y+55, p);
        }
        else
        {
        	canvas.drawText(dataStr, x-25, y+55, p);
        }

	}

	/**更新数据
	 * @param data
	 */
	public void setData(double data)
	{
		this.data = data;
		this.postInvalidate();
	}
}

```

# 自定义属性

```html
<declare-styleable name="ClockView">
        <attr name="clockText" format="string"/>
        <attr name="clockColor" format="color"/>
        <attr name="clockMin" format="float"></attr>
        <attr name="clockMax" format="float"></attr>
        <attr name="clockData" format="float"></attr>
</declare-styleable>
```

# 前台代码

```html
<com.cstx_railway_works_assistant.views.ClockView		      
			      android:layout_width="200dp"
			      android:layout_height="200dp"
			      clock:clockText="@string/gx"
			      clock:clockColor="@color/myred"
			      clock:clockMin="-5"
			      clock:clockMax="5"
			      clock:clockData="0"
			      android:layout_marginBottom="50dp"

			     />
```

——cailang.X 2016-11
