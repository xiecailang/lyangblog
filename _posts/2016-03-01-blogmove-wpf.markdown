---
layout:     post
title:      "[博客搬家]WPF的文件选择与保存"
subtitle:   " \"from csdn\""
date:       2016-03-01 12:00:00
author:     "cailang.X"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2015/move-post-bg.jpg?x-oss-process=image"
catalog: true
tags:
    - move
---
# 引用Windows.Form

![引用window.form](http://img.blog.csdn.net/20160301102529864)

# 打开文件

```csharp
System.Windows.Forms.OpenFileDialog openFileDialog1 = new System.Windows.Forms.OpenFileDialog();
            openFileDialog1.InitialDirectory = "c:\\";
            openFileDialog1.Filter = "(*.mdb)|*.mdb";
            openFileDialog1.FilterIndex = 2;
            openFileDialog1.RestoreDirectory = true;
            if (openFileDialog1.ShowDialog() == System.Windows.Forms.DialogResult.OK)
            {
                //此处做你想做的事
                this.txt_datasource.Text = openFileDialog1.FileName;
                }
```
# 保存文件

```csharp
System.Windows.Forms.SaveFileDialog saveDg = new System.Windows.Forms.SaveFileDialog();
            saveDg.Filter = "(*.xls)|*.xls|(*.xlsx)|*.xlsx";
            saveDg.FileName = tableName+DateTime.Now.ToString("yyyy-MM-dd_hh-mm-ss")+"";
            saveDg.AddExtension = true;
            saveDg.RestoreDirectory = true;
            if (saveDg.ShowDialog() == System.Windows.Forms.DialogResult.OK)
            {
                //此处做你想做的事
                string filePath = saveDg.FileName;
                }
```
# 文件格式过滤

```csharp
//
        // 摘要:
        //     获取或设置当前文件名筛选器字符串，该字符串决定对话框的“另存为文件类型”或“文件类型”框中出现的选择内容。
        //
        // 返回结果:
        //     对话框中可用的文件筛选选项。
        //
        // 异常:
        //   T:System.ArgumentException:
        //     Filter 格式无效。
        [DefaultValue("")]
        [Localizable(true)]
        [SRCategoryAttribute("CatBehavior")]
        [SRDescriptionAttribute("FDfilterDescr")]
        public string Filter { get; set; }
```

——cailang.X 2016-11
