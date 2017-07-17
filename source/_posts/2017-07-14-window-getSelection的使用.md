---
title: window.getSelection的使用
categories:
  - 笔记随笔
date: 2017-07-14 15:17:15
tags:
  - js
  - window
---
# 需求
项目中做了一个笔记的功能,需求是选中一段内容后添加笔记,用户写完后将用户写的内容和选中的内容保存起来,在文章中用下滑线标记。
在查看笔记时要看到之前的标记,但是不能改写数据库中原html的内容。
效果是这样的：
{% asset_img 1.png %}
{% asset_img 2.png %}
这里用到了浏览器选取文本信息,选用了window.getSelection这个api.
# window.getSelection
**概述**
返回一个  Selection 对象，表示用户选择的文本范围或插入符号的当前位置。
**语法**

```$xslt
const selection = window.getSelection() ;
```
*selection 是一个 Selection 对象。在页面中用鼠标选中一段内容时, Selection中就会包含所选中部分的信息,包括当前选中的文字内容,
父节点,子节点信息,选中部分的开始点,结束点分别属于的节点,以及在其父节点中的位置等.
详细的内容参考[ window.getSelection ](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/getSelection "window.getSelection")

#解决方案

##第一步是获得做笔记的原文片段
用selection.的tostring方法获得选中的文字内容,是作为存储笔记的原文章片段,
```$xslt
let str = window.getSelection().toString();//获得带有换行符的选中文本
```

##第二步是标记该片段在文章中的位置
首先想到页面加载完成后
获得保存的window.getSelection().toString()得到的片段,在文章中用正则匹配找到该片段
并在该片段前后加上标签做上标记.
但是发现如果取的片段过短,如只是个词时就会在文章中找到多处,也就不对了.
所以这个方案不行,在数据保存时就只能保存该片段的坐标,然后通过位置来找.

然后想到了用window.getSelection().anchorOffset;这个api获取坐标
```
var start = window.getSelection().anchorOffset; 
var end = window.getSelection().focusOffset; 
```
但是现这个起始结束位置是相对于他们所在的父节点的位置而不是你需要的某块内容的位置.
所以我最终采用了先在选取的位置插入一个唯一的id的标签,然后再在整个内容中去找这个唯一id的位置,
然后保存这个位置,在以后需要标记的时候取这个坐标然后再文章中标记.

```$xslt
            self.gS_r = window.getSelection().getRangeAt(0);//获得range对象
            self.gS_r1 = window.getSelection().toString();//获得带有换行符的选中文本
            let temp = document.createElement("u");

            let compLength = document.createElement("u");//计算带标签的实际长度
            let tempText = $('.text').html();

            temp.id="###"+self.noteCount;
            //$(temp).innerHTML=r1;
            $(compLength).append(self.gS_r.cloneContents());////计算带标签的实际长度

            self.gS_r.insertNode(temp);//

            let str1 = '<u id="###'+self.noteCount+'">';
            self.start = $('.text').html().indexOf(str1);
            let str = $(compLength).html();
            let regStart = /^<[\w]+?>/
            let regEnd = /<\/[\w]+?>$/
            if(str){
                for(;regStart.exec(str);){
                    str = str.replace(regStart,'')
                }
                for(;regEnd.exec(str);){
                    str = str.replace(regEnd,'')
                }
            }

            self.end = self.start+str.length;


            tempText = tempText.slice(0,self.end)+'</u>'+tempText.slice(self.end);
            tempText = tempText.slice(0,self.start)+'<u id="###'+self.noteCount+'">'+tempText.slice(self.start);
```
通过坐标在前端加载页面时更改html字符串
实现不修改库中文章,在文章中标记内容.

# 最后
[我的博客](http://hellowangwei.github.io/blog)
