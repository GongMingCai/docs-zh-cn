<script setup>
import gsap from 'gsap'

const list = [
  { msg: 'JavaScript' },
  { msg: 'TypeScript' },
  { msg: 'Vue.js' },
  { msg: '开源' },
  { msg: '前端' }
]

let query = $ref('')

const computedList = $computed(() => {
  return list.filter((item) => item.msg.toLowerCase().includes(query))
})

function onBeforeEnter(el) {
  el.style.opacity = 0
  el.style.height = 0
}

function onEnter(el, done) {
  gsap.to(el, {
    opacity: 1,
    height: '1.6em',
    delay: el.dataset.index * 0.15,
    onComplete: done
  })
}

function onLeave(el, done) {
  gsap.to(el, {
    opacity: 0,
    height: 0,
    delay: el.dataset.index * 0.15,
    onComplete: done
  })
}
</script>

<template>
  <div class="demo" style="height: 265px">
    <input v-model="query" style="margin-bottom: 20px" placeholder="输入内容来过滤列表" />
    <TransitionGroup
      tag="ul"
      :css="false"
      @before-enter="onBeforeEnter"
      @enter="onEnter"
      @leave="onLeave"
    >
      <li
        v-for="(item, index) in computedList"
        :key="item.msg"
        :data-index="index"
      >
        {{ item.msg }}
      </li>
    </TransitionGroup>
  </div>
</template>
