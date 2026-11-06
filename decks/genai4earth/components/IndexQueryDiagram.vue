<script setup lang="ts">
// Two lanes: build the index once (top), answer each question (bottom).
// Both lanes meet at the vector database in the middle.
const docs = [0, 1, 2]
const chunkRows = [70, 90, 110]
const vecRows = [72, 92, 112]
const cells = [0.9, 0.35, 0.7, 0.2, 0.55, 0.85, 0.3, 0.6]
const qCells = [0.8, 0.3, 0.65, 0.25, 0.5, 0.9, 0.35, 0.55]
const results = [
  { y: 240, n: 1 },
  { y: 264, n: 2 },
  { y: 288, n: 3 },
]
</script>

<template>
  <svg viewBox="0 0 880 360" style="width:100%;height:auto">
    <defs>
      <marker id="iqd-ah" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse">
        <path d="M0,0 L10,5 L0,10 z" fill="#888" />
      </marker>
    </defs>

    <!-- lanes -->
    <rect x="4" y="6" width="872" height="160" rx="12" fill="#2E86AB" opacity="0.06" />
    <rect x="4" y="192" width="872" height="162" rx="12" fill="#CF3F02" opacity="0.06" />
    <text x="22" y="32" style="font-size:15px" font-weight="700" fill="#2E86AB">Build once</text>
    <text x="22" y="218" style="font-size:15px" font-weight="700" fill="#CF3F02">Each question</text>

    <!-- top lane: documents -->
    <g v-for="i in docs" :key="'d' + i">
      <path :d="`M${62 + i * 12},${62 + i * 8} h26 l10,10 v34 h-36 z`" fill="white" stroke="#565B65" stroke-width="1.5" />
      <path :d="`M${88 + i * 12},${62 + i * 8} v10 h10`" fill="none" stroke="#565B65" stroke-width="1.5" />
    </g>
    <line v-for="k in 3" :key="'dl' + k" x1="92" :x2="116" :y1="88 + k * 7" :y2="88 + k * 7" stroke="#bbb" stroke-width="2" />
    <text x="90" y="150" text-anchor="middle" style="font-size:13px" fill="#443f3f">documents</text>

    <line x1="135" y1="95" x2="200" y2="95" stroke="#888" stroke-width="2" marker-end="url(#iqd-ah)" />
    <text x="167" y="86" text-anchor="middle" style="font-size:11px" fill="#565B65">split</text>

    <!-- top lane: chunks -->
    <rect v-for="y in chunkRows" :key="'c' + y" x="210" :y="y" width="62" height="14" rx="3" fill="white" stroke="#565B65" stroke-width="1.5" />
    <line v-for="y in chunkRows" :key="'cl' + y" x1="216" x2="260" :y1="y + 7" :y2="y + 7" stroke="#bbb" stroke-width="2" />
    <text x="241" y="150" text-anchor="middle" style="font-size:13px" fill="#443f3f">chunks</text>

    <line x1="285" y1="95" x2="355" y2="95" stroke="#888" stroke-width="2" marker-end="url(#iqd-ah)" />
    <text x="320" y="86" text-anchor="middle" style="font-size:11px" fill="#565B65">embed</text>

    <!-- top lane: vectors -->
    <g v-for="(y, r) in vecRows" :key="'v' + y">
      <rect v-for="(o, j) in cells" :key="j" :x="366 + j * 9" :y="y" width="8" height="12" :fill="'#2E86AB'" :opacity="((o + r * 0.3) % 1) * 0.8 + 0.2" />
    </g>
    <text x="402" y="150" text-anchor="middle" style="font-size:13px" fill="#443f3f">vectors</text>

    <line x1="448" y1="95" x2="552" y2="95" stroke="#888" stroke-width="2" marker-end="url(#iqd-ah)" />
    <text x="500" y="86" text-anchor="middle" style="font-size:11px" fill="#565B65">store</text>

    <text x="775" y="90" text-anchor="middle" style="font-size:12px" fill="#565B65">run again when</text>
    <text x="775" y="106" text-anchor="middle" style="font-size:12px" fill="#565B65">the data changes</text>

    <!-- vector database, shared by both lanes -->
    <path d="M560,75 v200 a50,14 0 0 0 100,0 v-200" fill="#eaf3f7" stroke="#2E86AB" stroke-width="2" />
    <ellipse cx="610" cy="75" rx="50" ry="14" fill="#d5e7ef" stroke="#2E86AB" stroke-width="2" />
    <circle v-for="(p, k) in [[585, 120], [628, 132], [600, 150], [640, 210], [582, 225], [615, 240]]" :key="'p' + k" :cx="p[0]" :cy="p[1]" r="4" fill="#2E86AB" opacity="0.6" />
    <text x="610" y="176" text-anchor="middle" style="font-size:13px" font-weight="700" fill="#443f3f">vector</text>
    <text x="610" y="192" text-anchor="middle" style="font-size:13px" font-weight="700" fill="#443f3f">database</text>

    <!-- bottom lane: question -->
    <rect x="26" y="248" width="114" height="44" rx="12" fill="white" stroke="#CF3F02" stroke-width="2" />
    <path d="M48,291 l-6,12 l16,-11" fill="white" stroke="#CF3F02" stroke-width="2" stroke-linejoin="round" />
    <rect x="44" y="289" width="16" height="4" fill="white" />
    <text x="83" y="266" text-anchor="middle" style="font-size:12px" fill="#443f3f">"burned areas</text>
    <text x="83" y="282" text-anchor="middle" style="font-size:12px" fill="#443f3f">in 2023?"</text>
    <text x="83" y="330" text-anchor="middle" style="font-size:13px" fill="#443f3f">question</text>

    <line x1="148" y1="270" x2="200" y2="270" stroke="#888" stroke-width="2" marker-end="url(#iqd-ah)" />
    <text x="174" y="261" text-anchor="middle" style="font-size:11px" fill="#565B65">embed</text>

    <!-- bottom lane: query vector -->
    <rect v-for="(o, j) in qCells" :key="'q' + j" :x="210 + j * 9" y="264" width="8" height="12" fill="#CF3F02" :opacity="o * 0.8 + 0.2" />
    <text x="246" y="330" text-anchor="middle" style="font-size:13px" fill="#443f3f">query vector</text>

    <line x1="294" y1="270" x2="552" y2="270" stroke="#888" stroke-width="2" marker-end="url(#iqd-ah)" />
    <text x="423" y="261" text-anchor="middle" style="font-size:11px" fill="#565B65">find the nearest vectors</text>

    <!-- bottom lane: top 3 chunks -->
    <line x1="668" y1="270" x2="712" y2="270" stroke="#888" stroke-width="2" marker-end="url(#iqd-ah)" />
    <g v-for="r in results" :key="'r' + r.n">
      <rect x="722" :y="r.y" width="136" height="18" rx="3" fill="white" stroke="#CF3F02" stroke-width="1.5" />
      <circle cx="733" :cy="r.y + 9" r="7" fill="#CF3F02" />
      <text x="733" :y="r.y + 13" text-anchor="middle" style="font-size:11px" font-weight="700" fill="white">{{ r.n }}</text>
      <line x1="748" x2="846" :y1="r.y + 9" :y2="r.y + 9" stroke="#bbb" stroke-width="2" />
    </g>
    <text x="790" y="330" text-anchor="middle" style="font-size:13px" fill="#443f3f">top 3 chunks</text>
  </svg>
</template>
