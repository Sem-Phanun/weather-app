<template>
  <div class="flex flex-col min-h-screen font-Roboto bg-weather-primary">
    <SiteNavigationVue/>
    <RouterView class="flex-1" v-slot="{ Component }">
      <Transition name="page">
        <component :is="Component"/>
      </Transition>
    </RouterView>
  </div>
</template>

<script setup>
import { RouterView } from 'vue-router'
import SiteNavigationVue from './components/SiteNavigation.vue'
</script>

<style scoped>
.page-enter-active {
  transition: 600ms ease all;
}

.page-enter-from {
  opacity: 0;
}
</style>