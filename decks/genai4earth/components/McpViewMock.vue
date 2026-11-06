<script setup lang="ts">
// A chat app with an MCP view: the tool result renders as an interactive card.
const rows = [
  { y: 168, t: 'Land cover', c: '#4DA167', sel: true },
  { y: 213, t: 'Surface temperature', c: '#A71D31' },
  { y: 258, t: 'Building heights', c: '#2E86AB' },
]
</script>

<template>
  <svg viewBox="0 0 880 360" class="g-map">
    <defs>
      <marker id="mv-arr" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse">
        <path d="M0,0 L10,5 L0,10 z" fill="#888" />
      </marker>
    </defs>

    <!-- Chat window -->
    <rect x="20" y="10" width="580" height="340" rx="12" fill="white" stroke="#bbb" stroke-width="2" />
    <path d="M20,40 V22 a12,12 0 0 1 12,-12 H588 a12,12 0 0 1 12,12 V40 Z" fill="#eee" />
    <circle cx="38" cy="25" r="5" fill="#bbb" />
    <circle cx="54" cy="25" r="5" fill="#bbb" />
    <circle cx="70" cy="25" r="5" fill="#bbb" />
    <text x="310" y="30" text-anchor="middle" style="font-size:12px" fill="#565B65">Chat app</text>

    <!-- User message -->
    <rect x="330" y="52" width="255" height="34" rx="14" fill="#2E86AB" />
    <text x="457" y="74" text-anchor="middle" style="font-size:13px" fill="white">Find land cover data near Munich</text>

    <!-- Assistant reply -->
    <text x="40" y="113" style="font-size:13px" fill="#443f3f">I found 3 datasets. Select one:</text>

    <!-- Embedded view (card) -->
    <rect x="40" y="125" width="540" height="210" rx="10" fill="#fafafa" stroke="#CF3F02" stroke-width="2" />
    <!-- mini map -->
    <rect x="55" y="140" width="180" height="180" rx="6" fill="#eef3f6" stroke="#bbb" />
    <path d="M55,250 C100,235 140,270 235,240" fill="none" stroke="#2E86AB" stroke-width="2" opacity="0.6" />
    <rect x="80" y="165" width="95" height="80" fill="#4DA167" fill-opacity="0.25" stroke="#4DA167" stroke-width="2" />
    <rect x="120" y="195" width="90" height="95" fill="#A71D31" fill-opacity="0.15" stroke="#A71D31" stroke-width="1.5" />
    <rect x="135" y="210" width="40" height="35" fill="#2E86AB" fill-opacity="0.3" stroke="#2E86AB" stroke-width="1.5" />
    <circle cx="150" cy="225" r="4.5" fill="#CF3F02" />

    <!-- table -->
    <text x="255" y="155" style="font-size:12px" font-weight="700" fill="#565B65">Dataset</text>
    <line x1="250" y1="162" x2="565" y2="162" stroke="#bbb" />
    <g v-for="r in rows" :key="r.t">
      <rect x="250" :y="r.y" width="315" height="38" rx="5" :fill="r.sel ? '#fdeee6' : 'white'" :stroke="r.sel ? '#CF3F02' : '#ddd'" />
      <rect x="260" :y="r.y + 13" width="12" height="12" :fill="r.c" />
      <text x="282" :y="r.y + 24" style="font-size:13px" fill="#443f3f">{{ r.t }}</text>
      <rect x="490" :y="r.y + 7" width="66" height="24" rx="5" :fill="r.sel ? '#CF3F02' : 'white'" stroke="#CF3F02" />
      <text x="523" :y="r.y + 23" text-anchor="middle" style="font-size:12px" :fill="r.sel ? 'white' : '#CF3F02'">Select</text>
    </g>

    <!-- Right side: how it works -->
    <rect x="652" y="40" width="198" height="70" rx="10" fill="white" stroke="#565B65" stroke-width="2" />
    <text x="751" y="64" text-anchor="middle" style="font-size:15px" font-weight="700" fill="#443f3f">Model</text>
    <text x="751" y="82" text-anchor="middle" style="font-size:12px" fill="#565B65">sees a short reference</text>
    <text x="751" y="99" text-anchor="middle" style="font-size:12px; font-family:'Roboto Mono', monospace" fill="#CF3F02">@results</text>

    <line x1="751" y1="112" x2="751" y2="196" stroke="#888" stroke-width="2" stroke-dasharray="5 4" marker-end="url(#mv-arr)" />
    <text x="761" y="158" style="font-size:11px" fill="#565B65">points to</text>

    <rect x="652" y="200" width="198" height="70" rx="10" fill="#2E86AB" />
    <text x="751" y="226" text-anchor="middle" style="font-size:15px" font-weight="700" fill="white">Tool state</text>
    <text x="751" y="246" text-anchor="middle" style="font-size:12px" fill="white">full rows and footprints</text>

    <line x1="650" y1="235" x2="584" y2="235" stroke="#888" stroke-width="2" marker-end="url(#mv-arr)" />
    <text x="626" y="226" text-anchor="middle" style="font-size:11px" fill="#565B65">renders</text>

    <text x="751" y="308" text-anchor="middle" style="font-size:14px" font-weight="700" fill="#CF3F02">Data stays in state.</text>
    <text x="751" y="328" text-anchor="middle" style="font-size:14px" font-weight="700" fill="#CF3F02">The view renders it.</text>
  </svg>
</template>
