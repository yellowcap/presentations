<script setup lang="ts">
// The whole system in two lanes: build the index offline, serve it online.
const build = [
  { x: 145, t: 'Sources', s: 'catalogue, docs', c: '#565B65' },
  { x: 269, t: 'Chunk', s: 'split text', c: '#2E86AB' },
  { x: 393, t: 'Embed', s: 'text to vectors', c: '#2E86AB' },
  { x: 517, t: 'Snapshot', s: 'object storage', c: '#2E86AB' },
  { x: 641, t: 'Evals', s: 'gate', c: '#E2C044', dark: true },
]
const access = [
  { y: 213, t: 'Agent' },
  { y: 253, t: 'MCP server' },
  { y: 293, t: 'CLI' },
]
</script>

<template>
  <svg viewBox="0 0 880 360" class="g-map">
    <defs>
      <marker id="sa-arr" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse">
        <path d="M0,0 L10,5 L0,10 z" fill="#888" />
      </marker>
    </defs>

    <!-- Lanes -->
    <rect x="20" y="20" width="730" height="145" rx="12" fill="#2E86AB" fill-opacity="0.06" stroke="#bbb" />
    <rect x="20" y="195" width="730" height="145" rx="12" fill="#4DA167" fill-opacity="0.06" stroke="#bbb" />

    <text x="75" y="75" text-anchor="middle" style="font-size:20px" font-weight="700" fill="#2E86AB">Build</text>
    <text x="75" y="97" text-anchor="middle" style="font-size:12px" fill="#565B65">nightly cron</text>
    <text x="75" y="113" text-anchor="middle" style="font-size:12px" fill="#565B65">in CI</text>

    <text x="75" y="248" text-anchor="middle" style="font-size:20px" font-weight="700" fill="#4DA167">Serve</text>
    <text x="75" y="270" text-anchor="middle" style="font-size:12px" fill="#565B65">Kubernetes,</text>
    <text x="75" y="286" text-anchor="middle" style="font-size:12px" fill="#565B65">Helm chart</text>
    <text x="75" y="302" text-anchor="middle" style="font-size:12px" fill="#565B65">per service</text>

    <!-- Build lane -->
    <g v-for="(b, i) in build" :key="b.t">
      <rect :x="b.x" y="55" width="100" height="60" rx="8" :fill="b.c" />
      <text :x="b.x + 50" y="81" text-anchor="middle" style="font-size:14px" font-weight="700" :fill="b.dark ? '#443f3f' : 'white'">{{ b.t }}</text>
      <text :x="b.x + 50" y="99" text-anchor="middle" style="font-size:11px" :fill="b.dark ? '#443f3f' : 'white'">{{ b.s }}</text>
      <line v-if="i < build.length - 1" :x1="b.x + 102" y1="85" :x2="b.x + 122" y2="85" stroke="#888" stroke-width="2" marker-end="url(#sa-arr)" />
    </g>
    <text x="691" y="138" text-anchor="middle" style="font-size:11px" fill="#A71D31">fail: keep old index</text>
    <path d="M743,85 H815 V126" fill="none" stroke="#888" stroke-width="2" marker-end="url(#sa-arr)" />
    <text x="779" y="77" text-anchor="middle" style="font-size:11px" fill="#4DA167">pass</text>

    <!-- Vector DB, shared by both lanes -->
    <path d="M770,140 V220 A45,10 0 0 0 860,220 V140" fill="white" stroke="#CF3F02" stroke-width="2.5" />
    <ellipse cx="815" cy="140" rx="45" ry="10" fill="white" stroke="#CF3F02" stroke-width="2.5" />
    <text x="815" y="183" text-anchor="middle" style="font-size:14px" font-weight="700" fill="#443f3f">Vector DB</text>
    <text x="815" y="200" text-anchor="middle" style="font-size:11px" fill="#565B65">versioned</text>

    <!-- Serve lane -->
    <path d="M702,268 H815 V234" fill="none" stroke="#888" stroke-width="2" marker-start="url(#sa-arr)" marker-end="url(#sa-arr)" />

    <rect x="560" y="238" width="140" height="60" rx="10" fill="#CF3F02" />
    <text x="630" y="264" text-anchor="middle" style="font-size:15px" font-weight="700" fill="white">Search tools</text>
    <text x="630" y="282" text-anchor="middle" style="font-size:11px" fill="white">no LLM inside</text>

    <g v-for="a in access" :key="a.t">
      <rect x="380" :y="a.y" width="120" height="32" rx="7" fill="white" stroke="#4DA167" stroke-width="2" />
      <text x="440" :y="a.y + 21" text-anchor="middle" style="font-size:13px" fill="#443f3f">{{ a.t }}</text>
      <path :d="`M502,${a.y + 16} C530,${a.y + 16} 530,268 556,268`" fill="none" stroke="#888" stroke-width="2" marker-end="url(#sa-arr)" />
      <line x1="378" :y1="a.y + 16" x2="312" y2="268" stroke="#888" stroke-width="1.5" stroke-dasharray="4 4" />
    </g>

    <rect x="160" y="243" width="150" height="50" rx="8" fill="white" stroke="#565B65" stroke-width="2" stroke-dasharray="6 4" />
    <text x="235" y="264" text-anchor="middle" style="font-size:14px" font-weight="700" fill="#443f3f">Tracing</text>
    <text x="235" y="281" text-anchor="middle" style="font-size:11px" fill="#565B65">every call logged</text>
  </svg>
</template>
