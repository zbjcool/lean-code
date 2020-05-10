css弹性盒子布局：Flexbox可以控制在容器内的子元素的对齐方式、排列方式以及排序顺序，即使其子元素的尺寸是未知或者动态的情况下
 Flex布局的主要思想是让容器能使其子元素的宽高（或其他属性）能够以最好的方式去填充可用空间（主要是去适应不同的设备跟分辨率）。一个Flex弹性盒子能让子元素填充可用空间或者为了阻止子元素超出区域而进行收缩。
     Flexbox布局常用于小的应用程序组件之中，而CSS Grid布局模块将应用于大规模的布局之中。

display:flex;/*or linline-flex*/
Webkit内核的浏览器，必须加上-webkit前缀。
注意，设为Flex布局以后，子元素的float、clear和vertical-align属性将失效。

6个属性
* flex-direction
* flex-wrap
* flex-flow
* justify-content
* align-items
* align-content



1.·flex-direction: row | row-reverse | column | column-reverse

  flex-wrap: nowrap | wrap | wrap-reverse;
nowrap:不换行（默认）
wrap:换行
wrap-reverse:换行，第一行在下方

 2. flex-flow: <flex-direction> || <flex-wrap>;
3.flex-flow:是flex-direction和flex-wrap的简写

4.justify-content属性定义了项目在主轴上的对齐方式。
  justify-content: flex-start | flex-end | center | space-between | space-around;
 flex-start：左对齐
flex-end ：右对齐
center：居中
between: 两端到底
 
space-around：每个项目两侧的间隔相等

5.align-items属性定义项目在交叉轴上如何对齐。
  align-items: flex-start | flex-end | center | baseline | stretch;
* flex-start：交叉轴的起点对齐。
* flex-end：交叉轴的终点对齐。
* center：交叉轴的中点对齐。
* baseline: 项目的第一行文字的基线对齐。
* stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。

6.align-content属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。
* flex-start：与交叉轴的起点对齐。
* flex-end：与交叉轴的终点对齐。
* center：与交叉轴的中点对齐。
* space-between：与交叉轴两端对齐，轴线之间的间隔平均分布。
* space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
* stretch（默认值）：轴线占满整个交叉轴。

item 属性
* order
* flex-grow
* flex-shrink
* flex-basis
* flex
* align-self

order属性定义项目的排列顺序。数值越小，排列越靠前，默认为0。可以是负数

flex-grow属性定义暂用剩余空间的比例，默认为0，即如果存在剩余空间，也不放大。item 设置了flex-grow会影响container的justify-content 

flex-shrink属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。1为缩小比例一倍，0不缩小
flex-basis属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。
flex属性是flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。后两个属性可选。
align-self属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。

  align-self: auto | flex-start | flex-end | center | baseline | stretch;
flex:1;
是flex:1 1 auto的缩写




盒子模型：
弹性盒子：flex:
flex-direction 方向
flex-wrap: 能不能换行
justify-content: 主轴，flex-start，flex-end,center,between
space-around：每个项目两侧的间隔相等
align-items: 交叉轴, flex-start | flex-end | center | baseline | stretch;
item属性
order,排序，数值越小，排列越靠前
flex-grow：默认0，不放大 有剩余空间的时候使用
flex-shrink: 默认1，缩小一倍，0不缩小 总的使用空间超过项目空间
flex-base: 默认auto，项目的本来大小
align-self：覆盖align-items
flex，是上面三个的合集，后面两个可选，
如果是flex:1 代表flex：1，1，auto
如果都是flex：1 剩余空间平分
都是flex:1 不放大
一个是flex:1,一个是flex:2剩余空间分成3份

