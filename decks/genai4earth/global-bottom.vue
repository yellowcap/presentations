<script setup lang="ts">
import { computed } from 'vue'
import { useNav } from '@slidev/client'

// Breadcrumb and project logo at the bottom of every content slide.
// A slide opts in with `section: 1..5` in its frontmatter.
const { currentSlideRoute } = useNav()
const parts = ['Embedding search', 'Why it matters', 'Building blocks', 'In practice', 'Summary']
const section = computed(() => Number(currentSlideRoute.value?.meta?.slide?.frontmatter?.section ?? 0))
</script>

<template>
  <div v-if="section" class="g-footer">
    <img src="/images/logos/eosc-genai4earth.png" alt="EOSC | GenAI4Earth" class="g-footer-logo" />
    <div class="g-crumb">
      <span v-for="(p, i) in parts" :key="p" :class="{ on: i + 1 === section }">{{ i + 1 === section ? `${i + 1} · ${p}` : i + 1 }}</span>
    </div>
  </div>
</template>

<style>
.g-footer { position: absolute; left: 4rem; bottom: 1rem; display: flex; align-items: center; gap: 1.2rem; z-index: 20; }
.g-footer-logo { height: 20px; width: auto; }
.g-crumb { display: flex; gap: 0.9rem; font-family: 'Roboto Mono', monospace; font-size: 0.62rem; color: #aaa; }
.g-crumb .on { color: #CF3F02; font-weight: 700; }
</style>
