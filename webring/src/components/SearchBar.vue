<script setup lang="ts">
import { ref } from "vue";

const searchInput = ref<string>("");
const emits = defineEmits<{ setSearchInput: [input: string] }>();

const handleInput = (event: Event) => {
  const target = event.target as HTMLInputElement;
  emits("setSearchInput", target.value);
};
</script>

<template>
  <div class="search-wrapper">
    <input
      type="search"
      v-model="searchInput"
      @input="handleInput"
      placeholder="Search members by name..."
      class="search-input"
    />
  </div>
</template>

<style scoped>
.search-wrapper {
  width: 100%;
}

.search-input {
  width: 100%;
  background-color: #1e1e1e;
  color: #cccccc;
  border: 1px solid #333333;
  padding: 1rem 1.25rem;
  font-size: 1rem;
  font-family: inherit;
  border-radius: 4px;
  appearance: none;
  transition:
    border-color 0.2s ease,
    outline 0.2s ease;
}

.search-input::placeholder {
  color: #888888;
}

.search-input:focus {
  outline: none;
}
</style>
