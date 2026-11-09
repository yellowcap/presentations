<script setup lang="ts">
// Scope spectrum: from metadata-only aggregator to data host.
const cols = [
  {
    x: 30, c: '#2E86AB', t: 'Metadata repository', s: 'aggregator',
    d1: 'Index other catalogues.', d2: 'Search, then link out.',
    a: 'Access as the provider gives it',
  },
  {
    x: 310, c: '#4DA167', t: 'Link + easier access', s: 'we do not host',
    d1: 'Point to provider assets.', d2: 'Add a layer on top.',
    a: 'Virtual layers: virtual Zarr, Kerchunk',
  },
  {
    x: 590, c: '#CF3F02', t: 'Host data', s: 'e.g. model output',
    d1: 'Store our own assets.', d2: 'We control the format.',
    a: 'Cloud native: COG, Zarr, GeoParquet',
  },
]
</script>

<template>
  <svg viewBox="0 0 880 360" class="g-map">
    <defs>
      <marker id="cs-arr" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse">
        <path d="M0,0 L10,5 L0,10 z" fill="#888" />
      </marker>
    </defs>

    <!-- spectrum arrow -->
    <line x1="40" y1="24" x2="840" y2="24" stroke="#888" stroke-width="2" marker-start="url(#cs-arr)" marker-end="url(#cs-arr)" />
    <text x="40" y="46" style="font-size:13px" font-weight="700" fill="#565B65">Aggregator</text>
    <text x="440" y="46" text-anchor="middle" style="font-size:12px" fill="#565B65">more control, more cost and responsibility →</text>
    <text x="840" y="46" text-anchor="end" style="font-size:13px" font-weight="700" fill="#565B65">Host</text>

    <g v-for="col in cols" :key="col.t">
      <rect :x="col.x" y="62" width="260" height="70" rx="10" :fill="col.c" />
      <text :x="col.x + 130" y="93" text-anchor="middle" style="font-size:18px" font-weight="700" fill="white">{{ col.t }}</text>
      <text :x="col.x + 130" y="116" text-anchor="middle" style="font-size:13px" fill="white">{{ col.s }}</text>

      <text :x="col.x + 130" y="160" text-anchor="middle" style="font-size:14px" fill="#443f3f">{{ col.d1 }}</text>
      <text :x="col.x + 130" y="180" text-anchor="middle" style="font-size:14px" fill="#443f3f">{{ col.d2 }}</text>

      <rect :x="col.x" y="200" width="260" height="50" rx="8" fill="white" :stroke="col.c" stroke-width="2" />
      <text :x="col.x + 130" y="220" text-anchor="middle" style="font-size:11px" font-weight="700" fill="#565B65">ACCESS</text>
      <text :x="col.x + 130" y="239" text-anchor="middle" style="font-size:13px" fill="#443f3f">{{ col.a }}</text>
    </g>

    <!-- STAC spans all -->
    <rect x="30" y="270" width="820" height="50" rx="10" fill="rgba(0,0,0,0.05)" stroke="#565B65" stroke-dasharray="5 4" />
    <text x="440" y="300" text-anchor="middle" style="font-size:15px" fill="#443f3f"><tspan font-weight="700">STAC</tspan> fits all three for spatial data: lots of tooling available</text>
  </svg>
</template>
