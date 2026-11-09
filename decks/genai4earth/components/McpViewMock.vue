<script setup lang="ts">
// Way 2: a chat app through an MCP server. The same view renders on web and mobile.
const rows = [
  { y: 172, t: 'Land cover', c: '#4DA167', sel: true },
  { y: 214, t: 'Surface temperature', c: '#A71D31' },
  { y: 256, t: 'Building heights', c: '#2E86AB' },
]
const mrows = [
  { y: 228, t: 'Land cover', c: '#4DA167', sel: true },
  { y: 254, t: 'Surface temp.', c: '#A71D31' },
  { y: 280, t: 'Building heights', c: '#2E86AB' },
]
const side = [
  { y: 30, t: 'Chat app', s: 'web, mobile', fill: '#2E86AB', fg: 'white', sub: 'white' },
  { y: 122, t: 'MCP server', s: 'tools + views', fill: 'white', fg: '#443f3f', sub: '#565B65', stroke: '#2E86AB' },
  { y: 214, t: 'Search tools', s: 'core', fill: '#CF3F02', fg: 'white', sub: 'white' },
]
</script>

<template>
  <svg viewBox="0 0 880 360" class="g-map">
    <defs>
      <marker id="mv-arr" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse">
        <path d="M0,0 L10,5 L0,10 z" fill="#888" />
      </marker>
    </defs>

    <!-- Web chat window -->
    <rect x="20" y="10" width="420" height="340" rx="12" fill="white" stroke="#bbb" stroke-width="2" />
    <path d="M20,40 V22 a12,12 0 0 1 12,-12 H428 a12,12 0 0 1 12,12 V40 Z" fill="#eee" />
    <circle cx="38" cy="25" r="5" fill="#bbb" />
    <circle cx="54" cy="25" r="5" fill="#bbb" />
    <circle cx="70" cy="25" r="5" fill="#bbb" />
    <text x="230" y="30" text-anchor="middle" style="font-size:12px" fill="#565B65">Chat app: web</text>

    <rect x="185" y="52" width="240" height="32" rx="14" fill="#2E86AB" />
    <text x="305" y="73" text-anchor="middle" style="font-size:13px" fill="white">Find land cover data near Munich</text>
    <text x="35" y="110" style="font-size:13px" fill="#443f3f">I found 3 datasets. Select one:</text>

    <!-- Embedded view (card) -->
    <rect x="35" y="124" width="390" height="210" rx="10" fill="#fafafa" stroke="#CF3F02" stroke-width="2" />
    <rect x="47" y="138" width="130" height="182" rx="6" fill="#eef3f6" stroke="#bbb" />
    <path d="M47,262 C80,250 120,272 177,244" fill="none" stroke="#2E86AB" stroke-width="2" opacity="0.6" />
    <rect x="62" y="165" width="75" height="75" fill="#4DA167" fill-opacity="0.25" stroke="#4DA167" stroke-width="2" />
    <rect x="92" y="190" width="70" height="85" fill="#A71D31" fill-opacity="0.15" stroke="#A71D31" stroke-width="1.5" />
    <rect x="104" y="205" width="32" height="30" fill="#2E86AB" fill-opacity="0.3" stroke="#2E86AB" stroke-width="1.5" />
    <circle cx="117" cy="220" r="4" fill="#CF3F02" />

    <text x="192" y="155" style="font-size:12px" font-weight="700" fill="#565B65">Dataset</text>
    <line x1="188" y1="162" x2="413" y2="162" stroke="#bbb" />
    <g v-for="r in rows" :key="r.t">
      <rect x="188" :y="r.y" width="225" height="34" rx="5" :fill="r.sel ? '#fdeee6' : 'white'" :stroke="r.sel ? '#CF3F02' : '#ddd'" />
      <rect x="196" :y="r.y + 11" width="11" height="11" :fill="r.c" />
      <text x="214" :y="r.y + 21" style="font-size:12px" fill="#443f3f">{{ r.t }}</text>
      <rect x="352" :y="r.y + 6" width="54" height="22" rx="5" :fill="r.sel ? '#CF3F02' : 'white'" stroke="#CF3F02" />
      <text x="379" :y="r.y + 21" text-anchor="middle" style="font-size:11px" :fill="r.sel ? 'white' : '#CF3F02'">Select</text>
    </g>

    <!-- Phone -->
    <rect x="460" y="18" width="146" height="326" rx="20" fill="#443f3f" />
    <rect x="467" y="32" width="132" height="298" rx="12" fill="white" />
    <rect x="513" y="24" width="40" height="5" rx="2.5" fill="#888" />
    <text x="533" y="50" text-anchor="middle" style="font-size:11px" fill="#565B65">Chat app: mobile</text>
    <line x1="467" y1="58" x2="599" y2="58" stroke="#eee" />
    <rect x="479" y="66" width="114" height="24" rx="11" fill="#2E86AB" />
    <text x="536" y="82" text-anchor="middle" style="font-size:11px" fill="white">Land cover, Munich</text>
    <text x="474" y="108" style="font-size:11px" fill="#443f3f">3 datasets:</text>

    <rect x="473" y="116" width="120" height="190" rx="8" fill="#fafafa" stroke="#CF3F02" stroke-width="2" />
    <rect x="481" y="124" width="104" height="94" rx="4" fill="#eef3f6" stroke="#bbb" />
    <path d="M481,190 C505,182 535,198 585,178" fill="none" stroke="#2E86AB" stroke-width="1.5" opacity="0.6" />
    <rect x="492" y="134" width="50" height="50" fill="#4DA167" fill-opacity="0.25" stroke="#4DA167" stroke-width="1.5" />
    <rect x="512" y="150" width="48" height="58" fill="#A71D31" fill-opacity="0.15" stroke="#A71D31" stroke-width="1" />
    <rect x="521" y="161" width="22" height="20" fill="#2E86AB" fill-opacity="0.3" stroke="#2E86AB" stroke-width="1" />
    <circle cx="532" cy="171" r="3" fill="#CF3F02" />
    <g v-for="r in mrows" :key="r.t">
      <rect x="479" :y="r.y - 2" width="108" height="22" rx="4" :fill="r.sel ? '#fdeee6' : 'white'" :stroke="r.sel ? '#CF3F02' : '#ddd'" />
      <rect x="485" :y="r.y + 5" width="8" height="8" :fill="r.c" />
      <text x="498" :y="r.y + 13" style="font-size:11px" fill="#443f3f">{{ r.t }}</text>
    </g>
    <rect x="474" y="314" width="118" height="10" rx="5" fill="#eee" />

    <!-- Side panel: which wrapper -->
    <g v-for="(b, i) in side" :key="b.t">
      <rect x="650" :y="b.y" width="200" height="52" rx="10" :fill="b.fill" :stroke="b.stroke || 'none'" stroke-width="2" />
      <text x="750" :y="b.y + 23" text-anchor="middle" style="font-size:15px" font-weight="700" :fill="b.fg">{{ b.t }}</text>
      <text x="750" :y="b.y + 41" text-anchor="middle" style="font-size:12px" :fill="b.sub">{{ b.s }}</text>
      <line v-if="i < 2" x1="750" :y1="b.y + 54" x2="750" :y2="b.y + 88" stroke="#888" stroke-width="2" marker-end="url(#mv-arr)" />
    </g>
    <text x="760" y="108" style="font-size:11px" fill="#565B65">MCP</text>
    <text x="760" y="200" style="font-size:11px" fill="#565B65">calls</text>

    <text x="750" y="306" text-anchor="middle" style="font-size:14px" font-weight="700" fill="#CF3F02">Data stays in state.</text>
    <text x="750" y="326" text-anchor="middle" style="font-size:14px" font-weight="700" fill="#CF3F02">The view renders it.</text>
  </svg>
</template>
