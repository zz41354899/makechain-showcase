<script setup lang="ts">
defineProps<{
  title: string;
  description: string;
  url: string;
}>();
</script>

<template>
  <article class="card">
    <div class="cover">
      <img src="/default-cover.png" alt="預設縮圖" />
    </div>

    <h3 class="card-title">{{ title }}</h3>
    <p class="card-desc">{{ description }}</p>
    <a class="btn btn-ghost" :href="url" target="_blank" rel="noreferrer">
      前往作品
    </a>
  </article>
</template>

<style scoped>
.card {
  grid-column: span 12;
  border: 1px solid var(--border);
  border-radius: 12px;
  padding: 12px;
  background: #fff;
  display: flex;
  flex-direction: column;
  gap: 8px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.05);
  transition: all 0.25s ease-in-out;
}
.card:hover {
  transform: scale(1.02);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.08);
}
@media (min-width: 640px) {
  .card {
    grid-column: span 6;
  }
}
@media (min-width: 1024px) {
  .card {
    grid-column: span 3;
  }
}

.cover {
  width: 100%;
  aspect-ratio: 16 / 9;
  background: #f6f6f6;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}
.cover img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.2s ease-in-out;
}
.card:hover .cover img {
  transform: scale(1.02);
}

.card-title {
  font-size: 16px;
  font-weight: 600;
  margin: 6px 0 2px;
  color: var(--text);
}
.card-desc {
  font-size: 14px;
  line-height: 1.5;
  min-height: 42px;
  color: var(--text-muted);
}

.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  height: 38px;
  padding: 0 14px;
  border-radius: 8px;
  font-weight: 600;
  text-decoration: none;
  transition: all 0.25s ease-in-out;
}
.btn-ghost {
  border: 1px solid var(--border);
  background: var(--brand-light);
  color: var(--brand);
}
.btn-ghost:hover {
  background: var(--brand);
  color: white;
}
</style>
