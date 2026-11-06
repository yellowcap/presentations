<script setup lang="ts">
// Retrieval: keywords and vectors in parallel, fuse, rerank, keep the top 5.
const dots = Array.from({ length: 60 }, (_, i) => ({ x: 42 + (i % 5) * 19, y: 72 + Math.floor(i / 5) * 18 }))
const top = [0, 1, 2, 3, 4].map(i => ({ y: 158 + i * 11, w: 64 - i * 6 }))
</script>

<template>
  <svg viewBox="0 0 880 360" style="width:100%;height:auto">
    <defs>
      <marker id="rf-arrow" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto">
        <path d="M0,0 L10,5 L0,10 z" fill="#888" />
      </marker>
    </defs>

    <!-- candidates -->
    <text x="80" y="38" text-anchor="middle" style="font-size:14px" font-weight="700" fill="#443f3f">all chunks</text>
    <rect x="20" y="50" width="120" height="240" rx="10" fill="#f3f3f3" stroke="#bbb" stroke-width="1.5" />
    <circle v-for="(d, i) in dots" :key="i" :cx="d.x" :cy="d.y" r="4" fill="#999" />

    <!-- two lanes -->
    <line x1="140" y1="100" x2="188" y2="100" stroke="#888" stroke-width="2.5" marker-end="url(#rf-arrow)" />
    <line x1="140" y1="240" x2="188" y2="240" stroke="#888" stroke-width="2.5" marker-end="url(#rf-arrow)" />
    <rect x="190" y="50" width="180" height="100" rx="10" fill="#2E86AB" />
    <text x="280" y="94" text-anchor="middle" style="font-size:19px" font-weight="700" fill="white">keywords</text>
    <text x="280" y="118" text-anchor="middle" style="font-size:13px" fill="white">grep, BM25</text>
    <rect x="190" y="190" width="180" height="100" rx="10" fill="#4DA167" />
    <text x="280" y="234" text-anchor="middle" style="font-size:19px" font-weight="700" fill="white">vectors</text>
    <text x="280" y="258" text-anchor="middle" style="font-size:13px" fill="white">similar meaning</text>
    <text x="280" y="176" text-anchor="middle" style="font-size:12px" fill="#565B65">in parallel</text>

    <!-- fuse -->
    <line x1="370" y1="100" x2="418" y2="138" stroke="#888" stroke-width="2.5" marker-end="url(#rf-arrow)" />
    <line x1="370" y1="240" x2="418" y2="202" stroke="#888" stroke-width="2.5" marker-end="url(#rf-arrow)" />
    <rect x="420" y="100" width="120" height="140" rx="10" fill="#CF3F02" />
    <text x="480" y="166" text-anchor="middle" style="font-size:19px" font-weight="700" fill="white">fuse</text>
    <text x="480" y="190" text-anchor="middle" style="font-size:13px" fill="white">hybrid</text>

    <!-- rerank -->
    <line x1="540" y1="170" x2="588" y2="170" stroke="#888" stroke-width="2.5" marker-end="url(#rf-arrow)" />
    <rect x="590" y="120" width="110" height="100" rx="10" fill="#A71D31" />
    <text x="645" y="166" text-anchor="middle" style="font-size:18px" font-weight="700" fill="white">reranker</text>
    <text x="645" y="190" text-anchor="middle" style="font-size:13px" fill="white">best first</text>

    <!-- top 5 -->
    <line x1="700" y1="170" x2="748" y2="170" stroke="#888" stroke-width="2.5" marker-end="url(#rf-arrow)" />
    <rect x="750" y="120" width="110" height="100" rx="10" fill="white" stroke="#443f3f" stroke-width="2" />
    <text x="805" y="146" text-anchor="middle" style="font-size:16px" font-weight="700" fill="#443f3f">top 5</text>
    <rect v-for="(t, i) in top" :key="i" x="773" :y="t.y" :width="t.w" height="6" rx="3" fill="#CF3F02" />

    <!-- no evidence -->
    <line x1="645" y1="220" x2="645" y2="286" stroke="#888" stroke-width="2" stroke-dasharray="5 4" marker-end="url(#rf-arrow)" />
    <rect x="530" y="290" width="330" height="54" rx="10" fill="#fff8e0" stroke="#E2C044" stroke-width="2" />
    <text x="695" y="314" text-anchor="middle" style="font-size:14px" font-weight="700" fill="#443f3f">Nothing fits?</text>
    <text x="695" y="334" text-anchor="middle" style="font-size:13px" fill="#443f3f">Say "no evidence". Do not guess.</text>
  </svg>
</template>
