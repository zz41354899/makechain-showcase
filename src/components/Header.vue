<template>
  <header class="header">
    <div class="brand">
      <img src="/Logo.svg" alt="Makechain logo" class="logo" />
    </div>
    <button class="btn btn-primary" @click="$emit('upload')">上傳作品</button>
  </header>
</template>

<script setup lang="ts">
defineEmits<{
  (e: "upload"): void;
}>();
</script>
