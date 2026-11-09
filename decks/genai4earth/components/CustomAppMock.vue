<script setup lang="ts">
// Way 1: a custom web app with its own agent. The app owns UI and workflow.
const chips = [
  { x: 35, w: 92, t: 'Area: Munich', on: true },
  { x: 133, w: 110, t: 'Time: 2021–2024', on: true },
  { x: 249, w: 104, t: 'Sentinel-2 L2A', on: true },
  { x: 359, w: 86, t: 'WorldCover', on: true },
  { x: 451, w: 88, t: 'Urban Atlas', on: false },
]
const results = [
  { y: 112, t: 'Sentinel-2 L2A', s: 'Jun 2024 · 10 m · 3% cloud', c: '#CF3F02', sel: true },
  { y: 168, t: 'Sentinel-2 L2A', s: 'Jul 2024 · 10 m · 5% cloud', c: '#2E86AB' },
  { y: 224, t: 'ESA WorldCover', s: '2021 · 10 m · land cover', c: '#4DA167' },
]
const side = [
  { y: 30, t: 'Custom app', s: 'own UI', fill: '#2E86AB', fg: 'white', sub: 'white' },
  { y: 122, t: 'Own agent', s: 'custom workflow', fill: 'white', fg: '#443f3f', sub: '#565B65', stroke: '#2E86AB' },
  { y: 214, t: 'Search tools', s: 'core', fill: '#CF3F02', fg: 'white', sub: 'white' },
]
</script>

<template>
  <svg viewBox="0 0 880 360" class="g-map">
    <defs>
      <marker id="ca-arr" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse">
        <path d="M0,0 L10,5 L0,10 z" fill="#888" />
      </marker>
    </defs>

    <!-- App window -->
    <rect x="20" y="10" width="580" height="340" rx="12" fill="white" stroke="#bbb" stroke-width="2" />
    <path d="M20,40 V22 a12,12 0 0 1 12,-12 H588 a12,12 0 0 1 12,12 V40 Z" fill="#2E86AB" />
    <circle cx="40" cy="25" r="8" fill="white" />
    <path d="M34,25 a6,6 0 0 1 12,0" fill="#4DA167" />
    <text x="56" y="30" style="font-size:14px" font-weight="700" fill="white">GenAI4Earth data finder</text>
    <text x="586" y="30" text-anchor="end" style="font-size:12px" fill="white">urban pilot</text>

    <!-- Filter chips -->
    <g v-for="c in chips" :key="c.t">
      <rect :x="c.x" y="50" :width="c.w" height="24" rx="12" :fill="c.on ? '#e3f0f5' : 'white'" :stroke="c.on ? '#2E86AB' : '#bbb'" />
      <text :x="c.x + c.w / 2" y="66" text-anchor="middle" style="font-size:12px" :fill="c.on ? '#2E86AB' : '#888'">{{ c.t }}</text>
    </g>

    <!-- Map panel -->
    <rect x="35" y="86" width="240" height="204" rx="6" fill="#eef3f6" stroke="#bbb" />
    <path d="M35,276 C80,250 110,215 150,190 C185,168 220,140 275,110" fill="none" stroke="#2E86AB" stroke-width="3" opacity="0.5" />
    <rect x="55" y="100" width="150" height="150" fill="#4DA167" fill-opacity="0.15" stroke="#4DA167" stroke-width="1.5" />
    <rect x="95" y="128" width="160" height="140" fill="#2E86AB" fill-opacity="0.12" stroke="#2E86AB" stroke-width="1.5" />
    <rect x="80" y="118" width="160" height="140" fill="#CF3F02" fill-opacity="0.12" stroke="#CF3F02" stroke-width="2.5" />
    <rect x="125" y="160" width="62" height="54" fill="none" stroke="#443f3f" stroke-width="1.5" stroke-dasharray="4 3" />
    <circle cx="156" cy="187" r="5" fill="#443f3f" />
    <text x="156" y="230" text-anchor="middle" style="font-size:12px" font-weight="700" fill="#443f3f">Munich</text>
    <rect x="41" y="268" width="62" height="16" rx="3" fill="white" opacity="0.85" />
    <text x="47" y="280" style="font-size:11px" fill="#565B65">footprints</text>

    <!-- Result list -->
    <text x="292" y="102" style="font-size:12px" font-weight="700" fill="#565B65">3 datasets</text>
    <g v-for="r in results" :key="r.s">
      <rect x="290" :y="r.y" width="295" height="48" rx="6" :fill="r.sel ? '#fdeee6' : 'white'" :stroke="r.sel ? '#CF3F02' : '#ddd'" />
      <rect x="300" :y="r.y + 10" width="12" height="12" :fill="r.c" />
      <text x="320" :y="r.y + 21" style="font-size:13px" font-weight="700" fill="#443f3f">{{ r.t }}</text>
      <text x="320" :y="r.y + 38" style="font-size:11px" fill="#565B65">{{ r.s }}</text>
      <rect x="527" :y="r.y + 12" width="48" height="24" rx="5" :fill="r.sel ? '#CF3F02' : 'white'" stroke="#CF3F02" />
      <text x="551" :y="r.y + 28" text-anchor="middle" style="font-size:12px" :fill="r.sel ? 'white' : '#CF3F02'">Add</text>
    </g>

    <!-- Ask box -->
    <rect x="35" y="304" width="550" height="32" rx="16" fill="white" stroke="#2E86AB" stroke-width="1.5" />
    <text x="52" y="325" style="font-size:13px" fill="#888">Ask: only summer scenes, less than 5% cloud</text>
    <rect x="525" y="308" width="56" height="24" rx="12" fill="#2E86AB" />
    <text x="553" y="324" text-anchor="middle" style="font-size:12px" fill="white">Ask</text>

    <!-- Side panel: which wrapper -->
    <g v-for="(b, i) in side" :key="b.t">
      <rect x="650" :y="b.y" width="200" height="52" rx="10" :fill="b.fill" :stroke="b.stroke || 'none'" stroke-width="2" />
      <text x="750" :y="b.y + 23" text-anchor="middle" style="font-size:15px" font-weight="700" :fill="b.fg">{{ b.t }}</text>
      <text x="750" :y="b.y + 41" text-anchor="middle" style="font-size:12px" :fill="b.sub">{{ b.s }}</text>
      <line v-if="i < 2" x1="750" :y1="b.y + 54" x2="750" :y2="b.y + 88" stroke="#888" stroke-width="2" marker-end="url(#ca-arr)" />
    </g>
    <text x="760" y="108" style="font-size:11px" fill="#565B65">asks</text>
    <text x="760" y="200" style="font-size:11px" fill="#565B65">calls</text>

    <text x="750" y="306" text-anchor="middle" style="font-size:14px" font-weight="700" fill="#CF3F02">The app owns</text>
    <text x="750" y="326" text-anchor="middle" style="font-size:14px" font-weight="700" fill="#CF3F02">UI and workflow.</text>
  </svg>
</template>
