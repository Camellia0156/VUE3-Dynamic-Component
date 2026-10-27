<!--
  许愿瓶：输入一句愿望，状态保存在组件内部的 wish（不依赖外部）。
  被 KeepAlive 缓存时，切走再切回，输入内容仍然保留。
-->
<script setup>
import { ref, onActivated, onDeactivated } from 'vue'

// 显式声明组件 name（KeepAlive 的 include/exclude 靠它匹配）
defineOptions({ name: 'WishBottle' })

const wish = ref('')

onActivated(() => {
  console.log('[WishBottle] onActivated：许愿瓶被激活，瓶中愿望 =', wish.value || '（空）')
})

onDeactivated(() => {
  console.log('[WishBottle] onDeactivated：许愿瓶被缓存，离开页面')
})
</script>

<template>
  <section class="bottle">
    <h2>🌟 许愿瓶</h2>
    <div class="emoji">🍶</div>
    <textarea
      v-model="wish"
      rows="3"
      placeholder="写下你的一句愿望……"
    ></textarea>
    <p class="display">瓶中的愿望：{{ wish || '（还没有写哦）' }}</p>
  </section>
</template>

<style scoped>
.bottle {
  padding: 8px 4px;
}

.emoji {
  font-size: 56px;
  margin: 6px 0 14px;
}

textarea {
  width: 100%;
  resize: vertical;
  padding: 10px 12px;
  border: 2px solid #ffc4b0;
  border-radius: 14px;
  font-size: 15px;
  outline: none;
  background: #fff7f3;
  color: #5a2a3a;
}

textarea:focus {
  border-color: #ff6a88;
}

.display {
  margin-top: 12px;
  font-size: 15px;
  color: #8a3b4d;
  word-break: break-all;
}
</style>
