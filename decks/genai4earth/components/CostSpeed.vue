<script setup lang="ts">
// Where time and cost go. Top: one request path with an illustrative share per step.
// Bottom: one-off costs that do not depend on the number of questions.
// Shares are illustrative only (no measured numbers).
const base = 200 // bar baseline
const hMax = 100
const steps = [
  { t: 'Question', share: 0, llm: false, lever: '' },
  { t: 'LLM plans', share: 0.85, llm: true, lever: 'small model if enough' },
  { t: 'Search tools', share: 0.12, llm: false, lever: 'keep LLMs out of tools' },
  { t: 'Database', share: 0.06, llm: false, lever: 'indexes · cache' },
  { t: 'Reranker', share: 0.3, llm: false, lever: 'rerank only top hits' },
  { t: 'LLM answers', share: 1, llm: true, lever: 'small model · cache' },
].map((s, i) => ({ ...s, x: 16 + i * 145, h: s.share * hMax }))
const oneOff = [
  { t: 'Embed the corpus', s: 'once, at build time' },
  { t: 'Re-embed everything', s: 'when the model changes' },
  { t: 'Storage', s: 'grows with vector length' },
].map((c, i) => ({ ...c, x: 200 + i * 226 }))
</script>

<template>
  <svg viewBox="0 0 880 360" style="width:100%;height:auto">
    <defs>
      <marker id="cs-ah" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="6" markerHeight="6" orient="auto">
        <path d="M0,0 L10,5 L0,10 z" fill="#888" />
      </marker>
    </defs>

    <!-- per question lane -->
    <rect x="4" y="6" width="872" height="236" rx="12" fill="#CF3F02" opacity="0.06" />
    <text x="22" y="30" style="font-size:15px" font-weight="700" fill="#CF3F02">Each question</text>
    <text x="860" y="30" text-anchor="end" style="font-size:12px" fill="#565B65">bar = typical share of time and cost (illustrative)</text>

    <g v-for="(s, i) in steps" :key="s.t">
      <rect :x="s.x" y="42" width="124" height="38" rx="8" :fill="i === 0 ? 'white' : (s.llm ? '#CF3F02' : '#2E86AB')" :stroke="i === 0 ? '#565B65' : 'none'" stroke-width="1.5" />
      <text :x="s.x + 62" y="66" text-anchor="middle" style="font-size:14px" font-weight="700" :fill="i === 0 ? '#443f3f' : 'white'">{{ s.t }}</text>
      <line v-if="i < steps.length - 1" :x1="s.x + 126" y1="61" :x2="s.x + 143" y2="61" stroke="#888" stroke-width="2" marker-end="url(#cs-ah)" />
      <!-- share bar -->
      <rect v-if="i > 0" :x="s.x + 37" :y="base - hMax" width="50" :height="hMax" rx="4" fill="#e9e9e9" />
      <rect v-if="i > 0" :x="s.x + 37" :y="base - s.h" width="50" :height="s.h" rx="4" :fill="s.llm ? '#CF3F02' : '#2E86AB'" />
      <text :x="s.x + 62" y="226" text-anchor="middle" style="font-size:11.5px" fill="#443f3f">{{ s.lever }}</text>
    </g>
    <text x="78" y="140" text-anchor="middle" style="font-size:13px" fill="#565B65">LLM calls</text>
    <text x="78" y="158" text-anchor="middle" style="font-size:13px" fill="#565B65">dominate</text>
    <text x="78" y="226" text-anchor="middle" style="font-size:11.5px" font-weight="700" fill="#443f3f">levers:</text>

    <!-- one-off lane -->
    <rect x="4" y="252" width="872" height="102" rx="12" fill="#2E86AB" opacity="0.07" />
    <text x="22" y="282" style="font-size:15px" font-weight="700" fill="#2E86AB">One-off costs</text>
    <text x="22" y="302" style="font-size:12px" fill="#565B65">not per question</text>
    <g v-for="c in oneOff" :key="c.t">
      <rect :x="c.x" y="264" width="210" height="52" rx="8" fill="white" stroke="#2E86AB" stroke-width="1.5" />
      <text :x="c.x + 105" y="286" text-anchor="middle" style="font-size:14px" font-weight="700" fill="#443f3f">{{ c.t }}</text>
      <text :x="c.x + 105" y="305" text-anchor="middle" style="font-size:12px" fill="#565B65">{{ c.s }}</text>
    </g>
    <text x="200" y="340" style="font-size:12px" fill="#443f3f"><tspan font-weight="700">levers:</tspan> quantise · shorter vectors · smaller model · embed only what changed</text>
  </svg>
</template>
