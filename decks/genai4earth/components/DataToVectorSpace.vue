<script setup lang="ts">
// Four very different sources go into one shared vector space.
// Dots group by meaning, not by source, so the colours mix.
const src = [
  { y: 50, l: 'satellite images', c: '#2E86AB' },
  { y: 130, l: 'phenotype tables', c: '#4DA167' },
  { y: 210, l: 'seismic waveforms', c: '#A71D31' },
  { y: 290, l: 'reports', c: '#E2C044' },
]
// Four topic clusters, each with dots from mixed sources (fixed pseudo-random layout).
const centres = [[420, 130], [528, 150], [430, 250], [535, 260]]
let seed = 7
const rnd = () => { seed = (seed * 9301 + 49297) % 233280; return seed / 233280 }
const dots: { x: number, y: number, c: string }[] = []
centres.forEach(([cx, cy], k) => {
  for (let i = 0; i < 9; i++) {
    const a = rnd() * Math.PI * 2
    const r = 8 + rnd() * 30
    dots.push({ x: cx + Math.cos(a) * r, y: cy + Math.sin(a) * r * 0.8, c: src[(i + k) % 4].c })
  }
})
</script>

<template>
  <svg viewBox="0 0 880 360" style="width:100%;height:auto">
    <defs>
      <marker id="dvs-ah" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse">
        <path d="M0,0 L10,5 L0,10 z" fill="#888" />
      </marker>
    </defs>

    <!-- sources -->
    <g v-for="(s, i) in src" :key="s.l">
      <rect x="20" :y="s.y - 22" width="44" height="44" rx="6" :fill="s.c" fill-opacity="0.15" :stroke="s.c" stroke-width="2" />
      <text x="76" :y="s.y + 5" style="font-size:13px" fill="#443f3f">{{ s.l }}</text>
      <path :d="`M206,${s.y} C270,${s.y} 280,180 330,180`" fill="none" :stroke="s.c" stroke-width="2" marker-end="url(#dvs-ah)" />
    </g>

    <!-- icon: satellite image (raster tiles) -->
    <rect v-for="(o, k) in [0.9, 0.5, 0.7, 0.4, 0.8, 0.3, 0.6, 0.9, 0.5]" :key="'t' + k"
      :x="28 + (k % 3) * 10" :y="36 + Math.floor(k / 3) * 10" width="9" height="9" :fill="k % 2 ? '#4DA167' : '#2E86AB'" :opacity="o" />
    <!-- icon: table -->
    <rect x="28" y="116" width="28" height="28" fill="white" stroke="#4DA167" stroke-width="1.5" />
    <line x1="28" y1="123" x2="56" y2="123" stroke="#4DA167" stroke-width="3" />
    <line x1="28" y1="130" x2="56" y2="130" stroke="#4DA167" stroke-width="1" />
    <line x1="28" y1="137" x2="56" y2="137" stroke="#4DA167" stroke-width="1" />
    <line x1="37" y1="116" x2="37" y2="144" stroke="#4DA167" stroke-width="1" />
    <line x1="47" y1="116" x2="47" y2="144" stroke="#4DA167" stroke-width="1" />
    <!-- icon: seismic waveform -->
    <polyline points="26,210 31,210 34,204 37,218 40,196 43,224 46,202 49,214 52,208 55,210 58,210" fill="none" stroke="#A71D31" stroke-width="2" stroke-linejoin="round" />
    <!-- icon: report page -->
    <path d="M30,276 h17 l7,7 v21 h-24 z" fill="white" stroke="#b39a1f" stroke-width="1.5" />
    <line v-for="k in 3" :key="'r' + k" x1="34" x2="50" :y1="283 + k * 5" :y2="283 + k * 5" stroke="#b39a1f" stroke-width="1.5" />

    <!-- shared vector space -->
    <rect x="336" y="24" width="270" height="312" rx="14" fill="#f4f4f4" stroke="#bbb" stroke-width="2" />
    <text x="471" y="52" text-anchor="middle" style="font-size:15px" font-weight="700" fill="#443f3f">one shared vector space</text>
    <circle v-for="(d, k) in dots" :key="'p' + k" :cx="d.x" :cy="d.y" r="6" :fill="d.c" stroke="white" stroke-width="1" />
    <text x="471" y="322" text-anchor="middle" style="font-size:11px" fill="#565B65">close dots = close meaning, from any source</text>

    <line x1="612" y1="180" x2="664" y2="180" stroke="#888" stroke-width="2" marker-end="url(#dvs-ah)" />

    <!-- conversational discovery -->
    <rect x="672" y="90" width="196" height="180" rx="14" fill="white" stroke="#565B65" stroke-width="2" />
    <rect x="732" y="106" width="124" height="44" rx="10" fill="#eeeeee" />
    <text x="794" y="124" text-anchor="middle" style="font-size:12px" fill="#443f3f">Drought data</text>
    <text x="794" y="140" text-anchor="middle" style="font-size:12px" fill="#443f3f">for my region?</text>
    <rect x="684" y="162" width="136" height="94" rx="10" fill="#CF3F02" />
    <text x="696" y="184" style="font-size:12px" fill="white">2 image sets</text>
    <text x="696" y="202" style="font-size:12px" fill="white">1 field table</text>
    <text x="696" y="220" style="font-size:12px" fill="white">1 report</text>
    <text x="696" y="240" style="font-size:11px" fill="white" opacity="0.85">with links</text>
    <text x="770" y="296" text-anchor="middle" style="font-size:14px" font-weight="700" fill="#443f3f">conversational</text>
    <text x="770" y="314" text-anchor="middle" style="font-size:14px" font-weight="700" fill="#443f3f">discovery</text>
  </svg>
</template>
