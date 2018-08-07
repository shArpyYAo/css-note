# css-note
css相关的读书笔记，做个Research Review

# 《CSS世界》

### 字母x与基线
> 基线：字母x的下边缘线就是基线

> line-height：两基线的间距

> vertical-align：默认值是基线

关于基线有个专门的概念叫作x-height，指的是x字母的高度，术语描述就是基线和等分线之间的距离


vertical-align： middle，在css中，middle指的是基线往上1/2x-height的高度

所以vertical-align：middle实现的垂直居中只是近似效果，只是平时我们使用的字号都是16、18px，看不出来

由基线衍生出来的有个叫ex的单位，在css中是个相对单位，指的就是小写字母x的高度，就是x-height

内联元素默认是基线对齐的，而基线是x的底部，1ex就是一个字母的高度。
假设一个图标的高度就是1ex，同时背景图片居中，可以使得图标和文字天然垂直居中，完全不受字体和字号的影响

#### 例子
HTML：

    zhangxinxu<i class="icon-arrow"></i>
     张鑫旭<i class="icon-arrow"></i>

css代码如下

    .icon-arrow {   
      display: inline-block;  
      width: 20px;   
      height: 1ex;   
      background: url(xxx.png) no-repeat center; 
    } 
    
### 关于line-height
<div>里面包裹文字，<div>的高度不是font-size决定的，是由line-height决定。
对于非替换的纯内联元素，line-height完全决定它的高度，padding、border之类的属性对它的可视高度是完全没有影响的。也就是说“盒模型”是针对块级元素的。
    
#### line-height对内联元素作用的细节
line-height是通过调节文字的行距来控制高度的，在css中，文字的行距是是分散在当前文字的上方和下方的。也就是说，第一行的文字也是有行距的，只不过这个行距的高度只是完整行距的一半，也被称为半行距。

#### 半行距的范围
在css中，行距 = line-height - em-box。有了行距，一分为二，分别在em-box的上面和下面加上，就构成了文字的完整高度。这里有个问题，em-box是什么东西？它的高度正好是1em，1em相当于当前一个font-size的大小。也就是说行距 近似等于 line-height - font-size，
当我们的字体的宋体的时候，内容区域（可以近似理解为ff/ie浏览器下文本选中带背景区域）和em-box是等同的（书上说的，未求证）。于是我们可以利用宋体，准确揪出半行距的范围
##### 测试代码
        
        .test {   
            font-family: simsun;   
            font-size: 80px;   
            line-height: 120px;   
            background-color: yellow; 
         } 
         .test > span {   
            background-color: white; 
         } 
         <div class="test">   
            <span>sphinx</span> 
         </div> 

这里要注意一点，文字的上边距，算出来需要向下取整，文字下边距，算出来需要向上取整。

举个例子来说说line-height对内联元素作用的细节，当line-height为2时，根据公式，行距就是相当于文字的大小。那么半行距就是一半的文字大小，两行文字之间就是一个文字的大小。如果line-height为1时，行距为0.两行文字就会紧密依偎在一起。如果line-height为0.5呢？虽然line-height不支持负值，行距支持负值的。此时两行文字就会重叠纠缠在一起。

#### line-height对替换元素作用的细节
line-height是影响不了替换元素的高度的。来看一段代码

        .box {   line-height: 256px; } 
        <div class="box">   
            <img src="1.jpg" height="128"> 
        </div>
.box的高度之所以是256px，是line-height作用到了幽灵空白节点（书上是这么称呼的，规范里叫stut。宽度为0）身上。图片为内联元素，会构成一个“行框盒子”，再h5文档模式下，每个行框盒子前都有一个幽灵空白节点，它的内联特性与普通字符的一模一样。
在图文混排的情况下，因为同属内联元素，所以会共同形成一个“行框盒子”。line-height在这种情况下只能决定行框盒子的最小高度。

#### line-height对块级元素作用的细节
line-height对块级元素完全不起作用，在工作中我们改变line-height块级元素也跟着改那是因为改变的是块级里面的内联元素的高度。

#### line-height为什么可以让内联元素垂直居中
准确的说法应该是“line-height 可以让单行或多行元素近似垂直居中”。能让元素垂直居中的原因上面也已经说了，因为在css中行距是上下等分的。这里说下为什么是近似。近似的原因是因为文字字形的垂直中线位置普遍要比真正的“行框盒子”的垂直中线位置低。
测试代码：

    p {   
        font-size: 80px;   
        line-height: 120px;   
        background-color: #666;   
        font-family: 'microsoft yahei';   
        color: #fff; 
    }
    <p>微软雅黑</p> 
让单行文本垂直居中line-height就可以搞定了，多行文本的话那还要加上vertical-align属性。
代码如下：

        .box {   
            line-height: 120px;
            background-color: #f0f3f9;
         } 
         .content {   
            display: inline-block;
            line-height: 20px;
            margin: 0 20px;
            vertical-align: middle;
         } 
         <div class="box">   
            <div class="content">基于行高实现的...</div> 
         </div>
实现的步骤大致分两步，
第一步是多行文字用一个标签包住，使用display：inline-block让其保持为内联属性，从而可以设置vertical-align属性。最重要的是生成行框盒子，获得每个行框盒子附带的幽灵空白节点。那line-height就有了用武之地，在.content元素前撑起了高度为120px、宽度为0的内联元素。
第二步是用vertical-align改变ling-height的默认值，line-height的默认值是基线对齐的。从而实现近似垂直居中效果。

### line-height的各类属性值
line-height的默认值是normal，此外还支持数值、百分比以及长度值。

#### line-height：normal
当值为normal时，

| 字体        | Chrome    |  Firefox  |  IE    |
| --------    | -----:    | ----:     |  :----:|
| 微软雅黑     | 1.32   |   1.321   |  1.32    |
| 宋体         | 1.141  |   1.142   |  1.142   |

我们可以看到，只要字体确定，那么各大浏览器的解析值基本上一样。但是，关键是，不同浏览器默认使用的中英文字体时不一样的。所以在开发中，对line-height进行重置是有必要的。在进行重置前，我们先看看line-height的其他数值

> 数值，如line-height:1.5，其终的计算值是和当前font-size相乘后的值。 例如，假设我们此时的 font-size 大小为 14px，则 line-height 计算值是 1.5*14px=21px。 

> 百分比值，如 line-height:150%，其终的计算值是和当前 font-size 相乘后 的值。例如，假设我们此时的font-size大小为14px，则 line-height计算值是 150%*14px=21px。 

> 长度值，也就是带单位的值，如 line-height:21px 或者 line-height:1.5em 等，此处 em 是一个相对于 font-size 的相对单位，因此，line-height:1.5em 终的计算值也是和当前font-size相乘后的值。例如，假设我们此时的font-size 大小为14px，则line-height计算值是1.5*14px=21px。 

这三者之间在继承的细节上有所差别。如果过使用数值作为line-height的属性值，那么所有子元素继承都会是这个值；如果是另外两个值，那么所有子元素继承的是最终的计算结果。
