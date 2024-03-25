<template>
  <div class="adver-float" @click="navigateToUrl(currentAd.url)" v-if="adverList.length > 0">
    <div class="adver-content">
      <div class="adver-img" :style="{ backgroundImage: 'url(' + currentAd.img + ')' }"></div>
      <div class="adver-name">{{ currentAd.name }}</div>
    </div>
  </div>
</template>
<script setup>
import { ref, computed, onMounted, watch, onUnmounted } from "vue";

const { adverList } = defineProps({
  adverList: {
    type: Array,
    required: true,
  },
});
const currentIndex = ref(0);
const currentAd = computed(() => adverList[currentIndex.value]);
// 广告链接
const navigateToUrl = (url) => {
  window.open(url, "_blank");
};

onMounted(() => {
  // 启动循环定时器
  const adChangeInterval = setInterval(() => {
    // 更新当前索引值，如果已经到达最后一个元素，则重新从头开始
    currentIndex.value = currentIndex.value < adverList.length - 1 ? currentIndex.value + 1 : 0;
  }, 5000);
  // 监听 adverList 的变化，当 adverList 变化时将 currentIndex 重置为 0
  watch(
    () => adverList,
    () => {
      currentIndex.value = 0;
    },
  );
  // 在组件卸载时清除定时器
  onUnmounted(() => {
    clearInterval(adChangeInterval);
  });
});
</script>
<style lang="scss" scoped>
.adver-float {
  position: fixed;
  bottom: 0px;
  left: 15px;
  width: 260px;
  height: 165px;
  cursor: pointer;
  animation: floatAnimation 5s infinite;
  border-radius: 10px;
  overflow: hidden;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  /* 添加 z-index 属性确保组件位于顶层 */
  z-index: 1;
}

.adver-content {
  width: 100%;
  height: 100%;
}

.adver-name {
  width: 100%;
  height: 40px;
  line-height: 40px;
  text-align: center;
  background-color: rgba(252, 198, 0, 0.75);
  color: white;
  font-size: 16px;
}

.adver-img {
  width: 100%;
  height: calc(100% - 40px);
  background-size: cover;
  background-position: center;
  filter: blur(2px);
}

@keyframes floatAnimation {
  0%,
  100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-46px);
  }
}
</style>
