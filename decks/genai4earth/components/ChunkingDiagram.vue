<script setup lang="ts">
// The same document cut into chunks in three ways.
const W = 140
const TOP = 30
const docs = [
  {
    x: 80, t: 'Whole document', pro: '+ full context', con: '– long, vague vector',
    chunks: [{ y0: 0, y1: 210, c: '#CF3F02' }],
  },
  {
    x: 370, t: 'Split at headings', pro: '+ keeps topics whole', con: '– sizes differ a lot',
    chunks: [{ y0: 0, y1: 134, c: '#2E86AB' }, { y0: 136, y1: 210, c: '#4DA167' }],
  },
  {
    x: 660, t: 'Fixed size + overlap', pro: '+ simple, even sizes', con: '– cuts mid-sentence',
    chunks: [
      { y0: 0, y1: 65, c: '#CF3F02' }, { y0: 50, y1: 115, c: '#CF3F02' },
      { y0: 100, y1: 165, c: '#CF3F02' }, { y0: 150, y1: 210, c: '#CF3F02' },
    ],
  },
]
// Document content, y offsets from the top of the page.
const heads = [{ y: 30, w: 64 }, { y: 140, w: 70 }]
const lines = [44, 54, 64, 74, 152, 162, 172, 182, 192]
</script>

<template>
  <svg viewBox="0 0 880 340" style="width:100%;height:auto">
    <g v-for="(d, n) in docs" :key="d.t">
      <!-- page -->
      <rect :x="d.x" :y="TOP" :width="W" height="210" rx="4" fill="white" stroke="#565B65" stroke-width="1.5" />
      <rect :x="d.x + 12" :y="TOP + 12" width="96" height="9" rx="2" fill="#443f3f" />
      <rect v-for="h in heads" :key="'h' + h.y" :x="d.x + 12" :y="TOP + h.y" :width="h.w" height="6" rx="2" fill="#565B65" />
      <rect v-for="y in lines" :key="'l' + y" :x="d.x + 12" :y="TOP + y" :width="y % 20 === 2 ? 90 : 116" height="4" rx="2" fill="#cccccc" />
      <!-- image in the document, with a question mark -->
      <rect :x="d.x + 12" :y="TOP + 86" width="56" height="40" rx="2" fill="#eaf3f7" stroke="#2E86AB" stroke-width="1.2" />
      <path :d="`M${d.x + 16},${TOP + 122} l14,-18 l10,12 l6,-6 l10,12 z`" fill="#2E86AB" opacity="0.6" />
      <circle :cx="d.x + 60" :cy="TOP + 94" r="4" fill="#E2C044" />
      <text :x="d.x + 90" :y="TOP + 114" text-anchor="middle" style="font-size:22px" font-weight="700" fill="#CF3F02">?</text>

      <!-- chunks drawn over the page -->
      <rect v-for="(c, k) in d.chunks" :key="'c' + k"
        :x="d.x - 8" :y="TOP + c.y0" :width="W + 16" :height="c.y1 - c.y0"
        rx="4" :fill="c.c" fill-opacity="0.16" :stroke="c.c" stroke-width="2" :stroke-dasharray="n === 2 ? '6 3' : 'none'" />

      <!-- labels -->
      <text :x="d.x + W / 2" y="270" text-anchor="middle" style="font-size:15px" font-weight="700" fill="#443f3f">{{ d.t }}</text>
      <text :x="d.x + W / 2" y="292" text-anchor="middle" style="font-size:13px" fill="#4DA167">{{ d.pro }}</text>
      <text :x="d.x + W / 2" y="312" text-anchor="middle" style="font-size:13px" fill="#A71D31">{{ d.con }}</text>
    </g>
    <text x="440" y="334" text-anchor="middle" style="font-size:11px" fill="#565B65">? = images in papers: skip, caption, or describe?</text>
  </svg>
</template>
