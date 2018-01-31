---
title: Vuejs-event-system
date: 2018-02-01 00:08:39
tags:
---


Vuejs-event-system
==================

1.  [组件之间通信的原则](#组件之间通信的原则)
2.  [实现](#实现)
    1.  [自定义事件](#自定义事件)
    2.  [全局事件总线(global event bus)](#全局事件总线-global-event-bus)

在开发多组件应用是，经常涉及到不同组件之间的通信问题，组件的作用域是相互独立的，我们当然可以直接引用父子组件来同步组件之间的状态，然而这种实现是高耦合的，与我们组件化的思想刚好相悖。  
使用一种合理的组件之间的通信方式有利于提高组件的健壮性与可维护性。

### [](#组件之间通信的原则 "组件之间通信的原则")组件之间通信的原则

使用props从父组件向子组件传递数据

使用event从子组件通知父组件改变数据

### [](#实现 "实现")实现

#### [](#自定义事件 "自定义事件")自定义事件

使用自定义事件，我们可以在子组件触发事件，在父组件监听事件。

于是父子组件的通信可以实现为：

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

<!\-\- 父组件 -->

<div id="example">

	<p>{{ total }}</p>

	<App v-on:incre="incrementTotal"></App>

</div>

new Vue({

	el: '#example',

		data: {

			total: 0

		},

	components:{

		App: App

	},

	methods: {

		incrementTotal() {

			this.total += 1

		}

	}

})

<!\-\- 子组件 -->

<template>

	<button v-on:click="increment">{{ counter }}</button>

</template>

export default {

  name: 'app',

  data: function () {

    return {

      counter: 0

    }

  },

  methods: {

    increment() {

      this.counter += 1

      this.$emit('incre')

    }

  }

}

#### [](#全局事件总线-global-event-bus "全局事件总线(global event bus)")全局事件总线(global event bus)

Vue本身实现了一套独立于原生事件之外的的事件系统，我们可以实例化一个Vue构造函数，在父组件中利用这个实例注册注册一个监听事件，然后在子组件中触发这个事件。

于是父子组件之间的通信可以实现为：

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

<!\-\- 父组件 -->

<template>

	<div id="app">

		<msg :bus='bus'></msg>

	</div>

</template>

var bus = new Vue()

this.bus.$on('evt',function () {

    console.log('On!');

})

export default {

	name: 'app',

	data(){

		return {

			bus: bus

		}

	},

	components: {

		Msg

	}

}

<!\-\- 子组件 -->

<template>

	<div class="hello">

		<h1 @click="test">{{ msg }}</h1>

	</div>

</template>

export default {

	name: 'hello',

	props: \['bus'\],

	data () {

		return {

			msg: 'Welcome to Your Vue.js App'

		}

	},

	methods:{

		test(){

			console.log('Emit!');

			this.bus.$emit('evt');

		}

	}

}

在同一个Vue实例上可以触发并监听事件，因此我们可以在父组件中实例化一个Vue构造函数，在一个组件中绑定监听函数，在另一个平行组件中触发这个函数。

于是同级组件之间的通信可以实现为：

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

<!\-\- 父组件 -->

<template>

	<div id="app">

		<hello :bus='bus'></hello>

		<msg :bus='bus'></msg>

	</div>

</template>

var bus = new Vue();

export default {

	name: 'app',

	data(){

		return {

			bus: bus

		}

	},

	components: {

		Hello,

		Msg

	}

}

<!\-\- 子组件A -->

<template>

	<div class="hello">

		<h1>{{ msg }}</h1>

	</div>

</template>

export default {

	name: 'hello',

	props: \['bus'\],

	data () {

		return {

			msg: 'Welcome to Your Vue.js App'

		}

	},

	mounted(){

		this.bus.$on('evt',function () {

			console.log('On!');

		})

  }

}

<!\-\- 子组件B -->

<template>

	<div class="msg">

		<h1 @click="test">{{ msg }}</h1>

	</div>

</template>

export default {

	name: 'hello',

	props: \['bus'\],

	data () {

		return {

			msg: 'Welcome to Your Vue.js App'

		}

  	},

	methods:{

		test(){

			console.log('Emit!');

			this.bus.$emit('evt');

		}

	}

}

[Vue.js](/tags/Vue-js/)