<script setup lang="ts">
import AppSidebar from '@/components/Sidebar/index.vue'
</script>

<template>
  <div class="flex h-screen">
    <AppSidebar />
    <main class="flex-1 p-6">
      <NuxtPage />
    </main>
  </div>
</template>