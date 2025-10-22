<script setup lang="ts">
import { showToast } from 'vant';
import { ref } from 'vue'

defineProps<{ msg: string }>()

const count = ref(0)

const handleClick = () => {
  count.value++
  showToast(`Current count is ${count.value}`);
}
</script>

<template>
  <p class="text-4xl text-[#2d71fb]">{{ msg }}</p>

  <div class="card">
    <!-- <button type="button" @click="count++">count is {{ count }}</button> -->
    <van-button type="danger" @click="handleClick">count is {{ count }}</van-button>
    <p>
      Edit
      <code>components/HelloWorld.vue</code> to test HMR
    </p>
  </div>

  <p>
    Check out
    <a href="https://vuejs.org/guide/quick-start.html#local" target="_blank"
      >create-vue</a
    >, the official Vue + Vite starter
  </p>
  <p>
    Learn more about IDE Support for Vue in the
    <a
      href="https://vuejs.org/guide/scaling-up/tooling.html#ide-support"
      target="_blank"
      >Vue Docs Scaling up Guide</a
    >.
  </p>
  <p class="read-the-docs">Click on the Vite and Vue logos to learn more</p>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
