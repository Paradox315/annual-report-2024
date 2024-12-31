<template>
  <UContainer class="min-h-screen">
    <!-- 头部区域 -->
    <div class="text-center space-y-4 py-12">
      <UBadge size="lg" class="animate-fade-in">{{ data.headline }}</UBadge>
      <h1 class="text-4xl font-bold tracking-tight animate-fade-in-up">
        {{ data.title }}
      </h1>
      <p class="text-gray-500 dark:text-gray-400 max-w-2xl mx-auto animate-fade-in-up delay-200">
        {{ data.description }}
      </p>
    </div>

    <!-- 主体内容区域 -->
    <div class="flex gap-8 relative min-h-[600px]">
      <!-- 左侧进度条 -->
      <div class="w-64 shrink-0 space-y-6">
        <div 
          v-for="(section, index) in data.sections" 
          :key="section.title"
          class="progress-item"
          :class="{ 'active': currentSection === index }"
          @click="setCurrentSection(index)"
        >
          <div class="flex items-center gap-3 cursor-pointer mb-2">
            <UIcon 
              :name="section.icon" 
              class="w-5 h-5"
              :class="currentSection === index ? 'text-primary-500' : 'text-gray-400'"
            />
            <span 
              class="text-sm font-medium transition-colors"
              :class="currentSection === index ? 'text-primary-500' : 'text-gray-500'"
            >
              {{ section.title }}
            </span>
          </div>
          <UProgress 
            :value="currentSection >= index ? 100 : 0"
            :color="currentSection === index ? 'primary' : 'gray'"
            class="h-1"
          />
        </div>
      </div>

      <!-- 右侧内容区域 -->
      <div class="flex-1 relative">
        <TransitionGroup 
          name="fade" 
          tag="div" 
          class="h-full"
        >
          <div 
            v-for="(section, index) in data.sections"
            :key="section.title"
            v-show="currentSection === index"
            class="absolute inset-0 rounded-xl overflow-hidden"
          >
            <!-- 内容卡片 -->
            <div class="h-full flex gap-8 bg-gray-50 dark:bg-gray-800 p-8 rounded-xl">
              <!-- 左侧图片 -->
              <div class="w-1/2">
                <img 
                  :src="section.image"
                  :alt="section.title"
                  class="w-full h-full object-cover rounded-lg"
                />
              </div>
              
              <!-- 右侧信息 -->
              <div class="w-1/2 space-y-6">
                <h3 class="text-2xl font-bold">{{ section.title }}</h3>
                <p class="text-gray-600 dark:text-gray-300">{{ section.description }}</p>
                
                <!-- 数据亮点 -->
                <div v-if="section.highlights" class="grid grid-cols-2 gap-4">
                  <div 
                    v-for="highlight in section.highlights"
                    :key="highlight.label"
                    class="bg-white dark:bg-gray-700 p-4 rounded-lg"
                  >
                    <div class="flex items-center gap-2 text-gray-500 dark:text-gray-400 mb-2">
                      <UIcon :name="highlight.icon" class="w-5 h-5" />
                      <span class="text-sm">{{ highlight.label }}</span>
                    </div>
                    <div class="text-2xl font-bold text-primary-500">
                      {{ highlight.value }}
                      <span v-if="highlight.unit" class="text-sm ml-1">{{ highlight.unit }}</span>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </TransitionGroup>
      </div>
    </div>
  </UContainer>
</template>

<script setup>
const { data } = await useAsyncData('study', () => queryContent('study').findOne())
const currentSection = ref(0)

// 自动播放
let autoplayInterval
onMounted(() => {
  autoplayInterval = setInterval(() => {
    currentSection.value = (currentSection.value + 1) % data.value.sections.length
  }, 5000)
})

onUnmounted(() => {
  clearInterval(autoplayInterval)
})

const setCurrentSection = (index) => {
  currentSection.value = index
}
</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: all 0.5s ease;
}

.fade-enter-from {
  opacity: 0;
  transform: translateX(30px);
}

.fade-leave-to {
  opacity: 0;
  transform: translateX(-30px);
}

.progress-item {
  cursor: pointer;
  transition: all 0.3s ease;
}

.progress-item:hover {
  transform: translateX(5px);
}
</style>