<!--
  🪄 魔法口袋 —— Vue 3 + <script setup> 入门示例（适合初学者）

  功能要点：
    1) 父组件用 <component :is="..."> 在“许愿瓶 / 星星罐”两个子组件之间切换；
       注意：组件对象放进普通对象 pages，不要放进普通 ref。
    2) 用 <KeepAlive> 缓存子组件，状态分别保存在各自子组件内部
       （不依赖路由、localStorage、接口或全局共享状态）。
    3) 两个子组件都显式声明 name，并在 onActivated / onDeactivated 中打印日志。
    4) 提供 include / exclude / max 三个配置变量：
         - 默认 include 包含两页，max = 2（两页都缓存，来回切换状态都在）
         - 把 max 改成 1：最多只保留 1 个实例。切换到第 2 页时第 1 页会被“淘汰”，
           再切回第 1 页会发现它被重新创建，之前输入/点击的状态丢失
           （看控制台 onActivated/onDeactivated 日志 + 页面内容即可验证）。

  如何运行（需要已安装 Node.js）：
    npm install
    npm run dev
  打开终端给出的 http://localhost:5173 ，按 F12 打开“控制台(Console)”查看生命周期日志。
-->
<script setup>
import { ref } from 'vue'
import WishBottle from './components/WishBottle.vue'
import StarJar from './components/StarJar.vue'

// 组件对象映射表：用普通对象保存，不放进普通 ref
const pages = {
  WishBottle,
  StarJar,
}

// 当前显示的页面 key
const current = ref('WishBottle')

// —— KeepAlive 配置变量 ——
const includeList = ref(['WishBottle', 'StarJar']) // 需要缓存的组件 name 列表
const excludeList = ref([])                         // 不缓存的组件 name 列表
const maxCount = ref(2)                            // 同时缓存的最大实例数（改成 1 看淘汰效果）

function switchTo(name) {
  current.value = name
}
</script>

<template>
  <div class="pocket">
    <h1>🪄 魔法口袋</h1>
    <p class="sub">当前 max = {{ maxCount }}（切换页面，观察状态与控制台日志）</p>

    <nav class="tabs">
      <button :class="{ active: current === 'WishBottle' }" @click="switchTo('WishBottle')">
        🌟 许愿瓶
      </button>
      <button :class="{ active: current === 'StarJar' }" @click="switchTo('StarJar')">
        ✨ 星星罐
      </button>
    </nav>

    <div class="stage">
      <KeepAlive :include="includeList" :exclude="excludeList" :max="maxCount">
        <component :is="pages[current]" />
      </KeepAlive>
    </div>
  </div>
</template>

<style scoped>
.pocket {
  width: 100%;
  max-width: 420px;
  background: rgba(255, 255, 255, 0.82);
  border-radius: 22px;
  padding: 24px;
  box-shadow: 0 18px 40px rgba(214, 91, 111, 0.28);
  backdrop-filter: blur(4px);
  text-align: center;
}

h1 {
  margin: 0 0 4px;
  font-size: 26px;
}

.sub {
  margin: 0 0 18px;
  font-size: 13px;
  color: #b5556a;
}

.tabs {
  display: flex;
  gap: 10px;
  justify-content: center;
  margin-bottom: 16px;
}

.tabs button {
  flex: 1;
  padding: 10px 0;
  border: 2px solid transparent;
  border-radius: 14px;
  background: #ffe3d3;
  color: #8a3b4d;
  font-size: 15px;
  cursor: pointer;
  transition: all 0.2s ease;
}

.tabs button.active {
  background: linear-gradient(135deg, #ff9a8b, #ff6a88);
  color: #fff;
  border-color: #ffd1a9;
  box-shadow: 0 6px 14px rgba(255, 106, 136, 0.4);
}

.stage {
  min-height: 200px;
}
</style>
