<template>
  <div class="h-screen flex justify-center items-center">

<h1>JSとVue3でショートカットキーを実装してみた！</h1>

<h2>JSではこんな感じ</h2>
<span>Enter + command</span>
<br>
<span>Enter + ctrl</span>
<br>

  <textarea
  
  @keydown.enter="handleKeydownEnter"
  @keyup.enter="handleKeyupEnter"
>ここにカーソル合わせとかないと反応しないぞ！</textarea>　


{{ text }}

<br>
<br>




<h2>Vue3ではこんな感じ</h2>

<div>

<span v-show="show">
    v-hotkey3を使ったらめっちゃ簡単にできる！<br>
    `ctrl と esc` =  toggle <br>
    `enter` =  hide
  </span>
  </div>

</div>
<br>

<h2>ポイントはkeydownとKeyupのようなイベントハンドラ</h2>
参考：<br>
https://blog.35d.jp/2022-08-15-vue3-keydown<br>
https://zenn.dev/imaginelab/articles/c59db2aa20aecf#vue.js%E3%81%AB%E3%82%82%E7%B0%A1%E5%8D%98%E3%81%AB%E8%BF%BD%E5%8A%A0%E3%81%A7%E3%81%8D%E3%82%8B <br>
https://github.com/wobsoriano/v-hotkey3<br>


<br>
基本はイベントハンドラで、keydownとkeyupのようなハンドラを使うことで、キーが押されたときと離されたときの処理を分けることができる。
vueはvue2,3でhotkeyを活用すると、キー名をそのまま指定できる。(例：'ctrl+esc+K')と直接かける。
そうじゃない場合は、KeyboardEventを使い、metaKeyと書くとctrlキーを意味している、という感じで若干の翻訳が必要。<br>
参考：<br>
https://developer.mozilla.org/ja/docs/Web/API/KeyboardEvent<br>
https://w3c.github.io/uievents/#interface-keyboardevent<br>


ちなみに。。。Reactもほとんどおんなじ実装になる。<br>
https://blog.stin.ink/articles/react-hooks-keybind


</template>

<script setup lang="ts">

import { useHotkey } from 'v-hotkey3'
import { ref } from 'vue'

const show = ref(true)
const text = ref('')

useHotkey({
  'ctrl+esc+K': () => {
    show.value = !show.value
  },
  'enter': {
    keydown() {
      show.value = false
    },
    keyup() {
      show.value = true
    }
  }
})


const handleKeydownEnter = (e: KeyboardEvent) => {
  // 同時押ししているキーが、Ctrl キーでも Meta（Command） キーでもない場合は何もしない
  if (e.metaKey) 
    text.value = "Command キーが押されたよ"
};

const handleKeyupEnter = (e: KeyboardEvent) => {
  // 同時押ししているキーが、Ctrl キーでも Meta（Command） キーでもない場合は何もしない
  if (e.ctrlKey) 
    text.value = "Ctrl キーが離されました"
};

</script>

