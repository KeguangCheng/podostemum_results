<script setup lang="ts">
// Point estimates with intervals (one linear axis, in %)
import { computed } from 'vue'

interface Est { label: string, sub?: string, v: number, lo: number, hi: number, em?: boolean, text?: string }

const props = withDefaults(defineProps<{
  rows: Est[]
  max?: number
  width?: number
  step?: number
  labelWidth?: number
}>(), { max: 20, width: 520, step: 5, labelWidth: 236 })

const labelW = props.labelWidth
const rowH = 40
const top = 6
const axisH = 30
const plotW = computed(() => props.width - labelW - 20)
const x = (v: number) => labelW + (v / props.max) * plotW.value
const height = computed(() => top + props.rows.length * rowH + axisH)
const ticks = computed(() => {
  const out: number[] = []
  for (let v = 0; v <= props.max + 1e-9; v += props.step) out.push(v)
  return out
})
</script>

<template>
  <svg :viewBox="`0 0 ${width} ${height}`" :width="width" :height="height" class="eci" role="img">
    <g v-for="t in ticks" :key="t">
      <line :x1="x(t)" :x2="x(t)" :y1="top" :y2="height - axisH + 2" stroke="#dcd7ee" stroke-width="1" />
      <text :x="x(t)" :y="height - axisH + 17" text-anchor="middle" class="tick">{{ t }}%</text>
    </g>
    <g v-for="(r, i) in rows" :key="r.label" :transform="`translate(0, ${top + i * rowH + rowH / 2})`">
      <text x="0" y="-2" class="lab" :class="{ em: r.em }">{{ r.label }}</text>
      <text v-if="r.sub" x="0" y="13" class="sub">{{ r.sub }}</text>
      <line :x1="x(r.lo)" :x2="x(r.hi)" y1="0" y2="0" :stroke="r.em ? '#5b4bc4' : '#9d90e8'" stroke-width="3" stroke-linecap="round" />
      <circle :cx="x(r.v)" cy="0" r="6" :fill="r.em ? '#5b4bc4' : '#8f81e6'" stroke="#fff" stroke-width="2">
        <title>{{ r.label }}: {{ r.v }}% ({{ r.lo }}–{{ r.hi }}%)</title>
      </circle>
      <text :x="x(r.hi) + 8" y="4" class="val" :class="{ em: r.em }">{{ r.text ?? `${r.v}% (${r.lo}–${r.hi}%)` }}</text>
    </g>
  </svg>
</template>

<style scoped>
.eci { display: block; max-width: 100%; height: auto; font-family: var(--font); }
.tick { font-size: 11px; fill: #5e5a6e; }
.lab { font-size: 12.5px; fill: #1f1d2b; }
.lab.em { font-weight: 650; }
.sub { font-size: 11px; fill: #8d899b; }
.val { font-size: 12px; fill: #5e5a6e; }
.val.em { fill: #1f1d2b; font-weight: 600; }
</style>
