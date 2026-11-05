<script setup lang="ts">
// One scene of the "Tool data, handled in middleware" walkthrough.
// The Excalidraw drawing is the background; an overlay SVG with the same
// viewBox outlines the active boxes and moves chips along the lines.
// Every animation loops on its own, so a slide never needs a click.
import { computed } from 'vue'

const props = withDefaults(defineProps<{
  scene: number
  cycle?: number
}>(), {
  cycle: 6,
})

const W = 1101.85
const H = 344.64

type Box = [number, number, number, number]
const BOX: Record<string, Box> = {
  user: [10, 58, 136, 46],
  agent: [202, 58, 150, 46],
  state: [201, 231, 147, 46],
  aoi: [856, 21, 224, 46],
  places: [857, 94, 229, 46],
  split: [436, 233, 144, 46],
  inject: [607, 236, 125, 46],
}

const PATH = {
  agentToAoi: 'M300 72 L840 67 L968 44',
  aoiToSplit: 'M968 80 L850 80 L838 113 L664 113 L664 216 L508 256',
  placesToSplit: 'M972 150 L850 150 L838 113 L664 113 L664 216 L508 256',
  splitToState: 'M508 256 L406 257 L275 254',
  splitToAgent: 'M508 256 L498 216 L498 94 L277 90',
  agentToInject: 'M277 90 L498 94 L498 216 L670 259',
  stateToInject: 'M275 254 L406 257 L420 300 L670 300 L670 259',
  injectToPlaces: 'M670 259 L664 216 L664 113 L850 113 L972 117',
  agentToState: 'M276 81 L276 254',
  stateToAgent: 'M276 254 L276 81',
  agentToUser: 'M277 81 L140 81',
}

type Kind = 'ref' | 'call' | 'poly' | 'pts' | 'data'
interface Token { path: string, kind: Kind, label?: string, t0: number, t1: number, hold?: number }
interface Scene { boxes: string[], tokens: Token[], held: string[], stored: ('poly' | 'pts')[], answer?: boolean }

const SCENES: Record<number, Scene> = {
  1: {
    boxes: ['user', 'agent', 'aoi'],
    tokens: [{ path: PATH.agentToAoi, kind: 'call', label: 'AOI Search("Taj Mahal")', t0: 0.1, t1: 0.6 }],
    held: [], stored: [],
  },
  2: {
    boxes: ['aoi', 'split', 'state', 'agent'],
    tokens: [
      { path: PATH.aoiToSplit, kind: 'poly', t0: 0.05, t1: 0.4, hold: 0.42 },
      { path: PATH.splitToState, kind: 'poly', t0: 0.45, t1: 0.75 },
      { path: PATH.splitToAgent, kind: 'ref', label: '@taj_mahal', t0: 0.45, t1: 0.8 },
    ],
    held: ['@taj_mahal'], stored: ['poly'],
  },
  3: {
    boxes: ['agent', 'inject'],
    tokens: [{ path: PATH.agentToInject, kind: 'call', label: 'Places Search(@taj_mahal, "coffee")', t0: 0.1, t1: 0.65 }],
    held: ['@taj_mahal'], stored: ['poly'],
  },
  4: {
    boxes: ['state', 'inject', 'places'],
    tokens: [
      { path: PATH.stateToInject, kind: 'poly', t0: 0.05, t1: 0.4, hold: 0.42 },
      { path: PATH.injectToPlaces, kind: 'call', label: 'Places Search(polygon, "coffee")', t0: 0.45, t1: 0.85 },
    ],
    held: ['@taj_mahal'], stored: ['poly'],
  },
  5: {
    boxes: ['places', 'split', 'state', 'agent'],
    tokens: [
      { path: PATH.placesToSplit, kind: 'pts', t0: 0.05, t1: 0.4, hold: 0.42 },
      { path: PATH.splitToState, kind: 'pts', t0: 0.45, t1: 0.75 },
      { path: PATH.splitToAgent, kind: 'ref', label: '@coffee_shops', t0: 0.45, t1: 0.8 },
    ],
    held: ['@taj_mahal', '@coffee_shops'], stored: ['poly', 'pts'],
  },
  6: {
    boxes: ['agent', 'state', 'user'],
    tokens: [
      { path: PATH.agentToState, kind: 'call', label: 'inspect(@coffee_shops)', t0: 0.05, t1: 0.3 },
      { path: PATH.stateToAgent, kind: 'data', label: '12 places', t0: 0.35, t1: 0.6 },
      { path: PATH.agentToUser, kind: 'data', label: '12 coffee shops', t0: 0.65, t1: 0.85 },
    ],
    held: ['@taj_mahal', '@coffee_shops'], stored: ['poly', 'pts'], answer: true,
  },
}

const scene = computed(() => SCENES[props.scene])
const dur = computed(() => `${props.cycle}s`)

const MONO = 14.4 // character width at font-size 24, Roboto Mono

function pillWidth(label = '') {
  return label.length * MONO + 22
}

// Split a call label so @references get the reference colour.
function segments(label = '') {
  return label.split(/(@\w+)/).filter(Boolean).map(t => ({ t, ref: t.startsWith('@') }))
}

function motionTimes(tok: Token) {
  return { keyPoints: '0;0;1;1', keyTimes: `0;${tok.t0};${tok.t1};1` }
}

function fadeTimes(tok: Token) {
  const end = Math.min(tok.hold ?? tok.t1 + 0.1, 0.96)
  return { values: '0;0;1;1;0;0', keyTimes: `0;${tok.t0};${tok.t0 + 0.02};${end};${end + 0.03};1` }
}

function heldX(i: number) {
  return BOX.agent[0] + scene.value.held.slice(0, i).reduce((x, l) => x + pillWidth(l) * 0.7 + 8, 0)
}
</script>

<template>
  <div class="mw-flow relative w-full" :style="{ aspectRatio: `${W} / ${H + 40}` }">
    <img src="/images/mcp-runtime/middleware-flow.svg" alt="" class="absolute left-0 top-0 w-full" :style="{ height: `${(H / (H + 40)) * 100}%` }" />
    <svg class="absolute inset-0 w-full h-full" :viewBox="`0 0 ${W} ${H + 40}`" role="img"
      :aria-label="`Middleware flow, scene ${props.scene}`">
      <defs>
        <symbol id="mw-poly" viewBox="0 0 40 28">
          <rect x="1" y="1" width="38" height="26" rx="6" fill="white" stroke="#2E86AB" stroke-width="2" />
          <path d="M9 20 L12 8 L22 6 L31 11 L28 21 Z" fill="#2E86AB" fill-opacity="0.25" stroke="#2E86AB" stroke-width="2" stroke-linejoin="round" />
        </symbol>
        <symbol id="mw-pts" viewBox="0 0 40 28">
          <rect x="1" y="1" width="38" height="26" rx="6" fill="white" stroke="#2E86AB" stroke-width="2" />
          <g fill="#2E86AB">
            <circle cx="11" cy="10" r="3" /><circle cx="20" cy="18" r="3" /><circle cx="27" cy="9" r="3" />
            <circle cx="31" cy="19" r="3" /><circle cx="14" cy="20" r="3" />
          </g>
        </symbol>
      </defs>

      <!-- The question, under the user -->
      <text x="10" y="140" class="mw-q">“How many coffee shops</text>
      <text x="10" y="166" class="mw-q">near the Taj Mahal?”</text>
      <text v-if="scene.answer" x="10" y="204" class="mw-a">12 coffee shops.</text>

      <!-- Active boxes, pulsing -->
      <rect v-for="b in scene.boxes" :key="b" :x="BOX[b][0] - 4" :y="BOX[b][1] - 4"
        :width="BOX[b][2] + 8" :height="BOX[b][3] + 8" rx="10" class="mw-hl">
        <animate attributeName="stroke-opacity" values="1;0.3;1" dur="1.6s" repeatCount="indefinite" />
      </rect>

      <!-- What the agent holds: references only -->
      <g v-for="(l, i) in scene.held" :key="l" :transform="`translate(${heldX(i)} 20) scale(0.7)`">
        <rect :width="pillWidth(l)" height="44" rx="22" fill="#CF3F02" />
        <text :x="pillWidth(l) / 2" y="30" class="mw-mono" fill="white" text-anchor="middle">{{ l }}</text>
      </g>

      <!-- What State holds: the data -->
      <g v-for="(k, i) in scene.stored" :key="k" :transform="`translate(${210 + i * 150} 286)`">
        <use :href="`#mw-${k}`" width="60" height="42" />
        <text x="30" y="66" class="mw-key" text-anchor="middle">{{ k === 'poly' ? 'taj_mahal' : 'coffee_shops' }}</text>
      </g>

      <!-- Moving chips, looping -->
      <g v-for="(tok, i) in scene.tokens" :key="`${props.scene}-${i}`" opacity="0">
        <g v-if="tok.kind === 'poly' || tok.kind === 'pts'">
          <use :href="`#mw-${tok.kind}`" x="-36" y="-25" width="72" height="50" />
        </g>
        <g v-else :transform="`translate(${-pillWidth(tok.label) / 2} -22)`">
          <rect :width="pillWidth(tok.label)" height="44" :rx="tok.kind === 'ref' ? 22 : 8"
            :class="`mw-chip mw-${tok.kind}`" />
          <text :x="pillWidth(tok.label) / 2" y="30" class="mw-mono" text-anchor="middle">
            <tspan v-for="(s, j) in segments(tok.label)" :key="j" :class="s.ref && tok.kind !== 'ref' ? 'mw-refword' : ''">{{ s.t }}</tspan>
          </text>
        </g>
        <animateMotion :path="tok.path" :dur="dur" repeatCount="indefinite" calcMode="linear"
          v-bind="motionTimes(tok)" />
        <animate attributeName="opacity" :dur="dur" repeatCount="indefinite" v-bind="fadeTimes(tok)" />
      </g>
    </svg>
  </div>
</template>

<style scoped>
.mw-hl {
  fill: rgba(207, 63, 2, 0.08);
  stroke: #CF3F02;
  stroke-width: 3;
}
.mw-mono {
  font-family: 'Roboto Mono', monospace;
  font-size: 24px;
}
.mw-chip.mw-ref { fill: #CF3F02; }
.mw-chip.mw-ref ~ text { fill: white; }
.mw-chip.mw-call { fill: white; stroke: #443F3F; stroke-width: 2; }
.mw-chip.mw-call ~ text { fill: #443F3F; }
.mw-chip.mw-data { fill: #2E86AB; }
.mw-chip.mw-data ~ text { fill: white; }
.mw-refword { fill: #CF3F02; font-weight: 700; }
.mw-q { font-size: 22px; font-style: italic; fill: #443F3F; }
.mw-a { font-size: 28px; font-weight: 700; fill: #2E86AB; }
.mw-key { font-family: 'Roboto Mono', monospace; font-size: 18px; fill: #443F3F; }
</style>
