<script setup lang="ts">
// Way 3: a coding harness. A plugin installs skills (how and when) and a CLI (code that calls the search tools).
const mono = "font-size:12px; font-family:'Roboto Mono', monospace"
const scenes = [
  { y: 260, id: 'S2B_32UPU_20240614', d: '2024-06-14', c: '3%' },
  { y: 278, id: 'S2A_32UPU_20240722', d: '2024-07-22', c: '5%' },
  { y: 296, id: 'S2B_32UPU_20240818', d: '2024-08-18', c: '8%' },
]
</script>

<template>
  <svg viewBox="0 0 880 360" class="g-map">
    <defs>
      <marker id="ch-arr" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse">
        <path d="M0,0 L10,5 L0,10 z" fill="#888" />
      </marker>
    </defs>

    <!-- Terminal window -->
    <rect x="20" y="10" width="580" height="340" rx="12" fill="#2b2828" stroke="#443f3f" stroke-width="2" />
    <path d="M20,40 V22 a12,12 0 0 1 12,-12 H588 a12,12 0 0 1 12,12 V40 Z" fill="#443f3f" />
    <circle cx="38" cy="25" r="5" fill="#888" />
    <circle cx="54" cy="25" r="5" fill="#888" />
    <circle cx="70" cy="25" r="5" fill="#888" />
    <text x="310" y="30" text-anchor="middle" style="font-size:12px" fill="#ddd">coding harness</text>

    <!-- Install -->
    <text x="36" y="62" :style="mono" fill="#CF3F02">&gt;</text>
    <text x="52" y="62" :style="mono" fill="white">/plugin install genai4earth</text>
    <text x="52" y="81" :style="mono" fill="#4DA167">✓</text>
    <text x="68" y="81" :style="mono" fill="#9fd3e6">skills/find-dataset</text>
    <text x="52" y="99" :style="mono" fill="#4DA167">✓</text>
    <text x="68" y="99" :style="mono" fill="#9fd3e6">skills/build-stac-query</text>
    <text x="52" y="117" :style="mono" fill="#4DA167">✓</text>
    <text x="68" y="117" :style="mono" fill="#9fd3e6">CLI: g4e</text>
    <text x="52" y="135" :style="mono" fill="#aaa">Installed 2 skills and 1 CLI.</text>

    <!-- Request and CLI call -->
    <text x="36" y="166" :style="mono" fill="#CF3F02">&gt;</text>
    <text x="52" y="166" :style="mono" fill="white">Find Sentinel-2 land cover data for Munich, 2024</text>
    <text x="52" y="187" :style="mono" fill="#CF3F02">●</text>
    <text x="68" y="187" :style="mono" fill="#ddd">Skill: find-dataset</text>
    <text x="52" y="206" :style="mono" fill="#CF3F02">●</text>
    <text x="68" y="206" :style="mono" fill="#ddd">Bash: <tspan fill="white">g4e search "land cover Munich 2024" \</tspan></text>
    <text x="118" y="224" :style="mono" fill="white">--collection sentinel-2-l2a</text>

    <!-- Result table -->
    <text x="68" y="242" :style="mono" fill="#aaa">ID</text>
    <text x="250" y="242" :style="mono" fill="#aaa">DATE</text>
    <text x="350" y="242" :style="mono" fill="#aaa">CLOUD</text>
    <line x1="68" y1="248" x2="400" y2="248" stroke="#565B65" />
    <g v-for="s in scenes" :key="s.id">
      <text x="68" :y="s.y" :style="mono" fill="#ddd">{{ s.id }}</text>
      <text x="250" :y="s.y" :style="mono" fill="#ddd">{{ s.d }}</text>
      <text x="350" :y="s.y" :style="mono" fill="#ddd">{{ s.c }}</text>
    </g>
    <text x="52" y="325" :style="mono" fill="white">Found 3 scenes. Best: 14 Jun 2024, 3% cloud.</text>

    <!-- Side panel: harness, plugin, search tools -->
    <rect x="650" y="16" width="200" height="50" rx="10" fill="#2E86AB" />
    <text x="750" y="38" text-anchor="middle" style="font-size:15px" font-weight="700" fill="white">Coding harness</text>
    <text x="750" y="56" text-anchor="middle" style="font-size:11px" fill="white">e.g. Claude Code, Codex</text>
    <line x1="750" y1="68" x2="750" y2="94" stroke="#888" stroke-width="2" marker-end="url(#ch-arr)" />
    <text x="760" y="86" style="font-size:11px" fill="#565B65">uses</text>

    <rect x="640" y="98" width="220" height="134" rx="10" fill="white" stroke="#2E86AB" stroke-width="2" stroke-dasharray="6 4" />
    <text x="750" y="118" text-anchor="middle" style="font-size:14px" font-weight="700" fill="#443f3f">Plugin: genai4earth</text>

    <rect x="650" y="128" width="96" height="94" rx="8" fill="#e3f0f5" stroke="#2E86AB" />
    <text x="698" y="152" text-anchor="middle" style="font-size:14px" font-weight="700" fill="#2E86AB">Skills</text>
    <text x="698" y="174" text-anchor="middle" style="font-size:11px" fill="#565B65">instructions:</text>
    <text x="698" y="190" text-anchor="middle" style="font-size:11px" fill="#565B65">how and when</text>

    <rect x="754" y="128" width="96" height="94" rx="8" fill="#e3f0f5" stroke="#2E86AB" />
    <text x="802" y="152" text-anchor="middle" style="font-size:14px" font-weight="700" fill="#2E86AB">CLI: g4e</text>
    <text x="802" y="174" text-anchor="middle" style="font-size:11px" fill="#565B65">code that</text>
    <text x="802" y="190" text-anchor="middle" style="font-size:11px" fill="#565B65">calls the tools</text>

    <line x1="802" y1="224" x2="802" y2="258" stroke="#888" stroke-width="2" marker-end="url(#ch-arr)" />
    <text x="812" y="250" style="font-size:11px" fill="#565B65">calls</text>
    <rect x="650" y="262" width="200" height="50" rx="10" fill="#CF3F02" />
    <text x="750" y="284" text-anchor="middle" style="font-size:15px" font-weight="700" fill="white">Search tools</text>
    <text x="750" y="302" text-anchor="middle" style="font-size:12px" fill="white">core</text>

    <text x="750" y="338" text-anchor="middle" style="font-size:13px" font-weight="700" fill="#CF3F02">Skills say when. The CLI does it.</text>
  </svg>
</template>
