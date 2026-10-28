<script setup lang="ts">
// Paired bars: two values per row (e.g. before / after screening, background / candidates) on one linear axis
import { computed } from 'vue'

interface Row { label: string, sw?: string, a: number, b: number, aText?: string, bText?: string, note?: string }

const props = withDefaults(defineProps<{
  rows: Row[]
  aName: string
  bName: string
  max?: number
  unit?: string
  title?: string
  aColor?: string
  bColor?: string
  labelWidth?: number
}>(), {
  unit: '%',
  aColor: '#c3bcdc',
  bColor: '#5b4bc4',
  labelWidth: 96,
})

const m = computed(() => props.max ?? Math.max(...props.rows.flatMap(r => [r.a, r.b])))
const w = (v: number) => `calc((100% - 58px) * ${Math.max(v / m.value, 0.004)})`
const fmt = (v: number, t?: string) => t ?? `${v}${props.unit}`
// Labels, notes and the title may contain <i> for taxon names; tooltips use the plain text
const plain = (s: string) => s.replace(/<[^>]+>/g, '')
</script>

<template>
  <div class="pbars">
    <div v-if="title" class="pbars-title" v-html="title" />
    <div class="pbars-legend">
      <span><i :style="{ background: aColor }" /><span v-html="aName" /></span>
      <span><i :style="{ background: bColor }" /><span v-html="bName" /></span>
    </div>
    <div v-for="r in rows" :key="r.label" class="pbars-row" :style="{ gridTemplateColumns: `${labelWidth}px 1fr` }">
      <div class="pbars-label">
        <span v-if="r.sw" class="sw" :class="r.sw" /><span v-html="r.label" />
        <div v-if="r.note" class="pbars-note" v-html="r.note" />
      </div>
      <div class="pbars-bars">
        <div class="pbars-line" :title="`${plain(aName)}: ${fmt(r.a, r.aText)}`">
          <div class="pbars-fill" :style="{ width: w(r.a), background: aColor }" />
          <span class="pbars-val">{{ fmt(r.a, r.aText) }}</span>
        </div>
        <div class="pbars-line" :title="`${plain(bName)}: ${fmt(r.b, r.bText)}`">
          <div class="pbars-fill" :style="{ width: w(r.b), background: bColor }" />
          <span class="pbars-val strong">{{ fmt(r.b, r.bText) }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.pbars { font-size: 13px; }
.pbars-title { font-size: 13px; font-weight: 600; color: var(--ink); margin-bottom: 4px; }
.pbars-legend { display: flex; gap: 16px; font-size: 12px; color: var(--ink2); margin-bottom: 8px; }
.pbars-legend i { display: inline-block; width: 10px; height: 10px; border-radius: 2px; margin-right: 6px; vertical-align: -1px; }
.pbars-row { display: grid; align-items: center; gap: 10px; padding: 6px 0; border-bottom: 1px solid var(--rule); }
.pbars-row:last-child { border-bottom: none; }
.pbars-label { font-size: 13px; color: var(--ink); line-height: 1.3; }
.pbars-note { font-size: 11.5px; color: var(--ink3); margin-top: 2px; }
.pbars-bars { display: flex; flex-direction: column; gap: 2px; border-left: 1px solid var(--rule2); }
.pbars-line { display: flex; align-items: center; height: 16px; gap: 6px; }
.pbars-fill { height: 14px; border-radius: 0 4px 4px 0; }
.pbars-val { font-size: 12px; color: var(--ink2); white-space: nowrap; }
.pbars-val.strong { color: var(--ink); font-weight: 600; }
</style>
