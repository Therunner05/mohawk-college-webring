<script setup lang="ts">
import { type JobFilters, type TechFilters, type YearFilters } from "./types";
import FilterButtons from "./components/FilterButtons.vue";
import { ref, toRaw } from "vue";
import DirectoryView from "./components/DirectoryView.vue";
import SearchBar from "./components/SearchBar.vue";
const activeJobFilters = ref<JobFilters[]>([]);
const activeTechFilters = ref<TechFilters[]>([]);
const activeYearFilters = ref<YearFilters[]>([]);
const activeSearchFilter = ref<string>("");
function setJobFilters(value: JobFilters) {
  const index = activeJobFilters.value.indexOf(value);
  if (index === -1) {
    activeJobFilters.value.push(value);
  } else {
    activeJobFilters.value.splice(index, 1);
  }
  console.log(toRaw(activeJobFilters.value));
}
function setTechFilters(value: TechFilters) {
  const index = activeTechFilters.value.indexOf(value);
  if (index === -1) {
    activeTechFilters.value.push(value);
  } else {
    activeTechFilters.value.splice(index, 1);
  }
  console.log(toRaw(activeTechFilters.value));
}
function setYearFilters(value: YearFilters) {
  const index = activeYearFilters.value.indexOf(value);
  if (index === -1) {
    activeYearFilters.value.push(value);
  } else {
    activeYearFilters.value.splice(index, 1);
  }
  console.log(toRaw(activeYearFilters.value));
}
</script>

<template>
  <main>
    <header>
      <h1>Mohawk College Webring</h1>
      <p class="header-sub">a directory of student portfolio sites</p>
    </header>
    <SearchBar @set-search-input="activeSearchFilter = $event"></SearchBar>
    <FilterButtons
      :currentJobFilters="activeJobFilters"
      @set-job-filter="setJobFilters"
      :currentTechFilters="activeTechFilters"
      @set-tech-filter="setTechFilters"
      :currentYearFilters="activeYearFilters"
      @set-year-filter="setYearFilters"
    ></FilterButtons>
    <DirectoryView
      :currentJobFilters="activeJobFilters"
      :currentTechFilters="activeTechFilters"
      :currentYearFilters="activeYearFilters"
      :current-search-filter="activeSearchFilter"
    ></DirectoryView>
  </main>
</template>

<style>
*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

html,
body {
  background-color: #111111;
  color: #1a1a1a;
  font-family: "Verdana", "Geneva", sans-serif;
  font-size: 14px;
  line-height: 1.6;
  min-height: 100vh;
  -webkit-font-smoothing: antialiased;
}

main {
  max-width: 100rem;
  margin: 0 auto;
  padding: 2.5rem 1.5rem 4rem;
}

header {
  border-bottom: 1px solid #2e2e2e;
  padding-bottom: 1.5rem;
  margin-bottom: 2rem;
}

.header-ring {
  display: inline-block;
  font-size: 0.65rem;
  font-weight: bold;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  color: #ff9933;
  border: 1px solid #ff9933;
  padding: 0.15rem 0.5rem;
  border-radius: 2px;
  margin-bottom: 0.6rem;
}

header h1 {
  font-size: 1.6rem;
  font-weight: bold;
  color: #e8e8e8;
  letter-spacing: -0.01em;
  line-height: 1.2;
}

header h1 span {
  color: #ff9933;
}

.header-sub {
  margin-top: 0.35rem;
  font-size: 0.8rem;
  color: #888888;
  letter-spacing: 0.02em;
}

a {
  color: #ff9933;
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}
</style>
