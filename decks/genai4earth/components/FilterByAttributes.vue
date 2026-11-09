<script setup lang="ts">
// Filter by attributes (area, time, collection, cloud): values come from valid options,
// the metadata stored next to each vector cuts the candidates, similarity ranks what passes.
const rows = [
  { k: 'area', y: 32, o: [{ l: 'Munich, city', id: 'id 201', pick: true }, { l: 'Munich, region', id: 'id 202' }] },
  { k: 'time', y: 86 },
  { k: 'collection', y: 140, o: [{ l: 'sentinel-2-l2a', pick: true }, { l: 'landsat-c2-l2' }] },
  { k: 'cloud', y: 194 },
]
const out = [
  { x: 453, y: 42 }, { x: 598, y: 44 }, { x: 452, y: 160 }, { x: 600, y: 162 },
]
const pass = [
  { x: 508, y: 82, l: 'A' }, { x: 550, y: 94, l: 'B' }, { x: 516, y: 128, l: 'C' },
]
const ranked = [
  { l: 'B', s: 88 }, { l: 'C', s: 66 }, { l: 'A', s: 44 },
].map((r, i) => ({ ...r, y: 38 + i * 68, n: i + 1 }))
const json = [
  '{ "collections": ["sentinel-2-l2a"], "intersects": "@place:201",',
  '  "datetime": "2024-06-01/2024-08-31", "query": {"eo:cloud_cover": {"lt": 20}} }',
]
</script>

<template>
  <svg viewBox="0 0 880 360" style="width:100%;height:auto">
    <defs>
      <marker id="fba-arrow" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="6" markerHeight="6" orient="auto">
        <path d="M0,0 L10,5 L0,10 z" fill="#888" />
      </marker>
    </defs>

    <!-- question -->
    <text x="70" y="22" text-anchor="middle" style="font-size:13px" font-weight="700" fill="#443f3f">question</text>
    <path d="M 20 52 H 120 Q 132 52 132 64 V 168 Q 132 180 120 180 H 46 L 28 196 L 32 180 H 20 Q 8 180 8 168 V 64 Q 8 52 20 52 Z" fill="#f3f3f3" stroke="#bbb" stroke-width="1.5" />
    <text x="70" y="88" text-anchor="middle" style="font-size:14px" fill="#443f3f">Sentinel-2</text>
    <text x="70" y="110" text-anchor="middle" style="font-size:14px" fill="#443f3f">over Munich,</text>
    <text x="70" y="132" text-anchor="middle" style="font-size:14px" fill="#443f3f">summer 2024,</text>
    <text x="70" y="154" text-anchor="middle" style="font-size:14px" fill="#443f3f">low cloud</text>
    <line x1="134" y1="130" x2="150" y2="130" stroke="#888" stroke-width="2.5" marker-end="url(#fba-arrow)" />

    <!-- filters: pick from valid options -->
    <text x="285" y="22" text-anchor="middle" style="font-size:13px" font-weight="700" fill="#443f3f">filters, from valid options</text>
    <g v-for="r in rows" :key="r.k">
      <rect x="154" :y="r.y" width="264" height="48" rx="8" fill="white" stroke="#bbb" stroke-width="1.5" />
      <!-- icons -->
      <g v-if="r.k === 'area'">
        <path :d="`M 180 ${r.y + 30} C 170 ${r.y + 19} 170 ${r.y + 7} 180 ${r.y + 7} C 190 ${r.y + 7} 190 ${r.y + 19} 180 ${r.y + 30} Z`" fill="#CF3F02" />
        <circle cx="180" :cy="r.y + 15" r="3.5" fill="white" />
      </g>
      <g v-if="r.k === 'time'">
        <rect x="170" :y="r.y + 9" width="20" height="19" rx="2" fill="none" stroke="#CF3F02" stroke-width="2.5" />
        <line x1="170" :y1="r.y + 15" x2="190" :y2="r.y + 15" stroke="#CF3F02" stroke-width="2.5" />
      </g>
      <g v-if="r.k === 'collection'">
        <path :d="`M 180 ${r.y + 7} L 192 ${r.y + 13} L 180 ${r.y + 19} L 168 ${r.y + 13} Z`" fill="#CF3F02" />
        <path :d="`M 168 ${r.y + 19} L 180 ${r.y + 25} L 192 ${r.y + 19}`" fill="none" stroke="#CF3F02" stroke-width="2.5" />
      </g>
      <g v-if="r.k === 'cloud'">
        <path :d="`M 171 ${r.y + 27} a 6 6 0 0 1 1 -11 a 8 8 0 0 1 15 -1 a 6 6 0 0 1 2 12 Z`" fill="#CF3F02" />
      </g>
      <text x="180" :y="r.y + 42" text-anchor="middle" style="font-size:11px" fill="#565B65">{{ r.k }}</text>

      <!-- option chips -->
      <g v-for="(o, j) in r.o" :key="o.l">
        <rect x="204" :y="r.y + 3 + j * 22" width="208" height="20" rx="10" :fill="o.pick ? '#4DA167' : 'white'" :fill-opacity="o.pick ? 0.15 : 1" :stroke="o.pick ? '#4DA167' : '#ccc'" :stroke-width="o.pick ? 2 : 1" />
        <path v-if="o.pick" :d="`M 211 ${r.y + 13 + j * 22} l 3 3 l 6 -7`" fill="none" stroke="#4DA167" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round" />
        <text x="226" :y="r.y + 17 + j * 22" style="font-size:12px" :font-weight="o.pick ? 700 : 400" :fill="o.pick ? '#443f3f' : '#888'">{{ o.l }}</text>
        <text v-if="o.id" x="404" :y="r.y + 17 + j * 22" text-anchor="end" style="font-size:11px;font-family:'Roboto Mono',monospace" :fill="o.pick ? '#565B65' : '#999'">{{ o.id }}</text>
      </g>
    </g>
    <!-- time: timeline with the selected range -->
    <rect x="208" y="104" width="200" height="8" rx="4" fill="#e2e2e2" />
    <rect x="301" y="100" width="25" height="16" rx="3" fill="#4DA167" />
    <text x="313" y="96" text-anchor="middle" style="font-size:12px" font-weight="700" fill="#443f3f">Jun–Aug 2024</text>
    <text x="208" y="129" style="font-size:11px" fill="#888">2023</text>
    <text x="408" y="129" text-anchor="end" style="font-size:11px" fill="#888">2025</text>
    <!-- cloud: 0 to 100 % with the selected part -->
    <rect x="208" y="212" width="200" height="8" rx="4" fill="#e2e2e2" />
    <rect x="208" y="208" width="40" height="16" rx="3" fill="#4DA167" />
    <text x="256" y="221" style="font-size:12px" font-weight="700" fill="#443f3f">under 20 %</text>
    <text x="208" y="237" style="font-size:11px" fill="#888">0 %</text>
    <text x="408" y="237" text-anchor="end" style="font-size:11px" fill="#888">100 %</text>

    <line x1="420" y1="130" x2="442" y2="130" stroke="#888" stroke-width="2.5" marker-end="url(#fba-arrow)" />

    <!-- metadata filter on the map -->
    <text x="545" y="22" text-anchor="middle" style="font-size:13px" font-weight="700" fill="#443f3f">metadata cuts down</text>
    <rect x="446" y="32" width="198" height="166" rx="8" fill="#f3f3f3" stroke="#bbb" stroke-width="1.5" />
    <path d="M 498 70 L 560 60 L 600 88 L 594 150 L 540 170 L 496 152 L 486 108 Z" fill="#CF3F02" fill-opacity="0.07" stroke="#CF3F02" stroke-width="2.5" stroke-dasharray="7 5" />
    <rect v-for="(f, i) in out" :key="'o' + i" :x="f.x" :y="f.y" width="34" height="24" rx="3" fill="none" stroke="#999" stroke-width="1.5" />
    <rect x="558" y="132" width="34" height="24" rx="3" fill="none" stroke="#999" stroke-width="1.5" stroke-dasharray="4 3" />
    <g v-for="f in pass" :key="f.l">
      <rect :x="f.x" :y="f.y" width="34" height="24" rx="3" fill="#2E86AB" />
      <text :x="f.x + 17" :y="f.y + 17" text-anchor="middle" style="font-size:13px" font-weight="700" fill="white">{{ f.l }}</text>
    </g>
    <text x="545" y="216" text-anchor="middle" style="font-size:11px" fill="#565B65">stored next to each vector:</text>
    <text x="545" y="232" text-anchor="middle" style="font-size:11px" fill="#565B65">bbox, time span, collection, cloud</text>

    <line x1="646" y1="130" x2="666" y2="130" stroke="#888" stroke-width="2.5" marker-end="url(#fba-arrow)" />

    <!-- rank what passes -->
    <text x="770" y="22" text-anchor="middle" style="font-size:13px" font-weight="700" fill="#443f3f">rank by similarity</text>
    <g v-for="r in ranked" :key="r.l">
      <rect x="670" :y="r.y - 6" width="200" height="56" rx="8" fill="#f3f3f3" />
      <circle cx="694" :cy="r.y + 22" r="14" fill="#CF3F02" />
      <text x="694" :y="r.y + 27" text-anchor="middle" style="font-size:14px" font-weight="700" fill="white">{{ r.n }}</text>
      <rect x="716" :y="r.y + 10" width="34" height="24" rx="3" fill="#2E86AB" />
      <text x="733" :y="r.y + 27" text-anchor="middle" style="font-size:13px" font-weight="700" fill="white">{{ r.l }}</text>
      <rect x="762" :y="r.y + 15" width="96" height="14" rx="7" fill="#e2e2e2" />
      <rect x="762" :y="r.y + 15" :width="r.s" height="14" rx="7" fill="#CF3F02" />
    </g>

    <!-- the valid STAC query -->
    <line x1="286" y1="244" x2="286" y2="262" stroke="#888" stroke-width="2.5" marker-end="url(#fba-arrow)" />
    <text x="12" y="290" style="font-size:13px" font-weight="700" fill="#443f3f">valid</text>
    <text x="12" y="308" style="font-size:13px" font-weight="700" fill="#443f3f">STAC query</text>
    <rect x="154" y="266" width="716" height="62" rx="8" fill="#443f3f" />
    <text v-for="(t, i) in json" :key="i" x="168" :y="292 + i * 22" style="font-size:13px;font-family:'Roboto Mono',monospace" fill="white" xml:space="preserve">{{ t }}</text>
    <text x="512" y="350" text-anchor="middle" style="font-size:12px" fill="#565B65">Each value is picked from a list of valid options. Nothing is invented.</text>
  </svg>
</template>
