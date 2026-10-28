<script setup lang="ts">
// Single-value horizontal bars; each row can take its own colour (e.g. by tissue)
import { computed } from 'vue'

interface Row { label: string, v: number, text?: string, color?: string, sw?: string, note?: string, em?: boolean }

const props = withDefaults(defineProps<{
  rows: Row[]
  max?: number
  unit?: string
  title?: string
  color?: string
  labelWidth?: number
  barH?: number
}>(), {
  unit: '',
  color: '#7c6ce0',
  labelWidth: 120,
  barH: 16,
})

const m = computed(() => props.max ?? Math.max(...props.rows.map(r => r.v)))
const w = (v: number) => `calc((100% - 64px) * ${Math.max(v / m.value, 0)})`
// Labels, notes and the title may contain <i> for taxon names; tooltips use the plain text
const plain = (s: string) => s.replace(/<[^>]+>/g, '')
</script>

<template>
  <div class="hbar">
    <div v-if="title" class="hbar-title" v-html="title" />
    <div v-for="r in rows" :key="r.label" class="hbar-row" :style="{ gridTemplateColumns: `${labelWidth}px 1fr` }">
      <div class="hbar-label" :class="{ em: r.em }">
        <span v-if="r.sw" class="sw" :class="r.sw" /><span v-html="r.label" />
        <div v-if="r.note" class="hbar-note" v-html="r.note" />
      </div>
      <div class="hbar-line" :title="`${plain(r.label)}: ${r.text ?? r.v + unit}`">
        <div class="hbar-fill" :style="{ width: w(r.v), height: `${barH}px`, background: r.color ?? color }" />
        <span class="hbar-val" :class="{ em: r.em }">{{ r.text ?? `${r.v}${unit}` }}</span>
      </div>
    </div>
  </div>
</template>

<style scoped>
.hbar { font-size: 13px; }
.hbar-title { font-size: 13px; font-weight: 600; color: var(--ink); margin-bottom: 6px; }
.hbar-row { display: grid; align-items: center; gap: 10px; padding: 3px 0; }
.hbar-label { font-size: 13px; color: var(--ink); text-align: right; line-height: 1.3; }
.hbar-label.em { font-weight: 650; }
.hbar-note { font-size: 11px; color: var(--ink3); }
.hbar-line { display: flex; align-items: center; gap: 7px; border-left: 1px solid var(--rule2); min-height: 22px; }
.hbar-fill { border-radius: 0 4px 4px 0; }
.hbar-val { font-size: 12.5px; color: var(--ink2); white-space: nowrap; }
.hbar-val.em { color: var(--ink); font-weight: 650; }
</style>
