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
<div>里面包裹文字，<div>的高度不是font-size决定的，是由line-height决定
对于非替换的纯内联元素，line-height完全决定它的高度，padding、border之类的属性对
它的可视高度是完全没有影响的。也就是说“盒模型”是针对块级元素的
    
#### line-height对内联元素作用的细节
line-height是通过调节文字的行距来控制高度的
在css中，文字的行距是是分散在当前文字的上方和下方的
也就是说，第一行的文字也是有行距的，只不过这个行距的高度
只是完整行距的一半，也被称为半行距

#### 半行距的范围
在css中，行距 = line-height - em-box
有了行距，一分为二，分别在em-box的上面和下面加上，就构成了文字的完整高度
这里有个问题，em-box是什么东西？
它的高度正好是1em，1em相当于当前一个font-size的大小
也就是说行距 近似等于 line-height - font-size
当我们的字体的宋体的时候，内容区域（可以近似理解为ff/ie浏览器下文本选中带背景区域）
和em-box是等同的（书上说的，未求证）。
于是我们可以利用宋体，准确揪出半行距的范围
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

这里要注意一点，文字的上边剧，算出来需要向下取整
文字下边距，算出来需要向上取整

 
