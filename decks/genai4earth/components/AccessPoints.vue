<script setup lang="ts">
// One core of search tools, three thin wrappers, three kinds of client.
const rows = [
  { y: 40, w: 'Agent', ws: 'custom agent', c: 'Custom app', cs: 'own UI', i: 'app' },
  { y: 150, w: 'MCP server', ws: 'tools over MCP', c: 'Chat app', cs: 'web, mobile', i: 'chat' },
  { y: 260, w: 'CLI + plugin', ws: 'skills + code', c: 'Coding harness', cs: 'Claude Code, Codex', i: 'term' },
]
</script>

<template>
  <svg viewBox="0 0 880 360" class="g-map">
    <defs>
      <marker id="ap-arr" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse">
        <path d="M0,0 L10,5 L0,10 z" fill="#888" />
      </marker>
    </defs>

    <text x="130" y="25" text-anchor="middle" style="font-size:13px" font-weight="700" fill="#565B65">Core</text>
    <text x="420" y="25" text-anchor="middle" style="font-size:13px" font-weight="700" fill="#565B65">Thin wrapper</text>
    <text x="730" y="25" text-anchor="middle" style="font-size:13px" font-weight="700" fill="#565B65">Client</text>

    <!-- Core -->
    <rect x="30" y="130" width="200" height="100" rx="12" fill="#CF3F02" />
    <text x="130" y="175" text-anchor="middle" style="font-size:20px" font-weight="700" fill="white">Search tools</text>
    <text x="130" y="199" text-anchor="middle" style="font-size:13px" fill="white">core</text>
    <text x="130" y="258" text-anchor="middle" style="font-size:12px" fill="#565B65">same tools,</text>
    <text x="130" y="274" text-anchor="middle" style="font-size:12px" fill="#565B65">three ways in</text>

    <g v-for="r in rows" :key="r.w">
      <path :d="`M232,180 C285,180 280,${r.y + 30} 326,${r.y + 30}`" fill="none" stroke="#888" stroke-width="2" marker-end="url(#ap-arr)" />
      <!-- wrapper -->
      <rect x="330" :y="r.y" width="180" height="60" rx="10" fill="white" stroke="#2E86AB" stroke-width="2" />
      <text x="420" :y="r.y + 27" text-anchor="middle" style="font-size:16px" font-weight="700" fill="#443f3f">{{ r.w }}</text>
      <text x="420" :y="r.y + 46" text-anchor="middle" style="font-size:12px" fill="#565B65">{{ r.ws }}</text>
      <line x1="512" :y1="r.y + 30" x2="606" :y2="r.y + 30" stroke="#888" stroke-width="2" marker-end="url(#ap-arr)" />
      <!-- client -->
      <rect x="610" :y="r.y" width="240" height="60" rx="10" fill="#2E86AB" />
      <g v-if="r.i === 'app'" :transform="`translate(624,${r.y + 15})`">
        <rect x="0" y="0" width="34" height="30" rx="3" fill="white" />
        <rect x="0" y="0" width="34" height="7" rx="2" fill="#bbb" />
        <rect x="5" y="12" width="12" height="13" fill="#2E86AB" opacity="0.5" />
        <rect x="20" y="12" width="9" height="4" fill="#bbb" />
        <rect x="20" y="20" width="9" height="4" fill="#bbb" />
      </g>
      <g v-if="r.i === 'chat'" :transform="`translate(624,${r.y + 14})`">
        <path d="M0,4 a4,4 0 0 1 4,-4 h26 a4,4 0 0 1 4,4 v16 a4,4 0 0 1 -4,4 h-18 l-8,7 v-7 h0 a4,4 0 0 1 -4,-4 z" fill="white" />
        <rect x="6" y="8" width="22" height="3" fill="#bbb" />
        <rect x="6" y="14" width="14" height="3" fill="#bbb" />
      </g>
      <g v-if="r.i === 'term'" :transform="`translate(624,${r.y + 15})`">
        <rect x="0" y="0" width="34" height="30" rx="3" fill="#443f3f" stroke="white" stroke-width="1.5" />
        <path d="M6,9 l6,5 l-6,5" fill="none" stroke="white" stroke-width="2" />
        <line x1="15" y1="21" x2="26" y2="21" stroke="white" stroke-width="2" />
      </g>
      <text x="672" :y="r.y + 27" style="font-size:16px" font-weight="700" fill="white">{{ r.c }}</text>
      <text x="672" :y="r.y + 46" style="font-size:12px" fill="white">{{ r.cs }}</text>
    </g>
  </svg>
</template>
