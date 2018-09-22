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
div里面包裹文字，div的高度不是font-size决定的，是由line-height决定。
对于非替换的纯内联元素，line-height完全决定它的高度，padding、border之类的属性对它的可视高度是完全没有影响的。也就是说“盒模型”是针对块级元素的。
    
#### line-height对内联元素作用的细节
line-height是通过调节文字的行距来控制高度的，在css中，文字的行距是是分散在当前文字的上方和下方的。也就是说，第一行的文字也是有行距的，只不过这个行距的高度只是完整行距的一半，也被称为半行距。

#### 半行距的范围
在css中，行距 = line-height - em-box。有了行距，一分为二，分别在em-box的上面和下面加上，就构成了文字的完整高度。这里有个问题，em-box是什么东西？它的高度正好是1em，1em相当于当前一个font-size的大小。也就是说行距 近似等于 line-height - font-size，
当我们的字体的宋体的时候，内容区域（可以近似理解为ff/ie浏览器下文本选中带背景区域）和em-box是等同的（书上说的，未求证）。于是我们可以利用宋体，准确揪出半行距的范围

测试代码
        
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

#### line-height的各类属性值
line-height的默认值是normal，此外还支持数值、百分比以及长度值。

##### line-height：normal
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

#### 内联元素line-height的“大值特性”
先来看一段代码

    <div class="box">   
        <span>内容...</span> 
    </div>
css不一样，分别是：

    .box {   
        line-height: 96px; 
    } 
    .box span {   
        line-height: 20px; 
    }
和

    .box {   
        line-height: 20px; 
     } 
     .box span {   
        line-height: 96px; 
     }
问题来了，box的高度是多少？第一种情况是父级设置的96px，按照经验，子元素会覆盖父级的，那么line-height应该是20px。第二种情况则是96px。

很可惜，这样是错的（我刚开始看的时候也是这样想的）。结果是box的高度都是96px。就是说，无论line-height如何设置，最终父级元素的高度都是由数值最大的linge-height决定的。这是因为，幽灵空白节点的存在。

<span>是内联元素，内联元素组成内联盒子，内联盒子组成行框盒子。注意，行框盒子前面都会有幽灵空白节点。当父级设置line-height为96px时，幽灵空白节点高度为96px。我们知道，行框盒子的高度是由最高的那个内联盒子决定的，这就是box元素永远是最大的line-height的原因。那么，要避免幽灵空白节点的干扰，可以设置<span>元素为inline-block，创建一个独立的行框盒子，这样子我们设置给<span>的line-height就可以对幽灵空白节点生效了。这也是多行文字垂直居中示例中这么设置的原因。
    
### vertical-align
vertical-align的属性值分为一下4类：
> 线类，如baseline（默认值）、top、middle、bottom;

> 文本类，如text-top、text-bottom;

> 上标下标类，如sub、super;

> 数值百分比类，如20px、2em、20%等

line-height的默认值是基线对齐的，也就是baseline。而vertical-align是改变line-height对齐的位置，它的默认值也是baseline。vertical-align:baseline等同于vertical-align:0。

vertical-align的百分比值是相对于line-height计算的。

#### vertical-align 作用的前提 
vertical-align只能作用于display计算值为：inline、inline- block，inline-table或table-cell的元素上。因此，默认情况下，span、strong、em等内联元素，img、button、input等替换元素，非 HTML 规范的自定义标签元素，以及td单元格，都是支持vertical-align属性的，其他块级元素则不支持。 

像float、position：absolute，这类会改变display计算值的css属性或属性值，会使vertical-align无效的，这里要注意下。
而且，要注意，幽灵空白节点的存在，如：
css：

    .box {   height: 128px; } 
    .box > img {   height: 96px;   vertical-align: middle; } 
html：

    <div class="box">   <img src="1.jpg"> </div>
    
上面的代码图片顶着box元素上边缘显示，根本没有垂直居中。这是因为幽灵空白节点太小了，如果加上line-height，设置成足够高的高度，vertical-align就生效了。

#### vertical-align 和 line-height的关系
明显的就是vertical-align的百分比值是相对于line-height计算的，这里举一个例子：

先看下相关代码

    .box { line-height: 32px; } 
    .box > span { font-size: 24px; } 
    <div class="box">   
        <span>文字</span> 
    </div>
    
这里box的高度是36而非设置的line-height高度32，原因是我们对span里面的文字设置了字号，但是，span外面.box里面却没有。大家应该还记得“strut”幽灵空白节点，为了看的清楚点，我们可以这样：

    .box { line-height: 32px; } 
    .box > span { font-size: 24px; } 
    <div class="box">   
        x<span>文字x</span> 
    </div>
    
此时，差别就看得出来了，span里的x与span外的x明显不一样，它们的高度都一样都是32px，但是字号不一样，font-size 越大字符的基线位置越往下，因为文字默认全部都是基线对齐，所以当字 号大小不一样的两个文字在一起的时候，彼此就会发生上下位移。在这个例子中，因为发生了位移，撑开了.box，导致最终高度是36px。

解决方法很简单，直接

    .box {   line-height: 32px;   font-size: 24px; }
    .box > span { }

或者改变垂直对齐方式，如顶部对齐，这样就不会有参差位移了： 

    .box { line-height: 32px; } 
    .box > span {    font-size: 24px;   vertical-align: top; }
    
#### 深入理解 vertical-align 线性类属性值 

* inline-block 与 baseline 

> vertical-align属性的默认值baseline在文本之类的内联元素那里就是字符 x 的下 边缘，对于替换元素则是替换元素的下边缘。但是，如果是 inline-block 元素，则规则要 复杂了：一个inline-block元素，如果里面没有内联元素，或者overflow不是visible， 则该元素的基线就是其margin底边缘；否则其基线就是元素里面最后一行内联元素的基线。 

举个例子，现在有两个同尺寸的inline-block元素，唯一的区别就是一个是空的，一个是里面有字符，代码如下：

    .dib-baseline {   
        display: inline-block;
        width: 150px;
        height: 150px;
        border: 1px solid #cad5eb;
        background-color: #f0f3f9;
    } 
    <span class="dib-baseline"></span> 
    <span class="dib-baseline">x-baseline</span> 
    
这时候会出现这两个元素不在同一水平上，感兴趣可以自己去测一测（之前时不时遇到这个问题，现在豁然开朗）。原因是第一个元素里面没有内联元素，因此基线就是容器的下边缘，也就是下边框；而第二个有字符，因此第二个框框的基线就是字符的基线。于是就会出现这样的情况。

解决方法就是要不把vertical-align改成除bottom之外的top、middle值。要不在第一个框框内也是加个字符如&nbsp;。都是改变对齐方式。

* 了解 vertial-align:top/bottom 

顾名思义，vertial-align:top就是垂直上边缘对齐，具体定义如下。 

> 内联元素：元素底部和当前行框盒子的顶部对齐。

> table-cell元素：元素底padding边缘和表格行的顶部对齐

vertial-align:bottom 声明与此类似，只是把“顶部”换成“底部”，把“上边缘” 换成“下边缘”。

* vertial-align:middle 与近似垂直居中 

line-height 和 vertial-align: middle 实现的多行文本或者图片 的垂直居中全部都是“近似垂直居中”，原因与vertial- align:middle的定义有关。

> 内联元素:元素的垂直中心点和行框盒子基线往上 1/2 x-height 处对齐。
> table-cell 元素:单元格填充盒子相对于外面的表格行居中对齐。

可以看到，vertical-align：middle 是让内联元素的真正意义上的垂直中心位置和字符x的交叉点对齐。

然而，x字符的
