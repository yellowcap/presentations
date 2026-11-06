<script setup lang="ts">
// Two levels of evals, and three real results (pass / total).
const x0 = 510
const w = 260 // track width = all cases
const runs = [
  { n: 'GNW gold evals', lvl: 'agent', c: '#CF3F02', p: 94, t: 109 },
  { n: 'Copernicus RAG test queries', lvl: 'search', c: '#2E86AB', p: 50, t: 50 },
  { n: 'stac-zap', lvl: 'query tool', c: '#4DA167', p: 166, t: 172 },
].map((r, i) => ({ ...r, y: 72 + i * 92 }))
const ranks = [0, 1, 2].map(i => ({ y: 250 + i * 22, w: 150 - i * 30 }))
</script>

<template>
  <svg viewBox="0 0 880 360" style="width:100%;height:auto">
    <!-- agent level -->
    <rect x="20" y="20" width="450" height="320" rx="14" fill="#CF3F02" fill-opacity="0.05" stroke="#CF3F02" stroke-width="2.5" />
    <text x="44" y="56" style="font-size:22px" font-weight="700" fill="#CF3F02">Agent</text>
    <text x="44" y="90" style="font-size:16px" fill="#443f3f">Right answer?</text>
    <text x="44" y="116" style="font-size:16px" fill="#443f3f">Right tool at the right time?</text>

    <!-- search level -->
    <rect x="44" y="140" width="402" height="176" rx="12" fill="#2E86AB" fill-opacity="0.08" stroke="#2E86AB" stroke-width="2.5" />
    <text x="66" y="176" style="font-size:20px" font-weight="700" fill="#2E86AB">Search</text>
    <text x="66" y="206" style="font-size:16px" fill="#443f3f">Right dataset in the top k?</text>
    <rect v-for="(r, i) in ranks" :key="i" x="66" :y="r.y" :width="r.w" height="12" rx="6" :fill="i === 1 ? '#4DA167' : '#bbb'" />
    <path d="M 226 279 l 6 6 l 11 -13" fill="none" stroke="#4DA167" stroke-width="3" stroke-linecap="round" stroke-linejoin="round" />
    <path d="M 262 248 h 8 v 62 h -8" fill="none" stroke="#565B65" stroke-width="1.5" />
    <text x="280" y="286" style="font-size:13px" fill="#565B65">top k</text>

    <!-- real results -->
    <text :x="x0" y="34" style="font-size:16px" font-weight="700" fill="#443f3f">Our runs (pass / total)</text>
    <g v-for="r in runs" :key="r.n">
      <text :x="x0" :y="r.y" style="font-size:14px" font-weight="700" fill="#443f3f">{{ r.n }}</text>
      <text :x="x0" :y="r.y + 18" style="font-size:12px" :fill="r.c">level: {{ r.lvl }}</text>
      <rect :x="x0" :y="r.y + 28" :width="w" height="24" rx="4" fill="#e6e6e6" />
      <rect :x="x0" :y="r.y + 28" :width="w * r.p / r.t" height="24" rx="4" :fill="r.c" />
      <text :x="x0 + w + 10" :y="r.y + 46" style="font-size:16px" font-weight="700" :fill="r.c">{{ r.p }}/{{ r.t }}</text>
    </g>
  </svg>
</template>
