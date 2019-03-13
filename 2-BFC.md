# BFC

## 什么是BFC

```
BFC全称是Block Formatting Context，即块格式化上下文。他是CSS2.1规范定义的，关于CSS渲染定位的一个概念。要明白BFC到底是什么，首先来看看什么是视觉格式化模型。
```

## 二、视觉格式化模型

视觉格式化模型(visual formatting model)是用来处理文档并将它显示在视觉媒体上的机制，他也是CSS中的一个概念。

视觉格式化模型定义了盒(Box)的生成，盒主要包含了块盒、行内盒、匿名盒(没有名字不能被选择器选中的盒)以及一些实验性的盒(未来可能添加到规范中)。盒的类型由display属性决定。

### 2.1块盒(block box)

块盒有一下特性：
- 当元素的css属性display为block，list-item或table时，他是块级元素block-level;
- 视觉上呈现为块，竖直排列;
- 块级盒参与(块格式化上下文);
- 每个块级元素至少生成一个块级盒，称为主要会块级盒(principal block-level box)。一些元素，比如<li>，生成额外的盒来放置项目符号，不过多数元素只生成一个主要块级盒

### 2.2行内盒(inline box)

- 当元素的css属性display的计算值为inline，inline-block或inline-table时，称它为行内级元素
-视觉上它将内容与其他行内级元素排列为多行；典型的如段落内容，有文本（可以有多重格式譬如着重），或图片，都是行内级元素；
-行内级元素生成行内级盒(inline-level boxes)，参与行内格式化上下文(inline formatting context)。同时参与生成行内格式化上下文的行内级盒称为行内盒(inline-boxes)。所有display:inline的非替换元素生成的盒是行内盒；
-不参与生成行内格式化上下文的行内级盒称为原子行内级盒(atomic inline-level boxes)。这些盒由可替换行内元素，或display值为inline-block或inline-table的元素生成，不能拆分成多个盒；

### 2.3 匿名盒(anonymous box)

匿名盒也有分匿名块盒与匿名行内盒，因为匿名盒没有名字，不能利用选择器来选择他们，所以他们的所有属性都为inherit或初始默认值；

如下面例子，会创建匿名块盒来包含毗邻的行内级盒：

```html
<div>
    Some inline text
    <p>followed by a paragraph</p>
    followed by more inline text
</div>
```

![BFC](img/bfc-1.webp "匿名盒")

## 三、三个定位方案

在定位的时候，浏览器就会根据元素的盒类型和上下文对这些元素进行定位，可以说盒就是定位的基本单位。定位时，有三种定位方案，分别是常规流，浮动以及绝对定位。

### 3.1常规流(Normal flow)

- 在常规流中，盒一个接着一个排列；
- 在块级格式化上下文里面，他们竖直排列；
- 在行内格式化上下文里面，他们横着排列；
- 当position为static或relative，并且float为none时会触发常规流；
- 对于静态定位(static positioning)，position:static，盒的位置是常规布局里的位置；
- 对于相对定位(relative positioning)，position:relative，盒偏移位置由这些属性定义top，bottom，left and right。即使有偏移，任然保留原有的位置，其他常规流不能占有这个位置。

### 3.2浮动(Floats)

- 盒称为浮动盒(floating boxes)；
- 它位于当前行的开头或末尾；
- 这导致常规流环绕在他的周边，除非设置clear属性；

### 3.3绝对定位(Absolute positioning)

- 绝对定位方案，盒从常规流中被移除，不影响常规流的布局；
- 它的定位相对于它的包含块，相关CSS属性：top，bottom，left及right；
- 如果元素的属性position为absolute或fixed，它是绝对定位元素；
- 对于position：absolute，元素定位将相对于最近的一个relative、fexed或absolute的父元素，如果没有则相对于body；

## 四、块格式化上下文

到这里，已经对CSS的定位有一定的了解了，从上面的信息中也可以得知，块格式上下文是页面CSS视觉渲染的一部分，用于决定块盒子的布局及浮动相互影响范围的一个区域。

### 4.1 BFC的创建方法

- 根元素或其他包含他的元素；
- 浮动(元素的float不为none)；
- 绝对定位元素(元素的position为absolute或fixed)；
- 行内块inline-blocks(元素的display:inline-block)；
- 表格单元格(元素的display:table-cell，HTML表格单与默认属性)；
- overflow的值不菲visible的元素
- 弹性盒flex boxes(元素的display:flex或inline-flex)；

但其中，最常见的就是overflow:hidden、float:left/right、position:absolute。也就是说，每次看到这些属性的时候，就代表了该元素已经创建了一个BFC了

### 4.2 BFC的范围

BFC的范围在MDN中是这样描述的。

>A block formatting context contains everything inside of the element creating it that is not also inside a descendant element that creates a new block formatting context.
中文的意思一个BFC包含创建该上下文元素的所有子元素，但不包括创建了新BFC的子元素的内部元素。

这段看上去有点奇怪，我是这么理解的，假如有下面代码，class名为.BFC代表创建了新的快格式化：

```html
<div id="div_1" class="BFC">
    <div id="div_2">
        <div id="div_3"></div>
        <div id="div_4"></div>
    </div>
    <div id="div_5" class="BFC">
        <div id="div_6"></div>
        <div id="div_7"></div>
    </div>
</div>
```

这段代码表示，#div_1创建了一个块格式上下文，这个上下文包括了#div_2、#div_3、#div_4、#div_5。即#div_2中的子元素也属于#div_1所创建的BFC。但由于#div_5创建了新的BFC，所以#div_6和#div_7就被排除在外层的BFC之外。

我认为，这从另一个角度说明，一个元素不能同时存在于两个BFC中。

BFC的一个重要的效果是，让处于BFC内部的元素与外部的元素相互隔离，使内外元素的定位不相互影响。这是利用BFC清除浮动所利用的特性，关于清除浮动将在后面讲述。

如果一个元素能够同时处于两个BFC钟，那么就意味着这个元素能与两个BFC中的元素发生作用，就违反了BFC的隔离作用，所以这个假设就不成立了。

### 4.3 BFC的效果

就如刚才提到的，BFC的最显著的效果就是建立一个隔离的空间，断绝空间内外元素间相互的作用。然而，BFC还有更多的特性：

>Floats, absolutely positioned elements, block containers (such as inline-blocks, table-cells, and table-captions) that are not block boxes, and block boxes with 'overflow' other than 'visible' (except when that value has been propagated to the viewport) establish new block formatting contexts for their contents.
>In a block formatting context, boxes are laid out one after the other, vertically, beginning at the top of a containing block. The vertical distance between two sibling boxes is determined by the 'margin' properties. Vertical margins between adjacent block-level boxes in a block formatting context collapse.
>In a block formatting context, each box's left outer edge touches the left edge of the containing block (for right-to-left formatting, right edges touch). This is true even in the presence of floats (although a box's line boxes may shrink due to the floats), unless the box establishes a new block formatting context (in which case the box itself may become narrower due to the floats).

简单归纳一下：

- 内部的盒会在垂直方向一个接一个排列(可以看做BFC中有一个常规流)
- 处于同一个BFC中的元素相互影响，可能发生margin collapse；
- 每个元素的margin box的左边，与容器块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此
- BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素，反之亦然；
- 计算BFC的高度时，考虑BFC所包含的所有元素，连浮动元素也参与计算；
- 浮动盒区域不叠加到BFC上；

这么多性质有点难以理解，但可以作如下推理来帮助理解；html的根元素就是<html>，而根元素会创建一个BFC，创建一个新的BFC时就相当于在这个元素内部创建一个新<html>，子元素的定位就如同在一个新<html>页面中那样，而这个新旧html页面之间是不会相互影响的

上述这个理解并不是最准确的理解，升值是将因果倒置了(因为html是根元素，因此才会有BFC的特性，而不是BFC有html的特性)，但这样的推理可以帮助理解BFC这个概念。

## 五、从实际代码来分析BFC

讲了这么多，还是比较难理解，所以下面通过一些例子来加深对BFC的认识。

### 5.1 实例一

```html
<div class="box">
    <div class="left"></div>
    <div class="right"></div>
</div>

<style>
    *{
        margin:0;
        padding:0;
    }

    .left{
        background:#ff0;
        opacity:0.5;
        border:3px solid #f00;
        width:200px;
        height:200px;
        float:left;
    }

    .right{
        background:#00f;
        opacity:0.5;
        border:3px solid #000;
        width:400px;
        min-height:100px;
    }

    .box{
        background:#888;
        height:100%;
        margin-left:50px;
    }
</style>
```

![BFC](img/bfc-2.webp "实例一")

黄色框(#left)向左浮动，它创建了一个新的BFC，但暂时不讨论它所传就的BFC。由于黄色框浮动了，它脱离了原本normal flow的位置，因此，蓝色框(#right)就被定为到灰色父元素的左上角(特性3：元素左边与容器左边相接触)，与浮动黄色框发生了重叠。
同时，由于灰色框(.box)并没有创建BFC，因此在计算高度的时候，并没有考虑黄色框的区域(特性6：浮动区域不叠加到BFC区域上)，发生了高度坍塌，(.box的高度为#right的高度并不是#left的高度)这也是常见问题之一。

### 5.2 实例二

现在通过设置overflow:hidden来创建BFC，在看看效果如何。

```html
.BFC{
    overflow:hidden;
}

<div class="box BFC">
    <div class="left"></div>
    <div class="right"></div>
</div>
```

![BFC](img/bfc-2.webp "实例二")

灰色框创建了一个新的BFC后，高度发生了变化，计算高度时它将黄色框区域也考虑进去了(特性5：计算BFC的高度时，浮动元素也参与计算)

而黄色框盒蓝色框的显示效果任然没有任何变化

### 5.3 实例三

现在，现将一些小块添加到蓝色框中，看看效果

```html
<style>
    .little{
        background:#fff;
        width:50px;
        height:50px;
        margin:10px;
        float:left;
    }

    <div class="box BFC">
        <div class="left"></div>
        <div class="right">
            <div class="little"></div>
            <div class="little"></div>
            <div class="little"></div>
        </div>
    </div>
</style>
```

![BFC](img/bfc-4.webp "实例三")

由于蓝色框没有创建新的BFC，因此蓝色框中白色块收到黄色框的影响，被挤到了右边去了。现不要管这个，看看白色块的margin

### 5.4 实例四

利用同实例二中一样的方法，为蓝色框创建BFC；

```html
<style>
    <div class="box BFC">
        <div class="left"></div>
        <div class="right BFC">
            <div class="little"></div>
            <div class="little"></div>
            <div class="little"></div>
        </div>
    </div>
</style>
```

![BFC](img/bfc-5.webp "实例五")

一旦蓝色框创建了新的BFC以后，蓝色框就不与黄色浮动框发生重叠了，同时内部的白块处于隔离空间(特性4：BFC就是页面上的一个隔离的独立容器)，白色块也不会受到黄色浮动框的挤压。

## 六、总结

以上就是BFC的分析，BFC的概念比较抽象，但通过实例分析应该能够更好的理解BFC。在实际中，利用BFC可以闭合浮动（实例二），繁殖与浮动元素重叠（实例四）。同时，由于BFC的隔离作用，可以利用BFC包含一个元素，防止这个元素与BFC外的元素发生margin collapse