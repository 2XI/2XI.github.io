---
title: CSS reset
date: 2018-02-01 00:01:00
tags:
---

CSS reset
=========

1.  [什么是CSS－reset](#什么是CSS－reset)
2.  [为什么要进行CSS-reset](#为什么要进行CSS-reset)
3.  [html标签默认样式](#html标签默认样式)
4.  [浏览器默认样式](#浏览器默认样式)
5.  [一份CSS－reset代码](#一份CSS－reset代码)
6.  [到底要不要用CSS－reset](#到底要不要用CSS－reset)
7.  [总结](#总结)

### [](#什么是CSS－reset "什么是CSS－reset")什么是CSS－reset

CSS-reset是指在开发一开始就将浏览器的默认样式全部去掉，更准确说就是通过重新定义标签样式。“覆盖”浏览器的CSS默认属性。最最简单的说法就是把浏览器提供的默认样式覆盖掉！这就是CSS reset。

### [](#为什么要进行CSS-reset "为什么要进行CSS-reset")为什么要进行CSS-reset

在HTML标签在浏览器里有默认的样式，例如 p 标签有上下边距，strong标签有字体加粗样式，em标签有字体倾斜样式。不同浏览器的默认样式之间也会有差别，例如ul默认带有缩进的样式，在IE下，它的缩进是通过margin实现的，而Firefox下，它的缩进是由padding实现的。在切换页面的时候，浏览器的默认样式往往会给我们带来麻烦，影响开发效率。同时，因为浏览器的品种很多，每个浏览器的默认样式也是不同的，比如标签，在IE浏览器、Firefox浏览器以及Safari浏览器中的样式都是不同的，所以，通过重置button标签的CSS属性，然后再将它统一定义，就可以产生相同的显示效果。

### [](#html标签默认样式 "html标签默认样式")html标签默认样式

1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

17

18

19

20

21

22

23

24

25

26

27

28

29

30

31

32

33

34

35

36

37

38

39

40

41

42

43

44

45

46

47

48

49

50

51

52

53

54

55

li { display: list-item }/*默认以列表显示*/

head { display: none }/*默认不显示*/

table { display: table }/*默认为表格显示*/

tr { display: table-row }/*默认为表格行显示*/

thead { display: table-header-group }/*默认为表格头部分组显示*/

tbody { display: table-row-group }/*默认为表格行分组显示*/

tfoot { display: table-footer-group }/*默认为表格底部分组显示*/

col { display: table-column }/*默认为表格列显示*/

colgroup { display: table-column-group }/*默认为表格列分组显示*/

td, th { display: table-cell; }/*默认为单元格显示*/

caption { display: table-caption }/*默认为表格标题显示*/

th { font-weight: bolder; text-align: center }/*默认为表格标题显示，呈现加粗居中状态*/

caption { text-align: center }/*默认为表格标题显示，呈现居中状态*/

body { margin: 8px; line-height: 1.12 }

h1 { font-size: 2em; margin: .67em 0 }

h2 { font-size: 1.5em; margin: .75em 0 }

h3 { font-size: 1.17em; margin: .83em 0 }

h4, p, blockquote, ul, fieldset, form, ol, dl, dir, menu { margin: 1.12em 0 }

h5 { font-size: .83em; margin: 1.5em 0 }

h6 { font-size: .75em; margin: 1.67em 0 }

h1, h2, h3, h4, h5, h6, b,strong { font-weight: bolder }

blockquote { margin-left: 40px; margin-right: 40px }

i, cite, em,var, address { font-style: italic }

pre, tt, code, kbd, samp { font-family: monospace }

pre { white-space: pre }

button, textarea, input, object, select { display:inline-block; }

big { font-size: 1.17em }

small, sub, sup { font-size: .83em }

sub { vertical-align: sub }/*定义sub元素默认为下标显示*/

sup { vertical-align: super }/*定义sub元素默认为上标显示*/

table { border-spacing: 2px; }

thead, tbody, tfoot { vertical-align: middle }/*定义表头、主体表、表脚元素默认为垂直对齐*/

td, th { vertical-align: inherit }/*定义单元格、列标题默认为垂直对齐默认为继承*/

s, strike, del { text-decoration: line-through }/*定义这些元素默认为删除线显示*/

hr { border: 1px inset }/*定义分割线默认为1px宽的3D凹边效果*/

ol, ul, dir, menu, dd { margin-left: 40px }

ol { list-style-type: decimal }

ol ul, ul ol, ul ul, ol ol { margin-top: 0; margin-bottom: 0 }

u, ins { text-decoration: underline }

br:before { content: ""A" }/*定义换行元素的伪对象内容样式*/

:before, :after { white-space: pre-line }/*定义伪对象空格字符的默认样式*/

center { text-align: center }

abbr, acronym { font-variant: small-caps; letter-spacing: 0.1em }

:link, :visited { text-decoration: underline }

:focus { outline: thin dotted invert }

/\* Begin bidirectionality settings (do not change) */

BDO\[DIR="ltr"\] { direction: ltr; unicode-bidi: bidi-override }/*定义BDO元素当其属性为DIR="ltr"时的默认文本读写显示顺序*/

BDO\[DIR="rtl"\] { direction: rtl; unicode-bidi: bidi-override }/*定义BDO元素当其属性为DIR="rtl"时的默认文本读写显示顺序*/

*\[DIR="ltr"\] { direction: ltr; unicode-bidi: embed }/*定义任何元素当其属性为DIR="ltr"时的默认文本读写显示顺序*/

*\[DIR="rtl"\] { direction: rtl; unicode-bidi: embed }/*定义任何元素当其属性为DIR="rtl"时的默认文本读写显示顺序*/

@media print { /*定义标题和列表默认的打印样式*/

h1 { page-break-before: always }

h1, h2, h3, h4, h5, h6 { page-break-after: avoid }

ul, ol, dl { page-break-before: avoid }

}

### [](#浏览器默认样式 "浏览器默认样式")浏览器默认样式

1.页边距  
IE默认为10px，通过body的margin属性设置  
FF默认为8px，通过body的padding属性设置  
要清除页边距一定要清除这两个属性值

代码如下:

1

2

3

4

body {

    margin:0;

    padding:0;

}

2.段间距  
IE默认为19px，通过p的margin-top属性设置  
FF默认为1.12em，通过p的margin-bottom属性设  
p默认为块状显示，要清除段间距，一般可以设置

代码如下:

1

2

3

4

p {

    margin-top:0;

    margin-bottom:0;

}

3.标题样式  
h1~h6默认加粗显示：font-weight:bold;。  
默认大小请参上表  
还有是这样的写的

代码如下:

1

2

3

4

5

6

h1 {font-size:xx-large;}

h2 {font-size:x-large;}

h3 {font-size:large;}

h4 {font-size:medium;}

h5 {font-size:small;}

h6 {font-size:x-small;}

各大浏览器默认字体大小为16px，即等于medium，h1~h6元素默认以块状显示字体显示为粗体，  
要清除标题样式，一般可以设置

代码如下:

1

2

3

4

hx {

    font-weight:normal;

    font-size:value;

}

4.列表样式  
IE默认为40px，通过ul、ol的margin属性设置  
FF默认为40px，通过ul、ol的padding属性设置  
dl无缩进，但起内部的说明元素dd默认缩进40px，而名称元素dt没有缩进。  
要清除列表样式，一般可以设置

代码如下:

1

2

3

4

5

ul, ol, dd {

    list-style-type:none;/*清楚列表样式符*/

    margin-left:0;/*清楚IE左缩进*/

    padding-left:0;/*清楚非IE左缩进*/

}

5.元素居中  
IE默认为`text-align:center;`  
FF默认为`margin-left:auto;margin-right:auto;`

6.超链接样式  
a 样式默认带有下划线，显示颜色为蓝色，被访问过的超链接变紫色，要清除链接样式，一般可以设置

代码如下:

1

2

3

4

a {

    text-decoration:none;

    color:#colorname;

}

7 鼠标样式  
IE默认为`cursor:hand;`.  
FF默认为`cursor:pointer;`.该声明在IE中也有效

8 图片链接样式  
IE默认为紫色2px的边框线  
FF默认为蓝色2px的边框线  
要清除图片链接样式，一般可以设置

1

2

3

img {

    border:0;

}

### [](#一份CSS－reset代码 "一份CSS－reset代码")一份CSS－reset代码

1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

17

18

19

20

21

22

23

24

25

26

27

28

29

30

31

32

33

34

35

36

37

38

39

40

41

42

43

44

45

46

47

48

49

50

51

52

53

54

55

56

57

58

59

60

61

62

63

64

65

66

67

68

69

70

71

72

73

74

75

76

77

78

79

80

81

82

83

84

85

86

87

88

/*

html5doctor.com Reset Stylesheet

v1.4.1

2010-03-01

Author: Richard Clark - http://richclarkdesign.com

*/

html, body, div, span, object, iframe,

h1, h2, h3, h4, h5, h6, p, blockquote, pre,

abbr, address, cite, code,

del, dfn, em, img, ins, kbd, q, samp,

small, strong, sub, sup, var,

b, i,

dl, dt, dd, ol, ul, li,

fieldset, form, label, legend,

table, caption, tbody, tfoot, thead, tr, th, td,

article, aside, canvas, details, figcaption, figure,

footer, header, hgroup, menu, nav, section, summary,

time, mark, audio, video {

    margin:0;

    padding:0;

    border:0;

    outline:0;

    font-size:100%;

    vertical-align:baseline;

    background:transparent;

}

body {

    line-height:1;

}

:focus {

    outline: 1;

}

article,aside,canvas,details,figcaption,figure,

footer,header,hgroup,menu,nav,section,summary {

    display:block;

}

nav ul {

    list-style:none;

}

blockquote, q {

    quotes:none;

}

blockquote:before, blockquote:after,

q:before, q:after {

    content:'';

    content:none;

}

a {

    margin:0;

    padding:0;

    border:0;

    font-size:100%;

    vertical-align:baseline;

    background:transparent;

}

ins {

    background-color:#ff9;

    color:#000;

    text-decoration:none;

}

mark {

    background-color:#ff9;

    color:#000;

    font-style:italic;

    font-weight:bold;

}

del {

    text-decoration: line-through;

}

abbr\[title\], dfn\[title\] {

    border-bottom:1px dotted #000;

    cursor:help;

}

table {

    border-collapse:collapse;

    border-spacing:0;

}

hr {

    display:block;

    height:1px;

    border:0;

    border-top:1px solid #cccccc;

    margin:1em 0;

    padding:0;

}

input, select {

    vertical-align:middle;

}

### [](#到底要不要用CSS－reset "到底要不要用CSS－reset")到底要不要用CSS－reset

那些所谓的需要重置的标签  
我现在问您一个问题，在您制作的或参与开发的页面中，h1~h6标签您使用了几个，我想不可能全部都使用吧，使用三种类型的标题标签就不多了。您有必要对h1~h6所有标签都使用margin的清除吗？ OK，我们现在换个角度思考，假如我们没有对h1~h6标签设置{margin:0;}的重置怎么办？从SEO的角度讲，一个页面最多只能出现一个h1标签，所以，显然，h1标签的CSS reset完全没有必要，页面什么地方用就设置相应的样式，只要你记住，h1标签是有个默认的margin-top与margin-bottom值的，所以，我们就可以由这样的属性：

1

h1{margin:10px 0 0;}

对比下CSS reset下的使用：

1

2

h1, h2, h3, h4, h5, h6{margin:0;}

h1{margin-top:10px;}

使用CSS reset不仅文件大小增加了，CSS代码属性也发生了重置，CSS渲染也增加了。显然不及没有CSS reset来的高效。我们将这些标签重置，在后面的css中很大程度上又要重新设置它，这反而增加了浏览器的渲染，与其这样，还不如在使用它的地方直接将它设定到合适的值，这样不仅达到了效果，反而减少了渲染。回过来，就算有一些差异，为何非得在头部CSS reset的位置统一呢？当需要的时候，再设置，反而更合理，更高效！

### [](#总结 "总结")总结

对于CSS－reset,我也觉得没有太大的使用必要，在开发过程中，开发者只要熟悉各个标签的默认样式，对于需要重新设置的属性在合适的地方显式的设置。在学习过程中渐渐熟悉在不同浏览器中表现有所不同的标签，并且合理的设置属性值就可以了。
