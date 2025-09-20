<script setup lang="ts">
import { ref, computed, onMounted } from "vue";
import { supabase } from "./lib/supabase";
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";
import ProjectCard from "./components/ProjectCard.vue";
import UploadModal from "./components/UploadModal.vue";
import Pagination from "./components/Pagination.vue";

type Project = {
  id: number;
  title: string;
  description: string;
  url: string;
  created_at: string;
};

const allProjects = ref<Project[]>([]);
const currentPage = ref(1);
const pageSize = 20;
const showUpload = ref(false);
const loading = ref(false);

const totalProjects = computed(() => allProjects.value.length);
const totalPages = computed(() =>
  totalProjects.value > 0 ? Math.ceil(totalProjects.value / pageSize) : 1
);

const pagedProjects = computed(() => {
  const start = (currentPage.value - 1) * pageSize;
  return allProjects.value.slice(start, start + pageSize);
});

async function fetchProjects() {
  loading.value = true;
  const { data, error } = await supabase
    .from("projects")
    .select("id, title, description, url, created_at")
    .order("created_at", { ascending: false })
    .limit(200);
  if (!error && data) allProjects.value = data;
  loading.value = false;
}

function onPageChange(page: number) {
  currentPage.value = page;
  window.scrollTo({ top: 0, behavior: "smooth" });
}

onMounted(fetchProjects);
</script>

<template>
  <Header @upload="showUpload = true" />

  <main class="container main">
    <h1 class="page-title">創作探索</h1>
    <p class="project-count">目前共有 {{ totalProjects }} 筆作品</p>

    <div v-if="loading" class="hint">載入中…</div>

    <div class="grid">
      <ProjectCard
        v-for="p in pagedProjects"
        :key="p.id"
        :title="p.title"
        :description="p.description"
        :url="p.url"
      />
    </div>

    <Pagination
      v-if="totalProjects > 0"
      :current-page="currentPage"
      :total-pages="totalPages"
      @change="onPageChange"
    />
  </main>

  <UploadModal
    v-if="showUpload"
    @close="showUpload = false"
    @refresh="fetchProjects"
  />
  <Footer />
</template>
