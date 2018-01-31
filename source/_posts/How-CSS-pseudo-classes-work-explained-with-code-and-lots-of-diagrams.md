---
title: 'How CSS pseudo-classes work, explained with code and lots of diagrams'
date: 2018-02-01 00:14:08
tags:
---

How CSS pseudo-classes work, explained with code and lots of diagrams
=====================================================================

1.  [HTML和DOM树](#HTML和DOM树)

原文：[How CSS pseudo-classes work, explained with code and lots of diagrams](https://medium.freecodecamp.com/explained-css-pseudo-classes-cef3c3177361#.s76ufr89z)

老实说，有时候CSS真的很让人伤脑筋，比如在一个父元素中居中一个元素就很困难。

今天我们来了解一个更具有挑战性的CSS特性：伪类。

我这里要讨论的伪类涉及以下两种类型：

*-of-type selectors

*-child selectors

你或许在想，“我要学习伪类，为什么你却在谈论选择器？”。好吧，它们基本上是同一个概念，我也将会互换着使用这它们。

伪类有时候很难理解，主要是因为它们是以一种抽象的方式来描述的。我将使用·一种不同的表述方式，并且通过构造一棵DOM树来帮助你理解。

### [](#HTML和DOM树 "HTML和DOM树")HTML和DOM树

我们首先来看一下HTML代码块，我将在所有的例子中使用这些代码。

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

<body>

  <div class=”main”>

     <a href=”#”>Inner Link 1</a>

     <a href=”#”>Inner Link 2</a>

     <ul>

       <a href=”#”>Inner Inner Link 1</a>

       <li>

         <a href=”#”>List Item 1</a>

       </li>

       <li>

         <a href=”#”>List Item 2</a>

       </li>

     </ul>

     <a href=”#”>Inner Link 3</a>

  </div>

  <a href=”#”>Outer Link 1</a>

  <a href=”#”>Outer Link 2</a>

</body>

现在我将把这些代码转换成更直观更符合视觉的东西——一棵DOM树。

下面的body元素有三个子元素，.main和两个锚点元素。

1

2

3

4

5

6

7

<body>

  <div class=”main”>

   ...

  </div>

  <a href=”#”>Outer Link 1</a>

  <a href=”#”>Outer Link 2</a>

</body>

下面是以一棵DOM树的形式展示时body和它的三个子元素的关系示意图：

![fig-1](https://cdn-images-1.medium.com/max/800/1*0J4m0pNfNUUe-JE9dPIbHw.png)

你需要记住的一点是子元素在DOM树中被放置的位置很重要，在代码中从上至下排列的子元素在DOM树中被从左至右放置。我们来看类名为.main的div元素：

1

2

3

4

5

6

7

8

<div class=”main”>

   <a href=”#”>Inner Link 1</a>

   <a href=”#”>Inner Link 2</a>

   <ul>

     ...

   </ul>

   <a href=”#”>Inner Link 3</a>

</div>

.main有4个子元素，前两个是锚点元素，接下来是一个ul元素，然后又是一个锚点元素。

![fig-2](https://cdn-images-1.medium.com/max/800/1*b1bt8tsEPJ7L1jJNkSB1WQ.png)

同样，我们逐级画出每一层的DOM树。

![fig-3](https://cdn-images-1.medium.com/max/800/1*xn3NJH7ajQ0t-nSQWkr2HA.png)

为了使这篇文章对你有所帮助，理解这棵DOM树很重要。

下面我们来看第一个伪类。

1

Pseudo-class #1 :only-of-type

所有的伪类都遵循同样的范式：

1

what-you-want-to-select:filter { /* styles */ }

what-you-want-to-select可以被用来选择任何以集合形式存在于DOM中的元素，下面让我来展示一个例子：

1

2

3

a:only-of-type { 

  border: 2px solid black;

}

在上面的代码块中，what-you-want-to-select是锚点元素（a标签），过滤器是only-of-type我们稍后会看到这个选择器做了什么。

首先，不用感谢我，如果你闲麻烦不想建测试项目的话，我在codepen中为你创建了一个。

你可以打开链接，查看这些变化，当你感到迷惑的时候再回头看这篇文章的解释，做好你的部分，我来完成我该做的。

下面我将完成的的那一部分，解释上面展示的代码。我们将从选中代码中所有的元素开始，然后把这些元素一个个过滤掉。

![fig-4](https://cdn-images-1.medium.com/max/800/1*uBjIeeXnjBgkB2GApFiiGQ.png)

注意到选择是如何进行的了吗？DOM树中的每一部分（标记为1-5）都有共同的父元素. 第一部分的父元素是body，第二部分是.main,以此类推。注意每一部分的子元素在嵌套中都对应着相同的层级。

接下来，因为锚点元素是what-you-want-to-select，我们将会这样做：

![fig-5](https://cdn-images-1.medium.com/max/1600/1*bxFbXy1QDeGf-84KSJNxDg.png)

我已经在每部分中选出所有的锚点元素并且从左到右依次编号，我已经说过，从左到右很重要。

到此what-you-want-to-select这部分结束，下面进入过滤部分。

![fig-6](https://cdn-images-1.medium.com/max/1600/1*WwCVWx4UKJ5bdPUV1e4vXQ.png)

only-of类依次查询每一部分，挑选出有且仅有一个锚点元素的部分中的锚点元素。注意到3, 4, 5部分中分别只有一个锚点元素了吗？上图所示就是这个选择器作用的结果。

1

Pseudo-class #2 :first-of-type

让我们回到我们选出所有“what-you-want-to-select”的地方（在我们的例子中是锚点元素）。

![fig-7](https://cdn-images-1.medium.com/max/1600/1*bxFbXy1QDeGf-84KSJNxDg.png)

过滤器first-of-type转化为在每一部分中挑选第一个出现的锚点元素。

![fig-8](https://cdn-images-1.medium.com/max/1600/1*PJExtAelKm7-Xdt31Dw6cA.png)

下面是完成部分工作的代码：

1

2

3

a:first-of-type { 

  border: 2px solid black;

}

以防你忘了我不辞辛劳为你在CodePen中创建的项目，这里再次给出查看代码效果的链接。

1

Pseudo-class #3 :last-of-type

如果你不能通过名称来区分，last-of-type刚好与first-of-type作用效果相反。这也就意味着它会选出DOM树每一部分中的最后一个锚点元素。

![fig-9](https://cdn-images-1.medium.com/max/1600/1*dWlzrEMXkZueTDY52sGLzg.png)

“What about the sections with just one anchor element?”, not very glad you asked that question. It’s quite simple to see if a section has just one anchor element, it obviously passes the only-of-type filter, but not only that. Since there are no anchor elements preceding or following that particular tag it passes both first-of-type and last-of-type filters as well (e.g a tags Section 4 and 5).

1

Pseudo-class #4 :nth-of-type(number/an + b/even/odd)

And now we finally bite into the juicy part of the article, there’s simple CSS with some fifth grade Math toppings, hope you enjoy savouring it.  
Let’s declare the following style to begin with.

1

2

3

a:nth-of-type(1) {

border: 2px solid black;

}

It looks a little cryptic but is quite simple really. To read the selector simply take the number from the parentheses and replace nth in the selector name with that number’s ordinal form. That’s another fancy English word for you, to be honest though…

Alright coming back, a:nth-of-type(1) can be therefore read as a:first-of-type and no surprise it works exactly like a:first-of-type and results in the elements getting selected as shown below; just the anchor elements which are first of their types in their respective section.

![fig-10](https://cdn-images-1.medium.com/max/1600/1*PJExtAelKm7-Xdt31Dw6cA.png)

Well that is fine and dandy, but let’s try something different here.

1

2

3

a:nth-of-type(0) {

border: 2px solid black;

}

If you guessed it right, which I am sure you didn’t, no anchor elements get selected in this case. As the numbering of types (and children as we’ll see) in each section starts from 1 and not 0, there is no “0” anchor elements in any of the sections and therefore a:zeroth-of-type selects nothing. And so will be the case for a:nth-of-type(5) or a:nth-of-type(6/7/8) because there are no a:fifth-of-type or a:sixth/seventh/eighth-of-type in any of the sections.  
But if we went ahead and used…

1

2

3

a:nth-of-type(2) { 

  border: 2px solid black;

}

quite clearly sections 1 and 2 have a second-of-type anchor elements and hence those are the ones that get selected.

![fig-11](https://cdn-images-1.medium.com/max/1600/1*o7aTc-EJF53N7bAHs2Ssxg.png)

Similarly, just to reinforce the point here, if we went ahead and declared the following style,

1

2

3

a:nth-of-type(3) { 

  border: 2px solid black;

}

it will select the third anchor elements in the second section as section 2is the only section with a :third-of-type anchor element.

![fig-12](https://cdn-images-1.medium.com/max/1600/1*Ob0gZ_tJhflq73RCzbyFkw.png)

Quite simple isn’t it? But numbers aren’t the only thing that you can pass into :nth-of-type(…), this bloke is more powerful that that, you can also pass in formulas of form (a*n) + b (or for brevity an + b). Where a and b are constants and n is a value >= 0. How did you like the Math topping sir? don’t worry it’ll all make sense in a second.

Consider the following style

1

a:nth-of-type(n) {  border: 2px solid black; }

The formula that’s passed in the selector above is (1 * n) + 0 \[= n\] , a is 1, b is 0 and n is well, n. What happens next is, starting from 0 the numerical value of n is incrementally plugged into the formula and selection is made. Therefore a:nth-of-type(n) basically translates to

1

2

3

4

5

a:nth-of-type(0) {  border: 2px solid black; } // n = 0

a:nth-of-type(1) {  border: 2px solid black; } // n = 1

a:nth-of-type(2) {  border: 2px solid black; } // n = 2

a:nth-of-type(3) {  border: 2px solid black; } // n = 3

a:nth-of-type(4) {  border: 2px solid black; } // n = 4

Hence this results in all the anchor elements getting selected.

Let’s consider one more example

1

a:nth-of-type(2n + 1) {  border: 2px solid black; }

Starting from 0 and incrementally plugging values of n in the formula generates the following selectors.

1

2

3

4

5

6

// n = 0 implies (2 * 0) + 1 = 1

a:nth-of-type(1) { border: 2px solid black; }

// n = 1 implies (2 * 1) + 1 = 3

a:nth-of-type(3) { border: 2px solid black; }

// n = 2 implies (2 * 2) + 1 = 5 - No selections since no fifth-of-type present in any of the sections

a:nth-of-type(5) { border: 2px solid black; }...

![fig-13](https://cdn-images-1.medium.com/max/1600/1*QrQj3hZlegF3D-kv7yB0gA.png)

Other than numbers and formulas that generate numbers, you can pass in either even or odd strings. even selects all the even occurrences of an element of particular type in a section i.e :second-of-type :fourth-of-type :sixth-of-type e.t.c and on the other hand obviously :nth-of-type(odd) selects all the odd occurrences i.e :first-of-type, :third-of-type, :fifth-of-type e.t.c

Pseudo-class #5 :nth-last-of-type(number/an + b/even/odd)

This selector functions exactly like the previous one, but with one little difference. See for yourself…

1

a:nth-last-of-type(1) {  border: 2px solid black; }

![fig-14](https://cdn-images-1.medium.com/max/1600/1*8iEbmV82IuBJ7jyYiyx_AA.png)

Notice how in each level numbering of anchor types is done from right to left instead of normal left to right. That is the only difference. last-of-type accepts numbers and formulas and even/odd just like nth-of-type except when selection is made the last type is treated as first, second last as second, third last as third and so on…

With that we come to an end of *-of-type selectors. Hope it was a fun ride for you, we started with only-of-type then moved to first-of-type, last-of-type and took a huge dip into nth-of-type(…) and nth-last-of-type(..). If in case somewhere in the middle you lost your grip and fell off I encourage you play around with this pen and re read the explanation.

Alright, time to hop on to the next one in this less visited corner of the CSS theme park. Another category of pseudo selectors/classes we’re going to delve into are -child classes. With a clear understanding of how -of-type selectors work grasping the concept behind -child selectors should be a cinch for you. “Cinch? What’s that? Is it a unit of measurement?” No ya dumbass, it means an extremely easy task. Anyways, let’s start with our very first -child selector, :only-child.

Child pseudo-class #1 :only-child

Consider the following style.

1

2

3

a:only-child { 

  border: 2px solid black;

}

Now that’s the very definition of self explanatory and straightforward. The selector says to select all the anchor elements, given that the anchor element should be the only child of its parent, or, to put in other words select all the anchor elements whose parent has just one child and that one child is an anchor element.

![fig-15](https://cdn-images-1.medium.com/max/1600/1*bNZeHcGUOqswsJDMQFszzw.png)

I had a friend who was never his mother’s favorite, and he was an only child. Just wanted to plug that in there, anyways, notice that in contrast with *-of-type selectors we are no longer numbering types, but children, starting of course from 1 (and not 0). When compared with only-of-type, the anchor element in section 3 is not selected as its parent (ul) has 3 children therefore even though it (the anchor element in level 3) is an only child of type ‘a’ of its parent, its not the only child, there are 2 lis as well.

Child pseudo-class #2 :first-child  
Consider the following style declaration.

1

2

3

a:first-child { 

  border: 2px solid black;

}

It simply says, select all the anchor element, but with one condition in mind, the anchor element should be the first child of its parent. That’s it, no further explanation needed.

![fig-16](https://cdn-images-1.medium.com/max/1600/1*qLx7ELzLcCUWHY9xakrsfg.png)

For if you are a little confused as of why the a in section 1 wasn’t selected it’s because the first child in section 1 (whose parent is body) is .main, the first a in section 1 is the second child and couldn’t pass the first-child filter, that is the exact reason why the poor bloke ended up not being selected and was given a big hashtag fuck off. Let’s continue to the next one.

1

Child pseudo-class #3 :last-child

This is the part where selectors should start to get self explanatory and you should start thinking I am dumb trying to explain them to you. But my name is not blurryface and I don’t care what you think. “Nice twenty one pilots reference there” yeah I know, thanks. Now, look at the following style declaration.

1

2

3

a:last-child { 

  border: 2px solid black;

}

what-you-want-to-select ? “Anchor elements.” And the filter you want to use? last-child. That quite simply translates to select those anchor elements which are the last child of their parent. Or, in other words select anchor elements whose parent finally decided it wasn’t fun anymore and stopped after that bloke was born. Below is what the tree looks like with :last-child selections.

![fig-17](https://cdn-images-1.medium.com/max/1600/1*PfU4UZ2kZvgWZlG-Pav05w.png)

Child pseudo-class #4 :nth-child(number/an+b/even/odd)  
I hope you were able to digest the Math topping you got served last time, because it’s about to happen again only this time on a slightly different crust.  
Now, I would like you to take all your attention and laser point it to the following example.

1

a:nth-child(1) {  border: 2px solid black; }

It’s all the same as :nth-of-type, I would have linked to that section of the article here but Medium policies don’t allow that, if you want a refresher, you will have to scroll up to that section. Leaving Medium policies aside which might change in future, what hasn’t changed is the process of decrypting nth-selectors .  
Just like with :nth-of-type, in the selector name take the number in parentheses and replace “nth” with that number’s ordinal form. Therefore the selector shown in example is equivalent to a:first-child and works exactly the same; i.e selects all the anchor elements, given that they are the first child of their parent.  
That should nail the similarity between the two nth-selectors (nth-of-type and nth-child), but we will anyways go ahead and take a look at another example.

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

a:nth-child(2n - 1) {  border: 2px solid black; }

``` 

We begin by incrementally plugging in values of n starting from 0 into the formula, which makes us realize that the selector shown above is basically equivalent to the ones shown below.

// n = 0 implies (2 0) - 1 = 0 - 1 = -1

a:nth-child(-1) { border: 2px solid black; } | No selections

// n = 1 implies (2 1) - 1 = 2 - 1 = 1

a:nth-child(1) { border: 2px solid black; }

// n = 2 implies (2 2) - 1 = 4 - 1 = 3

a:nth-child(3) { border: 2px solid black; }

// n = 3 implies (2 3) - 1 = 6 - 1 = 5

a:nth-child(5) { border: 2px solid black; } | No selections further

…

As it is, if the selector gets numbers out of bounds (like -1, 5, 6… in the case above) fed into it, it just ignores them. Following is how the tree looks with a:nth-child(2n-1) applied.

![fig-18](https://cdn-images-1.medium.com/max/1600/1*aXGTeApzv5e1c7CJdk-pvg.png)

Folks at CSS Tricks have a very informative article called Useful :nth-child Recipes you should check it out and put your knowledge of :nth-child to test. I challenge you m8.  
With that we will move to the last selector of this article which punningly is :nth-last-child. Holy shit! why is “punningly” a word even?  
Child pseudo-class #5 :nth-last-child(number/an + b/even/odd)  
This selector works exactly like :nth-child except that it starts selecting elements from the opposite direction just like that annoying high school teacher who would ask questions to the class starting from the peaceful folks seated at the last benches. God I hated him. If you look at the trees drawn so far, the children are numbered left to right in each section, but this selector bloke sees the tree like so  
![fig-19](https://cdn-images-1.medium.com/max/1600/1*2ChjMydCcmDb9TgFrY4htg.png)  
The children in each section are numbered right to left. So if we go ahead and apply the following style

a:nth-last-child(1) { border: 2px solid black; }  
```

the anchor elements will get selected as shown below.

![fig-20](https://cdn-images-1.medium.com/max/1600/1*XBmBun1e7jY0aaHsBlZHlg.png)

Quite straightforward isn’t it? This selector also very comfortably accepts formulas (of form an + b) and even/odd strings, the selections though, are made from the opposite end.  
OK, this is where our journey together ends. You can pay for your ticket by tweeting this article to your developer buddies.  
I hope you enjoyed reading this and learned something new, including some shiny new English words.  
This is Nash signing off. I’ll see you in the next article. Follow me on Twitter to keep in touch. I tweet about dev-related stuff. A lot.
