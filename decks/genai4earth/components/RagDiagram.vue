<script setup lang="ts">
// RAG: the search tool picks a few chunks; the LLM reads only those chunks
// (and the question) and answers with source tags.
const chunks = [
  { y: 112, id: 'A', c: '#2E86AB' },
  { y: 157, id: 'B', c: '#4DA167' },
  { y: 202, id: 'C', c: '#A71D31' },
]
const lines = [0, 1, 2, 3, 4]
</script>

<template>
  <svg viewBox="0 0 880 360" style="width:100%;height:auto">
    <defs>
      <marker id="rag-ah" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse">
        <path d="M0,0 L10,5 L0,10 z" fill="#888" />
      </marker>
    </defs>

    <!-- question -->
    <rect x="16" y="150" width="114" height="50" rx="12" fill="white" stroke="#CF3F02" stroke-width="2" />
    <text x="73" y="171" text-anchor="middle" style="font-size:12px" fill="#443f3f">"Which data</text>
    <text x="73" y="187" text-anchor="middle" style="font-size:12px" fill="#443f3f">on wheat?"</text>
    <text x="73" y="222" text-anchor="middle" style="font-size:13px" fill="#443f3f">question</text>

    <!-- the question also goes to the LLM -->
    <path d="M73,150 V40 H610 V136" fill="none" stroke="#888" stroke-width="1.5" stroke-dasharray="5 4" marker-end="url(#rag-ah)" />
    <text x="340" y="32" text-anchor="middle" style="font-size:11px" fill="#565B65">the question goes to the LLM too</text>

    <line x1="134" y1="175" x2="186" y2="175" stroke="#888" stroke-width="2" marker-end="url(#rag-ah)" />

    <!-- search tool -->
    <rect x="190" y="150" width="100" height="50" rx="10" fill="#2E86AB" />
    <text x="240" y="171" text-anchor="middle" style="font-size:14px" font-weight="700" fill="white">search</text>
    <text x="240" y="189" text-anchor="middle" style="font-size:14px" font-weight="700" fill="white">tool</text>

    <!-- index below the search tool -->
    <line x1="240" y1="204" x2="240" y2="252" stroke="#888" stroke-width="2" marker-start="url(#rag-ah)" marker-end="url(#rag-ah)" />
    <path d="M195,262 v48 a45,10 0 0 0 90,0 v-48" fill="#eeeeee" stroke="#565B65" stroke-width="1.5" />
    <ellipse cx="240" cy="262" rx="45" ry="10" fill="#dddddd" stroke="#565B65" stroke-width="1.5" />
    <text x="240" y="294" text-anchor="middle" style="font-size:12px" fill="#443f3f">index</text>
    <text x="240" y="342" text-anchor="middle" style="font-size:11px" fill="#565B65">all chunks: the LLM does not see them</text>

    <line x1="294" y1="175" x2="356" y2="175" stroke="#888" stroke-width="2" marker-end="url(#rag-ah)" />

    <!-- what the LLM sees -->
    <rect x="345" y="68" width="352" height="200" rx="12" fill="#CF3F02" fill-opacity="0.05" stroke="#CF3F02" stroke-width="2" stroke-dasharray="7 5" />
    <text x="359" y="90" style="font-size:12px" font-weight="700" fill="#CF3F02">the LLM sees only this</text>

    <!-- three chunks -->
    <g v-for="ch in chunks" :key="ch.id">
      <rect x="362" :y="ch.y" width="112" height="34" rx="4" fill="white" :stroke="ch.c" stroke-width="2" />
      <rect x="368" :y="ch.y + 7" width="20" height="20" rx="3" :fill="ch.c" />
      <text x="378" :y="ch.y + 22" text-anchor="middle" style="font-size:12px" font-weight="700" fill="white">{{ ch.id }}</text>
      <line x1="396" x2="464" :y1="ch.y + 12" :y2="ch.y + 12" stroke="#bbb" stroke-width="2" />
      <line x1="396" x2="448" :y1="ch.y + 22" :y2="ch.y + 22" stroke="#bbb" stroke-width="2" />
    </g>
    <text x="418" y="258" text-anchor="middle" style="font-size:12px" fill="#443f3f">top 3 chunks</text>

    <line x1="480" y1="175" x2="534" y2="175" stroke="#888" stroke-width="2" marker-end="url(#rag-ah)" />
    <text x="507" y="166" text-anchor="middle" style="font-size:11px" fill="#565B65">reads</text>

    <!-- LLM -->
    <rect x="540" y="140" width="140" height="72" rx="10" fill="#565B65" />
    <text x="610" y="172" text-anchor="middle" style="font-size:20px" font-weight="700" fill="white">LLM</text>
    <text x="610" y="194" text-anchor="middle" style="font-size:11px" fill="white">writes the answer</text>

    <line x1="701" y1="175" x2="734" y2="175" stroke="#888" stroke-width="2" marker-end="url(#rag-ah)" />

    <!-- answer with source tags -->
    <rect x="740" y="118" width="128" height="114" rx="12" fill="white" stroke="#4DA167" stroke-width="2" />
    <text x="754" y="140" style="font-size:13px" font-weight="700" fill="#443f3f">Answer</text>
    <line v-for="k in lines.slice(0, 3)" :key="'a' + k" x1="754" :x2="k === 2 ? 820 : 854" :y1="154 + k * 12" :y2="154 + k * 12" stroke="#bbb" stroke-width="2" />
    <text x="754" y="207" style="font-size:11px" fill="#565B65">sources:</text>
    <rect x="806" y="194" width="20" height="20" rx="3" fill="#2E86AB" />
    <text x="816" y="209" text-anchor="middle" style="font-size:12px" font-weight="700" fill="white">A</text>
    <rect x="832" y="194" width="20" height="20" rx="3" fill="#A71D31" />
    <text x="842" y="209" text-anchor="middle" style="font-size:12px" font-weight="700" fill="white">C</text>
    <text x="804" y="258" text-anchor="middle" style="font-size:12px" fill="#443f3f">answer + sources</text>
  </svg>
</template>
