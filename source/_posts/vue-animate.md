---
title: Vue动画使用小结
categories: 前端学习笔记
tags: VueJS
comment: true
date: 2018-10-10 20:26:07
---
vue 项目中，动画使用总结。

<!-- more -->

基础 class 名：

- fade-enter
- fade-enter-active
- fade-enter-to
- fade-leave
- fade-leave-active
- fade-leave-to

使用 animate.css 库：

```vue
<style>
    .active {}
    .leave {}
</style>
<!-- 以下类名 替换为 animate 里面的类名就可以了 -->
<transition name="fade" enter-active-class="active" leave-active-class="leave" appear appear-active-class="active"></transition>
<transition :duration="10000" type="transition" name="fade" enter-active-class="animated swing fade-enter-active" leave-active-class="animated swing fade-leave-active" appear appear-active-class="animated swing" @before-enter="handleBeforeEnter">
	<div v-if="show">
        123
    </div>
</transition>
<!-- appear-active-class 功能是让页面刚进入时也有进入动画
type="transition" 表示以 过渡的时长为准
duration 自己定义动画时长 -->
<script>
	handleBeforeEnter(el) {
        el.style.color="red"
	}
</script>
```

```stylus
// 过渡和css3的@keyframes 结合，添加一下类
.fade-enter-active, .fade-leave-active
  transition: opacity .5s
.fade-enter, .fade-leave-to
  opacity: 0
```

使用 JS 动画与 Velocity.js 结合。

github 查找库，看使用方法。

多个元素或组件过渡：

```vue
<style>
    .v-enter, v-leave-to {}
    .v-enter-active, v-leave-active {}
</style>
// mode="in-out" or in-out
<transition mode="in-out">
	<div v-if="show" :key="hello">
        hello
    </div>
    <div v-else key="bye">
        bye bye
    </div>
</transition>

<transition mode="in-out">
	<component is="xxxx"></component>
</transition>
```

列表过渡动画：

```vue
<style>
	.v-enter, v-leave-to {}
    .v-enter-active, v-leave-active {}
</style>
<transition-group>
	<div v-for="item in list">
        {{item.title}}
    </div>
</transition-group>
```

vue 动画组件封装：

```vue
<script>
    Vue.component('fade', {
        props: ['show'],
        template: `
			<transition @before-enter="handleBeforeEnter"><slot v-if="show"></transition>
		`,
        methods: {
            handleBeforeEnter: function(el) {
				el.style.color: 'red'
            },
            handleEnter: function(el, done) {
				setTimeout(() => {
					el.style.color = 'green'
                    done()
                }, 2000)
            }
        }
    })
</script>
```

官网查阅动态过渡、状态过渡等更复杂的动画。