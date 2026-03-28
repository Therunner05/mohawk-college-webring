<script setup lang="ts">
import { computed } from "vue";
import {
  type JobFilters,
  type Member,
  type TechFilters,
  type YearFilters,
} from "../types";
import membersArray from "../../../members.json";
const props = defineProps<{
  currentJobFilters: JobFilters[];
  currentTechFilters: TechFilters[];
  currentYearFilters: YearFilters[];
  currentSearchFilter: string;
}>();
const members: Member[] = membersArray as Member[];
const filteredMembers = computed(() => {
  return members.filter((member) => {
    let jobCheck = false;
    let techCheck = false;
    let yearCheck = false;
    let nameCheck = false;
    if (
      props.currentSearchFilter.length === 0 ||
      member.name
        .toLowerCase()
        .startsWith(props.currentSearchFilter.toLowerCase())
    ) {
      nameCheck = true;
    }
    if (
      props.currentJobFilters.length === 0 ||
      props.currentJobFilters.includes(member.jobStatus)
    ) {
      jobCheck = true;
    }
    if (
      props.currentTechFilters.length === 0 ||
      member.tags.some((tag) =>
        props.currentTechFilters.includes(tag as TechFilters),
      )
    ) {
      techCheck = true;
    }
    if (
      props.currentYearFilters.length === 0 ||
      props.currentYearFilters.includes(member.graduationYear as YearFilters)
    ) {
      yearCheck = true;
    }
    return jobCheck && techCheck && yearCheck && nameCheck;
  });
});
</script>

<template>
  <div class="members-grid">
    <div
      v-for="member in filteredMembers"
      :key="member.name"
      class="member-card"
    >
      <h5 class="member-name">
        <a :href="member.url">{{ member.name }}</a>
      </h5>
      <p class="member-info">
        Class of {{ member.graduationYear }} &middot; {{ member.jobStatus }}
      </p>
      <div class="tags-container">
        <span v-for="tag in member.tags" :key="tag" class="tag">
          {{ tag }}
        </span>
      </div>
    </div>
  </div>
</template>

<style scoped>
.members-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
  gap: 1.25rem;
}

.member-card {
  display: flex;
  flex-direction: column;
  background-color: #1a1a1a;
  border: 1px solid #2e2e2e;
  padding: 1.4rem 1.5rem;
  gap: 0.4rem;
  transition: background-color 0.15s ease;
}

.member-card:hover {
  background-color: #222222;
}

.member-name {
  margin: 0;
  font-size: 1rem;
  font-weight: bold;
  letter-spacing: 0.01em;
}

.member-name a {
  color: #e8e8e8;
  text-decoration: none;
}

.member-name a:hover {
  color: #ff9933;
}

.member-info {
  margin: 0;
  font-size: 0.75rem;
  color: #888888;
  letter-spacing: 0.02em;
}

.tags-container {
  display: flex;
  flex-wrap: wrap;
  gap: 0.4rem;
  margin-top: 0.75rem;
}

.tag {
  background-color: transparent;
  border: 1px solid #ff9933;
  border-radius: 999px;
  color: #ff9933;
  padding: 0.2rem 0.6rem;
  font-size: 0.7rem;
  letter-spacing: 0.03em;
}
</style>
