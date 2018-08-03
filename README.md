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
未完待续
