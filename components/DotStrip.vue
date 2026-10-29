<script setup lang="ts">
// Log-scale dot strip: two rows per taxon (root / leaf); dots are the three replicates, the tick is the mean
import { computed } from 'vue'

interface Group { label: string, sub?: string, call?: string, r: number[], l: number[] }

const props = withDefaults(defineProps<{
  groups: Group[]
  min?: number
  max?: number
  width?: number
}>(), { min: 1e-5, max: 0.2, width: 560 })

const C = { r: '#d4893a', l: '#16958b' }
const labelW = 196
const callW = 0
const rowH = 15
const groupGap = 16
const top = 8
const axisH = 34

const plotW = computed(() => props.width - labelW - callW - 14)
const lx = (v: number) => {
  const a = Math.log10(props.min)
  const b = Math.log10(props.max)
  const t = (Math.log10(Math.max(v, props.min)) - a) / (b - a)
  return labelW + t * plotW.value
}
const ticks = computed(() => {
  const out: { v: number, s: string }[] = []
  for (let e = Math.ceil(Math.log10(props.min)); e <= Math.floor(Math.log10(props.max)); e++)
    out.push({ v: 10 ** e, s: e >= -3 ? String(10 ** e).replace(/^0\./, '0.') : `1e${e}` })
  return out
})
const gH = 2 * rowH + groupGap
const height = computed(() => top + props.groups.length * gH + axisH - groupGap + 6)
const mean = (a: number[]) => a.reduce((s, x) => s + x, 0) / a.length
const tissues = ['r', 'l'] as const
</script>

<template>
  <div class="dstrip-legend">
    <span><i style="background:#d4893a" />Root</span>
    <span><i style="background:#16958b" />Leaf</span>
    <span class="faint">dots: three replicates; tick: mean</span>
  </div>
  <svg :viewBox="`0 0 ${width} ${height}`" :width="width" :height="height" class="dstrip" role="img">
    <g v-for="t in ticks" :key="t.v">
      <line :x1="lx(t.v)" :x2="lx(t.v)" :y1="top - 4" :y2="height - axisH + 4" stroke="#dcd7ee" stroke-width="1" />
      <text :x="lx(t.v)" :y="height - axisH + 20" text-anchor="middle" class="tick">{{ t.s }}</text>
    </g>
    <text :x="labelW + plotW / 2" :y="height - 2" text-anchor="middle" class="axis">% of original library (log scale)</text>
    <g v-for="(g, gi) in groups" :key="g.label" :transform="`translate(0, ${top + gi * gH})`">
      <text x="0" :y="rowH * 1.35" class="glabel">{{ g.label }}</text>
      <text v-if="g.sub" x="0" :y="rowH * 2.45" class="gsub">{{ g.sub }}</text>
      <g v-for="(t, ti) in tissues" :key="t" :transform="`translate(0, ${ti * rowH + rowH / 2})`">
        <line :x1="lx(mean(g[t]))" :x2="lx(mean(g[t]))" y1="-6" y2="6" :stroke="C[t]" stroke-width="3" stroke-linecap="round" />
        <circle v-for="(v, vi) in g[t]" :key="vi" :cx="lx(v)" cy="0" r="4.2" :fill="C[t]" stroke="#fff" stroke-width="1.5">
          <title>{{ t === 'r' ? 'Root' : 'Leaf' }} replicate {{ vi + 1 }}: {{ v }}%</title>
        </circle>
      </g>
    </g>
  </svg>
</template>

<style scoped>
.dstrip { display: block; max-width: 100%; height: auto; font-family: var(--font); }
.dstrip-legend { display: flex; gap: 14px; font-size: 12px; color: var(--ink2); margin: 0 0 4px; }
.dstrip-legend i { display: inline-block; width: 9px; height: 9px; border-radius: 50%; margin-right: 5px; vertical-align: 0; }
.tick { font-size: 11px; fill: #5e5a6e; }
.axis { font-size: 11px; fill: #8d899b; }
.glabel { font-size: 13px; font-weight: 600; fill: #1f1d2b; }
.gsub { font-size: 11px; fill: #5e5a6e; }
</style>
