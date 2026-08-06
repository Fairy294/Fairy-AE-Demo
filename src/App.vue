<script setup>
import { computed, onMounted, onUnmounted, ref } from 'vue'

const frames = [
  { title: 'Fairy · 系统启动', body: '你好，主人。\n我是 III 型急序式集成泛用人工智能，开发代号 Fairy。\n欢迎使用我的 AE。' },
  { title: 'Fairy · 动态响应', body: '检测到视觉界面连接。\n我会用柔和的光效回应你的每一次操作。' },
  { title: 'Fairy · 等待指令', body: '核心模块运行正常。\n点击右下角继续，或者让我为你播放一段演示。' },
]
const active = ref(0)
const playing = ref(true)
const elapsed = ref(8)
const muted = ref(false)
const menuOpen = ref(false)
let timer
const current = computed(() => frames[active.value])
const progress = computed(() => `${(elapsed.value / 42) * 100}%`)
function next() { active.value = (active.value + 1) % frames.length; elapsed.value = 8 }
function togglePlay() { playing.value = !playing.value }
function seek(e) { const r = e.currentTarget.getBoundingClientRect(); elapsed.value = Math.round(((e.clientX-r.left)/r.width)*42) }
onMounted(() => { timer = window.setInterval(() => { if (playing.value) { elapsed.value++; if (elapsed.value >= 42) next() } }, 1000) })
onUnmounted(() => clearInterval(timer))
</script>

<template>
  <main class="screen" @click.self="menuOpen = false">
    <div class="film film-top"></div>
    <div class="film film-bottom"></div>
    <div class="atmosphere atmosphere-a"></div><div class="atmosphere atmosphere-b"></div>
    <header class="topbar">
      <h1>动态效果演示</h1>
      <button class="menu" @click="menuOpen = !menuOpen"><span></span><b>Menu</b></button>
      <div v-if="menuOpen" class="menu-pop"><span>Fairy Interface</span><span>v1.0 · Online</span></div>
    </header>

    <section class="stage">
      <div class="orbital orbital-outer"></div><div class="orbital orbital-mid"></div>
      <div class="orbital orbital-inner"><div class="core"></div><div class="pearl"></div></div>
      <div class="spark spark-one"></div><div class="spark spark-two"></div><div class="spark spark-three"></div>
    </section>

    <section class="dialogue">
      <div class="nameplate">{{ current.title }}</div>
      <div class="dialogue-copy"><span v-for="(line, i) in current.body.split('\n')" :key="i">{{ line }}</span></div>
      <button class="continue" @click="next" aria-label="下一段">»</button>
    </section>

    <section class="player">
      <button class="player-btn" @click="togglePlay">{{ playing ? '❚❚' : '▶' }}</button>
      <button class="player-btn skip" @click="next">▶|</button>
      <span class="time">00:{{ String(elapsed).padStart(2,'0') }} / 00:42</span>
      <div class="track" @click="seek"><i :style="{ width: progress }"></i></div>
      <span class="quality">1080P <em>60帧</em></span>
      <button class="player-btn" @click="muted = !muted">{{ muted ? '🔇' : '🔊' }}</button>
      <button class="player-btn gear">⚙</button>
      <button class="player-btn expand">⛶</button>
    </section>
  </main>
</template>

<style>
:root { font-family: Arial, 'Microsoft YaHei', sans-serif; color:#fff; background:#020812; font-synthesis:none }
* { box-sizing:border-box } body { margin:0; overflow:hidden; background:#020812 }
button { font:inherit; color:inherit; border:0; cursor:pointer }
.screen { min-height:100vh; position:relative; overflow:hidden; isolation:isolate; background:radial-gradient(ellipse at 50% 47%, #123c78 0%, #071b3b 24%, #020916 67%, #01050d 100%) }
.screen:before { content:''; position:absolute; inset:0; z-index:-1; opacity:.5; background:repeating-linear-gradient(0deg, transparent 0 5px, rgba(41,119,221,.08) 6px, transparent 7px), radial-gradient(ellipse at center, transparent 25%, #000 100%) }
.film { position:absolute; left:0; right:0; height:26px; z-index:2; opacity:.8; background:repeating-linear-gradient(90deg, transparent 0 13px, #06111d 13px 26px), #1f405c; box-shadow:0 0 20px #000 }
.film:after { content:''; position:absolute; inset:5px 0; background:repeating-linear-gradient(90deg, #020a13 0 11px, transparent 11px 26px) }
.film-top { top:0 } .film-bottom { bottom:0 }
.topbar { position:relative; z-index:3; display:flex; justify-content:space-between; align-items:flex-start; padding:60px 6.5vw 0 }
h1 { margin:0; font-size:clamp(28px,4vw,62px); letter-spacing:3px; font-weight:900; text-shadow:3px 4px 0 #071126, 0 0 15px #77bdff; transform:skew(-5deg) }
.menu { display:flex; align-items:center; gap:12px; background:rgba(0,0,0,.72); padding:13px 20px; border-radius:25px; font-size:19px; box-shadow:0 0 0 1px #22374d inset }
.menu span { width:22px; height:22px; border-radius:50%; background:#caff43; display:inline-block; position:relative }.menu span:after { content:'☰'; color:#102016; position:absolute; font-size:13px; left:4px; top:1px }.menu-pop { position:absolute; right:6.5vw; top:116px; padding:13px 17px; display:grid; gap:7px; background:#061126; border:1px solid #4679a7; border-radius:9px; box-shadow:0 8px 24px #0009; color:#b7daf8 }
.atmosphere { position:absolute; width:70vw; height:42vw; border-radius:50%; filter:blur(35px); opacity:.38; z-index:-1 }.atmosphere-a { left:15%; top:24%; background:#0b70ff; animation:pulse 4s infinite alternate }.atmosphere-b { left:30%; top:35%; background:#a7eaff; opacity:.13; animation:drift 7s infinite alternate }
.stage { position:absolute; left:50%; top:44%; width:min(52vw,610px); aspect-ratio:1; transform:translate(-50%,-50%); filter:drop-shadow(0 0 25px #369dff); animation:float 5s ease-in-out infinite }
.orbital { position:absolute; border-radius:50%; left:50%; top:50%; transform:translate(-50%,-50%); border:2px solid #78d4ff; box-shadow:0 0 10px #1689ff, inset 0 0 20px #126fca }
.orbital-outer { width:100%; height:100%; border-color:#2068b5; opacity:.7; animation:spin 18s linear infinite }.orbital-mid { width:76%; height:76%; border-width:10px; border-color:#d8f4ff #477eac #9fdfff #244b7d; animation:spin-reverse 12s linear infinite }.orbital-inner { width:54%; height:54%; border:18px solid #ecfaff; background:#082654; box-shadow:0 0 0 7px #6aa0ce, 0 0 25px 4px #49b5ff; animation:breathe 3s ease-in-out infinite }
.core { position:absolute; inset:18%; border-radius:50%; background:radial-gradient(circle at 38% 33%, #2679bd, #061b45 62%, #021129); box-shadow:inset 0 0 20px #000, 0 0 11px #75d4ff }.pearl { position:absolute; width:26%; aspect-ratio:1; right:-11%; bottom:5%; border-radius:50%; background:#f5eed6; box-shadow:0 0 10px #fff7d8 }
.spark { position:absolute; width:6%; height:1px; background:#a9e4ff; box-shadow:0 0 12px 4px #37aaff; animation:blink 1.4s infinite alternate }.spark-one { left:8%; top:35%; transform:rotate(20deg) }.spark-two { right:4%; top:63%; transform:rotate(-18deg); animation-delay:.5s }.spark-three { left:25%; bottom:6%; transform:rotate(-45deg); animation-delay:1s }
.dialogue { position:absolute; z-index:3; left:7%; right:7%; bottom:12%; min-height:156px; padding:43px 70px 27px 45px; border:4px solid #172535; border-radius:9px; background:linear-gradient(120deg,#03060bfa,#07101cf7); box-shadow:0 0 0 1px #7892a744 inset, 0 10px 26px #0008 }.nameplate { position:absolute; top:-25px; left:50%; transform:translateX(-50%); padding:10px 34px 12px; min-width:150px; text-align:center; background:#111b27; border:3px solid #394756; border-radius:8px; font-size:20px; font-weight:bold }.dialogue-copy { display:grid; gap:8px; font-size:clamp(16px,2vw,27px); line-height:1.35; font-weight:bold; white-space:pre-wrap; text-shadow:0 2px 2px #000 }.continue { position:absolute; right:25px; bottom:18px; background:transparent; font-size:62px; line-height:30px; color:#fff; animation:arrow 1s infinite alternate }
.player { position:absolute; z-index:4; bottom:27px; left:0; right:0; height:54px; padding:0 5%; display:flex; gap:18px; align-items:center; background:linear-gradient(transparent,rgba(0,0,0,.85)); color:#e7f5ff }.player-btn { background:transparent; min-width:24px; font-size:17px }.skip { font-size:15px }.time { font-variant-numeric:tabular-nums; font-size:14px; white-space:nowrap }.track { height:5px; background:#73889b77; border-radius:4px; flex:1; cursor:pointer }.track i { display:block; height:100%; background:#51c9ff; box-shadow:0 0 8px #62d7ff; border-radius:4px }.quality { white-space:nowrap; font-size:14px; font-weight:bold }.quality em { color:#8aa2b7; font-style:normal; margin-left:3px }.gear,.expand { font-size:20px }
@keyframes spin{to{transform:translate(-50%,-50%) rotate(360deg)}} @keyframes spin-reverse{to{transform:translate(-50%,-50%) rotate(-360deg)}} @keyframes float{50%{transform:translate(-50%,-53%) scale(1.025)}} @keyframes breathe{50%{box-shadow:0 0 0 11px #6aa0ce,0 0 35px 8px #49b5ff}} @keyframes pulse{to{transform:scale(1.15);opacity:.55}} @keyframes drift{to{transform:translate(7%, -4%) scale(1.2)}} @keyframes blink{to{opacity:.25;transform:scaleX(2)}} @keyframes arrow{to{transform:translateX(8px);opacity:.65}}
@media(max-width:650px){.topbar{padding:45px 5vw 0} h1{font-size:30px}.menu{padding:10px 13px;font-size:15px}.stage{width:78vw;top:39%}.dialogue{left:4%;right:4%;bottom:13%;padding:37px 42px 24px 22px;min-height:190px}.dialogue-copy{font-size:16px}.nameplate{font-size:15px;padding:8px 20px;top:-21px}.player{gap:8px;padding:0 3%;}.quality{display:none}.time{font-size:12px}.continue{right:10px}}
</style>
