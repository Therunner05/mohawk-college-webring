<script setup lang="ts">
import { ref } from "vue";

const searchInput = ref<string>("");
const emits = defineEmits<{ setSearchInput: [input: string] }>();
</script>

<template>
  <div>
    <input
      v-model="searchInput"
      @input="emits('setSearchInput', searchInput)"
      placeholder="Search members by name..."
    />
  </div>
</template>

<style scoped>
input {
  margin: auto;
  width: 97rem;
  border-radius: 8px;
  background-color: #2e2e2e;
  padding: 1rem;
  border: 1px solid #2e2e2e;
  color: #e8e8e8;
}

input:focus {
  outline: none;
}
</style>
