<script setup lang="ts">
import { type JobFilters, type TechFilters, type YearFilters } from "../types";

const JOBFILTERS: JobFilters[] = [
  "Seeking co-op",
  "Employed",
  "Not looking for work",
  "Seeking full-time",
];

const TECHFILTERS: TechFilters[] = [
  "React",
  "TypeScript",
  "JavaScript",
  "Vue",
  "Angular",
  "Node.js",
  "ASP.NET",
  "C",
  "C++",
  "C#",
  "Linux",
  "System Administration",
];

const YEARFILTERS: YearFilters[] = [2024, 2025, 2026, 2027, 2028];

const props = defineProps<{
  currentJobFilters: JobFilters[];
  currentTechFilters: TechFilters[];
  currentYearFilters: YearFilters[];
}>();

const emits = defineEmits<{
  setJobFilter: [filter: JobFilters];
  setTechFilter: [filter: TechFilters];
  setYearFilter: [filter: YearFilters];
}>();
</script>

<template>
  <div class="container">
    <button
      @click="emits('setJobFilter', filter)"
      v-for="filter in JOBFILTERS"
      v-bind:class="{
        'active-button': props.currentJobFilters.includes(filter),
        button: !props.currentJobFilters.includes(filter),
      }"
    >
      {{ filter }}
    </button>
  </div>
  <div class="container">
    <button
      @click="emits('setTechFilter', filter)"
      v-for="filter in TECHFILTERS"
      :key="filter"
      v-bind:class="{
        'active-button': props.currentTechFilters.includes(filter),
        button: !props.currentTechFilters.includes(filter),
      }"
    >
      {{ filter }}
    </button>
  </div>
  <div class="container">
    <button
      @click="emits('setYearFilter', filter)"
      v-for="filter in YEARFILTERS"
      :key="filter"
      v-bind:class="{
        'active-button': props.currentYearFilters.includes(filter),
        button: !props.currentYearFilters.includes(filter),
      }"
    >
      {{ filter }}
    </button>
  </div>
</template>

<style scoped>
.container {
  display: flex;
  padding: 0.8rem;
  border-radius: 8px;
  background-color: #1a1a1a;
  border: 1px solid #2e2e2e;
  gap: 0.6rem;
  margin-top: 0.8rem;
  margin-bottom: 0.8rem;
  flex-wrap: wrap;
}

.button {
  border: 1px solid #ff9933;
  border-radius: 8px;
  background-color: #2e2e2e;
  color: #e8e8e8;
  font-size: 1.2rem;
  padding: 0.8rem 1.3rem;
  transition: background-color 0.25s ease;
}

.active-button {
  border: 1px solid #000000;
  border-radius: 8px;
  background-color: #ff9933;
  color: #1a1a1a;
  font-size: 1.2rem;
  padding: 0.8rem 1.3rem;
  transition: background-color 0.25s ease;
}
</style>
