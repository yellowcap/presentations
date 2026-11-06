<script setup lang="ts">
// Row of numbered steps joined by arrows. items: [{ t: 'Title', s: 'sub text', c: 'primary' }]
defineProps<{ items: { t: string; s?: string; c?: string }[] }>()
</script>

<template>
  <div class="g-flow">
    <template v-for="(it, i) in items" :key="i">
      <div v-if="i > 0" class="g-arrow">→</div>
      <div class="g-step" :style="{ background: `var(--slidev-theme-${it.c || 'primary'})` }">
        <div class="g-n">{{ i + 1 }}</div>
        <div class="g-t">{{ it.t }}</div>
        <div v-if="it.s" class="g-s">{{ it.s }}</div>
      </div>
    </template>
  </div>
</template>
