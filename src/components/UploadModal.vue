<script setup lang="ts">
import { ref } from "vue";
import { supabase } from "../lib/supabase";

const emit = defineEmits<{ (e: "close"): void; (e: "refresh"): void }>();

const title = ref("");
const description = ref("");
const url = ref("");

const isValid = () =>
  title.value.trim().length > 0 &&
  description.value.trim().length > 0 &&
  /^(https?:\/\/)/.test(url.value.trim());

async function submit() {
  if (!isValid()) return;
  const { error } = await supabase.from("projects").insert([
    {
      title: title.value.trim(),
      description: description.value.trim(),
      url: url.value.trim(),
    },
  ]);
  if (error) {
    alert("上傳失敗，請稍後再試");
    return;
  }
  title.value = "";
  description.value = "";
  url.value = "";
  emit("refresh");
  emit("close");
}
</script>

<template>
  <div class="modal-backdrop" @click.self="emit('close')">
    <div class="modal">
      <h2 class="modal-title">上傳作品</h2>

      <label class="field">
        <span>作品名稱*</span>
        <input
          v-model="title"
          type="text"
          maxlength="120"
          placeholder="例：履歷寫作建議平台"
        />
      </label>

      <label class="field">
        <span>作品描述*</span>
        <textarea
          v-model="description"
          rows="4"
          maxlength="800"
          placeholder="簡述你的作品與用途"
        ></textarea>
      </label>

      <label class="field">
        <span>作品連結*</span>
        <input v-model="url" type="url" placeholder="https://example.com" />
      </label>

      <div class="modal-actions">
        <button class="btn btn-ghost" @click="emit('close')">取消</button>
        <button class="btn btn-primary" :disabled="!isValid()" @click="submit">
          送出
        </button>
      </div>
      <p class="tiny">* 必填，送出後會顯示於作品牆。</p>
    </div>
  </div>
</template>

<style scoped>
.modal-backdrop {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.3);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 16px;
  z-index: 50;
}
.modal {
  width: 100%;
  max-width: 520px;
  background: #fff;
  border-radius: 12px;
  padding: 24px;
  box-shadow: 0 12px 40px rgba(0, 0, 0, 0.1);
}
.modal-title {
  font-size: 18px;
  font-weight: 600;
  margin-bottom: 12px;
  padding-bottom: 12px;
  border-bottom: 1px solid var(--border);
  color: var(--text);
}
.field {
  display: flex;
  flex-direction: column;
  gap: 8px;
  margin: 10px 0;
}
.field input,
.field textarea {
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 10px 12px;
  font-size: 14px;
  transition: all 0.25s ease-in-out;
}
.field input:focus,
.field textarea:focus {
  outline: none;
  border-color: var(--brand);
  box-shadow: 0 0 0 3px rgba(22, 163, 74, 0.15);
}
.modal-actions {
  display: flex;
  justify-content: flex-end;
  gap: 8px;
  margin-top: 8px;
}
.btn {
  height: 36px;
  padding: 0 14px;
  border-radius: 8px;
  cursor: pointer;
  font-weight: 600;
  transition: all 0.25s ease-in-out;
}
.btn-ghost {
  background: #fff;
  border: 1px solid var(--border);
  color: var(--text);
}
.btn-ghost:hover {
  background: #f5f5f5;
}
.btn-primary {
  background: var(--brand);
  color: #fff;
  border: none;
}
.btn-primary:hover {
  background: var(--brand-hover);
}
.btn-primary:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}
.tiny {
  font-size: 12px;
  color: var(--text-muted);
  margin-top: 4px;
}
</style>
