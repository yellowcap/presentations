<script setup lang="ts">
// Filter on metadata first (place, time), then rank the few that pass by similarity.
const out = [
  { x: 35, y: 55 }, { x: 262, y: 58 }, { x: 40, y: 190 }, { x: 268, y: 196 }, { x: 196, y: 204 },
]
const pass = [
  { x: 135, y: 95, l: 'A' }, { x: 190, y: 122, l: 'B' }, { x: 140, y: 152, l: 'C' },
]
const ranked = [
  { l: 'B', s: 230 }, { l: 'A', s: 175 }, { l: 'C', s: 120 },
].map((r, i) => ({ ...r, y: 50 + i * 86, n: i + 1 }))
</script>

<template>
  <svg viewBox="0 0 880 360" style="width:100%;height:auto">
    <!-- place -->
    <text x="20" y="28" style="font-size:15px" font-weight="700" fill="#443f3f">place (area of interest)</text>
    <rect x="20" y="40" width="300" height="200" rx="8" fill="#f3f3f3" stroke="#bbb" stroke-width="1.5" />
    <path d="M 20 150 C 70 130 90 175 150 160 S 240 120 320 140" fill="none" stroke="#bbb" stroke-width="1.5" />
    <rect v-for="(f, i) in out" :key="'o' + i" :x="f.x" :y="f.y" width="40" height="28" rx="3" fill="none" stroke="#999" stroke-width="1.5" />
    <rect x="205" y="86" width="40" height="28" rx="3" fill="none" stroke="#999" stroke-width="1.5" stroke-dasharray="4 3" />
    <rect x="120" y="80" width="135" height="112" rx="4" fill="#CF3F02" fill-opacity="0.07" stroke="#CF3F02" stroke-width="2.5" stroke-dasharray="7 5" />
    <g v-for="f in pass" :key="f.l">
      <rect :x="f.x" :y="f.y" width="40" height="28" rx="3" fill="#2E86AB" />
      <text :x="f.x + 20" :y="f.y + 19" text-anchor="middle" style="font-size:13px" font-weight="700" fill="white">{{ f.l }}</text>
    </g>

    <!-- time -->
    <text x="20" y="274" style="font-size:15px" font-weight="700" fill="#443f3f">time span</text>
    <rect x="20" y="286" width="300" height="10" rx="5" fill="#ddd" />
    <rect x="150" y="282" width="110" height="18" rx="4" fill="#CF3F02" />
    <text x="20" y="318" style="font-size:12px" fill="#565B65">older</text>
    <text x="320" y="318" text-anchor="end" style="font-size:12px" fill="#565B65">newer</text>

    <!-- filter arrow -->
    <text x="405" y="150" text-anchor="middle" style="font-size:18px" font-weight="700" fill="#CF3F02">filter</text>
    <line x1="350" y1="165" x2="448" y2="165" stroke="#CF3F02" stroke-width="3" />
    <polygon points="462,165 446,156 446,174" fill="#CF3F02" />
    <text x="405" y="190" text-anchor="middle" style="font-size:12px" fill="#565B65">place + time</text>

    <!-- rank -->
    <text x="490" y="28" style="font-size:15px" font-weight="700" fill="#443f3f">rank by similarity</text>
    <g v-for="r in ranked" :key="r.l">
      <rect x="490" :y="r.y" width="370" height="68" rx="8" fill="#f3f3f3" />
      <circle cx="522" :cy="r.y + 34" r="17" fill="#CF3F02" />
      <text x="522" :y="r.y + 40" text-anchor="middle" style="font-size:16px" font-weight="700" fill="white">{{ r.n }}</text>
      <rect x="552" :y="r.y + 20" width="40" height="28" rx="3" fill="#2E86AB" />
      <text x="572" :y="r.y + 39" text-anchor="middle" style="font-size:13px" font-weight="700" fill="white">{{ r.l }}</text>
      <rect x="612" :y="r.y + 27" width="230" height="14" rx="7" fill="#e2e2e2" />
      <rect x="612" :y="r.y + 27" :width="r.s" height="14" rx="7" fill="#CF3F02" />
    </g>
    <text x="612" y="72" style="font-size:11px" fill="#565B65">similarity</text>
    <text x="675" y="330" text-anchor="middle" style="font-size:13px" fill="#565B65">Only the few that pass get a score.</text>
  </svg>
</template>
