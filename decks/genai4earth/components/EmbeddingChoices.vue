<script setup lang="ts">
// The knobs of an embedding model and their trade-offs. Generic, no benchmark numbers.
// Top row: model size, vector length, quantisation. Bottom row: fit, and the rules.
const vec = [0.92, 0.31, 0.68, 0.12, 0.55, 0.83, 0.27, 0.61, 0.44, 0.75, 0.18, 0.5]
const longCells = Array.from({ length: 24 }, (_, i) => vec[i % 12] * (i < 12 ? 1 : 0.8))
const shortCells = longCells.slice(0, 8)
// The same 8 values at three precisions.
const q8 = vec.slice(0, 8)
const quant = [
  { n: 'float32', s: 'full detail', cells: q8, mem: 120, l: '1×' },
  { n: 'int8', s: 'about 4× smaller', cells: q8.map(v => Math.round(v * 4) / 4), mem: 30, l: '¼' },
  { n: 'binary', s: 'about 32× smaller', cells: q8.map(v => (v > 0.5 ? 1 : 0.08)), mem: 4, l: '1/32' },
].map((r, i) => ({ ...r, y: 66 + i * 46 }))
const fits = [
  { t: 'context length', s: 'text per chunk' },
  { t: 'language', s: 'French, German, English…' },
  { t: 'domain', s: 'Earth science words' },
].map((f, i) => ({ ...f, y: 262 + i * 30 }))
</script>

<template>
  <svg viewBox="0 0 880 360" style="width:100%;height:auto">
    <defs>
      <marker id="ec-ah" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="6" markerHeight="6" orient="auto">
        <path d="M0,0 L10,5 L0,10 z" fill="#888" />
      </marker>
    </defs>

    <!-- panel 1: model size -->
    <rect x="4" y="6" width="200" height="214" rx="12" fill="#CF3F02" opacity="0.06" />
    <text x="20" y="32" style="font-size:15px" font-weight="700" fill="#CF3F02">Model size</text>
    <circle cx="58" cy="102" r="16" fill="#CF3F02" opacity="0.55" />
    <text x="58" y="146" text-anchor="middle" style="font-size:13px" font-weight="700" fill="#443f3f">small</text>
    <text x="58" y="164" text-anchor="middle" style="font-size:12px" fill="#565B65">fast, cheap</text>
    <circle cx="146" cy="102" r="36" fill="#CF3F02" />
    <text x="146" y="164" text-anchor="middle" style="font-size:13px" font-weight="700" fill="#443f3f">large</text>
    <text x="146" y="182" text-anchor="middle" style="font-size:12px" fill="#565B65">more detail, slower</text>
    <text x="104" y="208" text-anchor="middle" style="font-size:12px" fill="#565B65">speed ↔ quality</text>

    <!-- panel 2: vector length -->
    <rect x="214" y="6" width="290" height="214" rx="12" fill="#2E86AB" opacity="0.06" />
    <text x="230" y="32" style="font-size:15px" font-weight="700" fill="#2E86AB">Vector length</text>
    <text x="230" y="62" style="font-size:12px" fill="#565B65">long: more detail</text>
    <rect v-for="(v, i) in longCells" :key="'l' + i" :x="230 + i * 11" y="70" width="10" height="22" rx="1" fill="#2E86AB" :opacity="0.15 + v * 0.85" />
    <!-- cut line -->
    <line x1="318" y1="66" x2="318" y2="128" stroke="#A71D31" stroke-width="2" stroke-dasharray="4 3" />
    <text x="326" y="122" style="font-size:12px" fill="#A71D31">cut to a shorter length</text>
    <line x1="274" y1="98" x2="274" y2="136" stroke="#888" stroke-width="2" marker-end="url(#ec-ah)" />
    <rect v-for="(v, i) in shortCells" :key="'s' + i" :x="230 + i * 11" y="142" width="10" height="22" rx="1" fill="#2E86AB" :opacity="0.15 + v * 0.85" />
    <text x="326" y="158" style="font-size:12px" fill="#565B65">short: less storage, faster</text>
    <text x="359" y="208" text-anchor="middle" style="font-size:12px" fill="#565B65">storage and speed ↔ detail</text>

    <!-- panel 3: quantisation -->
    <rect x="514" y="6" width="362" height="214" rx="12" fill="#4DA167" opacity="0.08" />
    <text x="530" y="32" style="font-size:15px" font-weight="700" fill="#4DA167">Quantisation</text>
    <text x="860" y="32" text-anchor="end" style="font-size:11px" fill="#565B65">same vector, fewer bits</text>
    <text x="724" y="54" style="font-size:11px" fill="#565B65">memory</text>
    <g v-for="r in quant" :key="r.n">
      <text x="530" :y="r.y + 14" style="font-size:13px" font-weight="700" fill="#443f3f">{{ r.n }}</text>
      <text x="530" :y="r.y + 30" style="font-size:11px" fill="#565B65">{{ r.s }}</text>
      <rect v-for="(v, i) in r.cells" :key="i" :x="640 + i * 9" :y="r.y" width="8" height="22" rx="1" fill="#4DA167" :opacity="0.1 + v * 0.9" />
      <rect x="724" :y="r.y + 4" :width="r.mem" height="14" rx="3" fill="#4DA167" />
      <text :x="732 + r.mem" :y="r.y + 16" style="font-size:12px" font-weight="700" fill="#4DA167">{{ r.l }}</text>
    </g>
    <text x="695" y="208" text-anchor="middle" style="font-size:12px" fill="#565B65">memory ↔ a little accuracy</text>

    <!-- panel 4: fit -->
    <rect x="4" y="230" width="300" height="124" rx="12" fill="#565B65" opacity="0.07" />
    <text x="20" y="252" style="font-size:15px" font-weight="700" fill="#565B65">Fit to your data</text>
    <g v-for="f in fits" :key="f.t">
      <circle cx="26" :cy="f.y + 12" r="4" fill="#565B65" />
      <text x="38" :y="f.y + 16" style="font-size:13px" font-weight="700" fill="#443f3f">{{ f.t }}</text>
      <text x="150" :y="f.y + 16" style="font-size:11px" fill="#565B65">{{ f.s }}</text>
    </g>

    <!-- panel 5: rules -->
    <rect x="314" y="230" width="562" height="124" rx="12" fill="#A71D31" opacity="0.06" />
    <text x="330" y="252" style="font-size:15px" font-weight="700" fill="#A71D31">Rules</text>
    <!-- documents and questions into one model -->
    <rect x="330" y="266" width="88" height="26" rx="5" fill="white" stroke="#565B65" stroke-width="1.5" />
    <text x="374" y="284" text-anchor="middle" style="font-size:12px" fill="#443f3f">documents</text>
    <rect x="330" y="310" width="88" height="26" rx="5" fill="white" stroke="#CF3F02" stroke-width="1.5" />
    <text x="374" y="328" text-anchor="middle" style="font-size:12px" fill="#443f3f">questions</text>
    <line x1="418" y1="279" x2="452" y2="295" stroke="#888" stroke-width="2" marker-end="url(#ec-ah)" />
    <line x1="418" y1="323" x2="452" y2="307" stroke="#888" stroke-width="2" marker-end="url(#ec-ah)" />
    <rect x="456" y="264" width="130" height="74" rx="8" fill="none" stroke="#A71D31" stroke-width="1.5" stroke-dasharray="5 4" />
    <rect x="466" y="280" width="110" height="38" rx="6" fill="#A71D31" />
    <text x="521" y="304" text-anchor="middle" style="font-size:14px" font-weight="700" fill="white">same model</text>
    <text x="521" y="333" text-anchor="middle" style="font-size:10px" fill="#A71D31">standard API</text>
    <!-- rules text -->
    <text x="606" y="278" style="font-size:13px" font-weight="700" fill="#443f3f">New model = re-embed everything</text>
    <text x="606" y="304" style="font-size:13px" font-weight="700" fill="#443f3f">Standard API, so you can swap it</text>
    <text x="606" y="330" style="font-size:13px" font-weight="700" fill="#443f3f">Test it on your own queries</text>
  </svg>
</template>
