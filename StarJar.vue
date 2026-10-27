<!--
  星星罐：点击按钮增加星星数量，状态保存在组件内部的 stars（不依赖外部）。
  被 KeepAlive 缓存时，切走再切回，星星数量仍然保留。
-->
<script setup>
import { ref, onActivated, onDeactivated } from 'vue'

// 显式声明组件 name（KeepAlive 的 include/exclude 靠它匹配）
defineOptions({ name: 'StarJar' })

const stars = ref(0)
const addStar = () => {
  stars.value++
}

onActivated(() => {
  console.log('[StarJar] onActivated：星星罐被激活，当前星星数 =', stars.value)
})

onDeactivated(() => {
  console.log('[StarJar] onDeactivated：星星罐被缓存，离开页面')
})
</script>

<template>
  <section class="jar">
    <h2>✨ 星星罐</h2>
    <div class="emoji">{{ stars > 0 ? '⭐'.repeat(Math.min(stars, 30)) : '🫙' }}</div>
    <p class="display">星星数量：{{ stars }}</p>
    <button class="add" @click="addStar">➕ 增加一颗星星</button>
  </section>
</template>

<style scoped>
.jar {
  padding: 8px 4px;
}

.emoji {
  font-size: 40px;
  line-height: 1.4;
  min-height: 56px;
  margin: 6px 0 14px;
  word-break: break-all;
}

.display {
  margin: 0 0 14px;
  font-size: 16px;
  color: #8a3b4d;
}

.add {
  padding: 11px 22px;
  border: none;
  border-radius: 14px;
  background: linear-gradient(135deg, #ffb86c, #ff6a88);
  color: #fff;
  font-size: 15px;
  cursor: pointer;
  box-shadow: 0 6px 14px rgba(255, 106, 136, 0.4);
  transition: transform 0.15s ease;
}

.add:active {
  transform: scale(0.95);
}
</style>
