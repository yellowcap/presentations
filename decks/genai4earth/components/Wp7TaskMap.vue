<script setup lang="ts">
// Where T7.2 sits in WP7: metadata in, search tools, agent, pilots. T7.4 on the side.
const pilots = [
  { y: 75, t: 'Urban' },
  { y: 157, t: 'Agro-forestry' },
  { y: 240, t: 'Seismology' },
]
</script>

<template>
  <svg viewBox="0 0 880 360" class="g-map">
    <defs>
      <marker id="wp7-arr" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse">
        <path d="M0,0 L10,5 L0,10 z" fill="#888" />
      </marker>
    </defs>

    <!-- T7.1 -->
    <rect x="20" y="145" width="170" height="70" rx="10" fill="#2E86AB" />
    <text x="105" y="168" text-anchor="middle" style="font-size:12px" fill="white" opacity="0.85">T7.1</text>
    <text x="105" y="189" text-anchor="middle" style="font-size:16px" font-weight="700" fill="white">Metadata</text>
    <text x="105" y="206" text-anchor="middle" style="font-size:12px" fill="white">better metadata in</text>

    <line x1="192" y1="180" x2="236" y2="180" stroke="#888" stroke-width="2.5" marker-end="url(#wp7-arr)" />

    <!-- T7.2 -->
    <rect x="240" y="125" width="200" height="110" rx="12" fill="#CF3F02" />
    <text x="340" y="153" text-anchor="middle" style="font-size:14px" fill="white" opacity="0.9">T7.2</text>
    <text x="340" y="185" text-anchor="middle" style="font-size:22px" font-weight="700" fill="white">Search tools</text>
    <text x="340" y="212" text-anchor="middle" style="font-size:13px" fill="white">this talk</text>

    <line x1="442" y1="180" x2="486" y2="180" stroke="#888" stroke-width="2.5" marker-end="url(#wp7-arr)" />
    <text x="464" y="170" text-anchor="middle" style="font-size:11px" fill="#565B65">calls</text>

    <!-- T7.5 -->
    <rect x="490" y="145" width="170" height="70" rx="10" fill="#4DA167" />
    <text x="575" y="168" text-anchor="middle" style="font-size:12px" fill="white" opacity="0.85">T7.5</text>
    <text x="575" y="189" text-anchor="middle" style="font-size:16px" font-weight="700" fill="white">Discovery agent</text>
    <text x="575" y="206" text-anchor="middle" style="font-size:12px" fill="white">calls the tools</text>

    <!-- Pilots -->
    <text x="785" y="60" text-anchor="middle" style="font-size:13px" font-weight="700" fill="#565B65">Three pilots</text>
    <g v-for="p in pilots" :key="p.t">
      <path :d="`M662,180 C686,180 686,${p.y + 23} 706,${p.y + 23}`" fill="none" stroke="#888" stroke-width="2" marker-end="url(#wp7-arr)" />
      <rect x="710" :y="p.y" width="150" height="46" rx="8" fill="white" stroke="#565B65" stroke-width="2" />
      <text x="785" :y="p.y + 28" text-anchor="middle" style="font-size:15px" fill="#443f3f">{{ p.t }}</text>
    </g>

    <!-- T7.4 side box -->
    <line x1="340" y1="237" x2="340" y2="283" stroke="#A71D31" stroke-width="2" stroke-dasharray="6 4" />
    <text x="350" y="264" style="font-size:11px" fill="#565B65">explain results</text>
    <rect x="240" y="285" width="200" height="60" rx="10" fill="white" stroke="#A71D31" stroke-width="2" stroke-dasharray="6 4" />
    <text x="340" y="307" text-anchor="middle" style="font-size:12px" fill="#A71D31">T7.4</text>
    <text x="340" y="329" text-anchor="middle" style="font-size:14px" font-weight="700" fill="#443f3f">Provenance, explanations</text>

    <text x="20" y="30" style="font-size:13px" fill="#565B65">WP7 · AI-ready dataspaces</text>
  </svg>
</template>
