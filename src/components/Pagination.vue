<script setup lang="ts">
const props = defineProps<{
  currentPage: number;
  totalPages: number;
}>();

const emit = defineEmits<{ (e: "change", page: number): void }>();

function goTo(page: number) {
  if (page >= 1 && page <= props.totalPages) {
    emit("change", page);
  }
}
</script>

<template>
  <div class="pagination">
    <button
      :disabled="props.currentPage <= 1"
      @click="goTo(props.currentPage - 1)"
    >
      上一頁
    </button>
    <span>第 {{ props.currentPage }} / {{ props.totalPages }} 頁</span>
    <button
      :disabled="props.currentPage >= props.totalPages"
      @click="goTo(props.currentPage + 1)"
    >
      下一頁
    </button>
  </div>
</template>
