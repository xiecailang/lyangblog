---
layout:     post
title:      "[博客搬家]WPF不同界面之间的通信"
subtitle:   " \"from csdn\""
date:       2016-04-09 12:00:00
author:     "cailang.X"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2015/move-post-bg.jpg?x-oss-process=image"
catalog: true
tags:
    - move
---


# 背景

最近几天在做项目中的一个数据呈现模块，遇到了挺多的问题，主要是界面之间的通信，因为之前也有碰到过，觉得应该记下来。模块功能是呈现多组double数据，包括表格和图标的形式（波形）。

# 显示流程

![这里写图片描述](http://img.blog.csdn.net/20160408171639625)

其中，表格（图表）又需要可以切换显示4中不同数据，如下

![这里写图片描述](http://img.blog.csdn.net/20160408171910298)
# 主界面与子界面通信

程序使用ModerUI,表格和图形与主窗口使用Links关联起来，如下代码

```html
<mui:ModernTab Grid.Row="1" Layout="Tab" SelectedSource="/UserControls/DataViewsOfGrid.xaml" Background="Blue" >
            <mui:ModernTab.Links>
                <mui:Link DisplayName="列表"  Source="/UserControls/DataViewsOfGrid.xaml"/>
                <mui:Link DisplayName="图形"  Source="/UserControls/DataViewsOfChart.xaml"/>   
            </mui:ModernTab.Links>
        </mui:ModernTab>nTab>
```

所有数据在主界面中完成加载

```csharp
//查找数据
  DataBaseHelper dbh = new DataBaseHelper(Environment.CurrentDirectory + ShareValues.projectDir + pi.ProjectName + ".db");
  RelativeDataTable rdt = new RelativeDataTable(dbh.Conn);
  allDatas = rdt.findAll();
  allDatas.Sort(delegate (RelativeData a, RelativeData b) { return a.Railway_Mil.CompareTo(b.Railway_Mil); });
  foreach (RelativeData rd in allDatas)
  {
      curDatas.Add(rd);
  }
  DesignDataTable ddt = new DesignDataTable(dbh.Conn);
  curDesign = ddt.findAll();
  dbh.close();
```

在子界面中根据不同的需要重新组织数据，比如在表格显示的界面重新构造一个DataTable作为DataGrid的数据源。
# 主界面与子界面的数据通信

界面之间的通信采用的方式在前面的文章中有讲到过，用于不同窗口之间的通信，同样也可以用在主界面和子界面的通信里面

子界面注册监听事件
```csharp
public static EventArgsHelper eah;
private void DataViewsOfChart_Loaded(object sender, RoutedEventArgs e)
        {
            if (isVisible)
            {
                showDatas();
                eah.changed += Eah_changed;
            }
            else
            {
                eah.changed -= Eah_changed;
            }
            isVisible = !isVisible;
        }
private void Eah_changed(object sender, EventArgs args)
        {
            showDatas();    
        }
```

主界面发送数据切换指令

```csharp
DataViewsOfGrid.eah.onChanged(new EventMessage<DatasViewType>(DatasViewType.偏差轨枕));
```

# 最终效果
![表格](http://img.blog.csdn.net/20160409104743165)
![图形](http://img.blog.csdn.net/20160409104813666)


——cailang.X 2016-11
