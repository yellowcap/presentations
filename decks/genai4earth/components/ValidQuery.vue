<script setup lang="ts">
// Free text becomes a valid STAC query: each part is picked from a list of valid options.
const lists = [
  { h: 'collection', o: ['Sentinel-2', 'Landsat 8', 'MODIS'], pick: 0 },
  { h: 'place', o: ['Amazon river', 'Amazon forest', 'Amazonas'], pick: 1 },
  { h: 'dates', o: ['Jun–Aug 2024', 'Jan–Dec 2024', 'Jun–Aug 2023'], pick: 0 },
].map((l, i) => ({ ...l, x: 196 + i * 124 }))
const json = [
  '{',
  '"collections": ["sentinel-2-l2a"],',
  '"intersects": "@place:102",',
  '"datetime": "2024-06-01/2024-08-31"',
  '}',
]
const chips = [
  { l: 'river', id: 'id 101', x: 232 },
  { l: 'forest', id: 'id 102', x: 442, pick: true },
  { l: 'company', id: 'id 103', x: 652 },
]
</script>

<template>
  <svg viewBox="0 0 880 360" style="width:100%;height:auto">
    <defs>
      <marker id="vq-arrow" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto">
        <path d="M0,0 L10,5 L0,10 z" fill="#888" />
      </marker>
    </defs>

    <!-- free text -->
    <text x="85" y="30" text-anchor="middle" style="font-size:13px" font-weight="700" fill="#443f3f">question</text>
    <path d="M 22 70 H 148 Q 160 70 160 82 V 158 Q 160 170 148 170 H 50 L 32 186 L 36 170 H 22 Q 10 170 10 158 V 82 Q 10 70 22 70 Z" fill="#f3f3f3" stroke="#bbb" stroke-width="1.5" />
    <text x="85" y="104" text-anchor="middle" style="font-size:14px" fill="#443f3f">Sentinel-2 over</text>
    <text x="85" y="124" text-anchor="middle" style="font-size:14px" fill="#443f3f">the Amazon,</text>
    <text x="85" y="144" text-anchor="middle" style="font-size:14px" fill="#443f3f">summer 2024</text>
    <line x1="164" y1="120" x2="194" y2="120" stroke="#888" stroke-width="2.5" marker-end="url(#vq-arrow)" />

    <!-- option lists -->
    <g v-for="l in lists" :key="l.h">
      <text :x="l.x + 60" y="30" text-anchor="middle" style="font-size:13px" font-weight="700" fill="#443f3f">{{ l.h }}</text>
      <rect :x="l.x" y="40" width="120" height="160" rx="8" fill="white" stroke="#bbb" stroke-width="1.5" />
      <g v-for="(o, j) in l.o" :key="o">
        <rect v-if="j === l.pick" :x="l.x + 4" :y="48 + j * 50" width="112" height="44" rx="6" fill="#4DA167" fill-opacity="0.15" />
        <path v-if="j === l.pick" :d="`M ${l.x + 9} ${71 + j * 50} l 4 4 l 8 -10`" fill="none" stroke="#4DA167" stroke-width="3" stroke-linecap="round" stroke-linejoin="round" />
        <text :x="l.x + 26" :y="74 + j * 50" style="font-size:12px" :fill="j === l.pick ? '#443f3f' : '#888'">{{ o }}</text>
      </g>
    </g>
    <text x="380" y="222" text-anchor="middle" style="font-size:13px" fill="#565B65">pick from valid options only</text>
    <line x1="568" y1="120" x2="596" y2="120" stroke="#888" stroke-width="2.5" marker-end="url(#vq-arrow)" />

    <!-- query -->
    <text x="600" y="30" style="font-size:13px" font-weight="700" fill="#443f3f">valid STAC query</text>
    <rect x="600" y="40" width="270" height="160" rx="8" fill="#443f3f" />
    <text v-for="(t, i) in json" :key="i" :x="i === 0 || i === 4 ? 612 : 624" :y="78 + i * 23" style="font-size:11px;font-family:'Roboto Mono',monospace" fill="white">{{ t }}</text>

    <!-- geocoding -->
    <line x1="10" y1="242" x2="870" y2="242" stroke="#ddd" stroke-width="1.5" />
    <rect x="20" y="268" width="130" height="54" rx="10" fill="white" stroke="#443f3f" stroke-width="2" />
    <text x="85" y="301" text-anchor="middle" style="font-size:18px" font-weight="700" fill="#443f3f">"Amazon"</text>
    <text x="185" y="285" text-anchor="middle" style="font-size:11px" fill="#565B65">geocode</text>
    <line x1="156" y1="295" x2="212" y2="295" stroke="#888" stroke-width="2.5" marker-end="url(#vq-arrow)" />
    <rect x="216" y="258" width="654" height="74" rx="10" fill="#f3f3f3" />
    <g v-for="c in chips" :key="c.l">
      <rect :x="c.x" y="272" width="190" height="46" rx="23" fill="white" :stroke="c.pick ? '#4DA167' : '#bbb'" :stroke-width="c.pick ? 3 : 1.5" />
      <path v-if="c.l === 'river'" :d="`M ${c.x + 16} 290 q 6 -6 12 0 t 12 0 M ${c.x + 16} 300 q 6 -6 12 0 t 12 0`" fill="none" stroke="#2E86AB" stroke-width="2.5" />
      <g v-if="c.l === 'forest'">
        <polygon :points="`${c.x + 28},282 ${c.x + 17},302 ${c.x + 39},302`" fill="#4DA167" />
        <rect :x="c.x + 26" y="302" width="4" height="7" fill="#565B65" />
      </g>
      <g v-if="c.l === 'company'">
        <rect :x="c.x + 18" y="283" width="20" height="26" fill="#565B65" />
        <rect v-for="k in 3" :key="k" :x="c.x + 22" :y="282 + k * 7" width="12" height="3" fill="white" />
      </g>
      <text :x="c.x + 52" y="300" style="font-size:15px" font-weight="700" fill="#443f3f">{{ c.l }}</text>
      <text :x="c.x + 176" y="300" text-anchor="end" style="font-size:12px;font-family:'Roboto Mono',monospace" fill="#565B65">{{ c.id }}</text>
    </g>
    <text x="543" y="352" text-anchor="middle" style="font-size:12px" fill="#565B65">The geocoder returns candidates with ids. The agent picks one.</text>
  </svg>
</template>
