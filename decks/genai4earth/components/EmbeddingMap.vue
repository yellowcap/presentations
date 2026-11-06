<script setup lang="ts">
// A made-up 2D map of embedding space: close points have close meaning.
const pts = [
  { x: 95, y: 70, l: 'ocean surface height', g: 0 },
  { x: 140, y: 105, l: 'sea level rise', g: 0, q: true },
  { x: 125, y: 155, l: 'tide gauge records', g: 0 },
  { x: 330, y: 65, l: 'wildfire burned area', g: 1 },
  { x: 375, y: 110, l: 'forest fire map', g: 1 },
  { x: 300, y: 115, l: 'fire radiative power', g: 1 },
  { x: 120, y: 245, l: 'crop yield', g: 2 },
  { x: 170, y: 280, l: 'wheat phenology', g: 2 },
  { x: 80, y: 290, l: 'soil moisture', g: 2 },
  { x: 340, y: 255, l: 'seismic waveforms', g: 3 },
  { x: 395, y: 285, l: 'earthquake catalogue', g: 3 },
  { x: 290, y: 300, l: 'fibre optic DAS', g: 3 },
]
const col = ['info', 'danger', 'success', 'warning']
</script>

<template>
  <svg viewBox="0 0 460 340" class="g-map">
    <rect x="1" y="1" width="458" height="338" rx="12" fill="none" stroke="#bbb" stroke-width="2" />
    <circle cx="125" cy="100" r="95" fill="var(--slidev-theme-info)" opacity="0.1" />
    <circle cx="125" cy="100" r="95" fill="none" stroke="var(--slidev-theme-primary)" stroke-width="2.5" stroke-dasharray="7 5" />
    <g v-for="p in pts" :key="p.l">
      <circle :cx="p.x" :cy="p.y" :r="p.q ? 9 : 6" :fill="p.q ? 'var(--slidev-theme-primary)' : `var(--slidev-theme-${col[p.g]})`" />
      <text :x="p.x" :y="p.y + 21" text-anchor="middle" style="font-size:11px" :font-weight="p.q ? 700 : 400" fill="#443f3f">{{ p.l }}</text>
    </g>
    <text x="230" y="332" text-anchor="middle" style="font-size:10px" fill="#888">meaning space (simplified to 2D)</text>
  </svg>
</template>
