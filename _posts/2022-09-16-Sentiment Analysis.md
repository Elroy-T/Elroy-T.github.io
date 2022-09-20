---
title: "Markup: Image Alignment"
image: 
  path: /images/sample.jpg
  thumbnail: /images/simple.jpg
  caption: "Photo from [WeGraphics](http://wegraphics.net/downloads/free-ultimate-blurred-background-pack/)"
categories:
  - Markup
tags:
  - alignment
  - captions
last_modified_at: 2018-02-05T16:19:20-05:00
---

The best way to demonstrate the ebb and flow of the various image positioning options is to nestle them snuggly among an ocean of words. Grab a paddle and let's get started.

Assign classes with HTML:


#文字效果：#

标题：
#Taylor#
##Taylor##
###Taylor###

加粗：**left aligned**

突出：`.align-left`

超链接：[inline attribute lists](https://kramdown.gettalong.org/syntax.html#inline-attribute-lists):

代码：

```html
<img src="image.jpg" class="align-left" alt="">
<img src="image.jpg" class="align-center" alt="">
<img src="image.jpg" class="align-right" alt="">
```

```markdown
![left-aligned-image](image.jpg){: .align-left}
![center-aligned-image](image.jpg){: .align-center}
![right-aligned-image](image.jpg){: .align-right}
```


#图片：#

##不带评论（markdown）：##

###居中：

![image-center]({{ '/images/image-alignment-580x300.jpg' | absolute_url }}){: .align-center}

###左对齐：

![image-left]({{ '/images/image-alignment-150x150.jpg' | absolute_url }}){: .align-left} The rest of this paragraph is filler for the sake of seeing the text wrap around the 150×150 image, which is **left aligned** with the `.align-left` class.

###左右对齐不溢出：

![no-alignment]({{ '/images/image-alignment-1200x4002.jpg' | absolute_url }})

###右对齐：

![image-right]({{ '/images/image-alignment-300x200.jpg' | absolute_url }}){: .align-right}

##带评论（html）：

###居中：

<figure class="align-center">
  <a href="#"><img src="{{ '/images/image-alignment-580x300.jpg' | absolute_url }}" alt=""></a>
  <figcaption>Look at 580 x 300 <a href="#">getting some</a> love.</figcaption>
</figure> 

###左对齐：

<figure style="width: 150px" class="align-left">
  <img src="{{ '/images/image-alignment-150x150.jpg' | absolute_url }}" alt="">
  <figcaption>Itty-bitty caption.</figcaption>
</figure> 

###左对齐可溢出（大图）：

<figure style="width: 1200px">
  <img src="{{ '/images/image-alignment-1200x4002.jpg' | absolute_url }}" alt="">
  <figcaption>Massive image comment for your eyeballs.</figcaption>
</figure> 

###右对齐

<figure style="width: 300px" class="align-right">
  <img src="{{ '/images/image-alignment-300x200.jpg' | absolute_url }}" alt="">
  <figcaption>Feels good to be on the right.</figcaption>
</figure> 




