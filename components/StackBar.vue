<script setup lang="ts">
// 100% stacked bar; the legend gives counts and percentages
interface Seg { label: string, pct: number, n?: number, color: string, dark?: boolean }
const fmt = (n: number) => n.toLocaleString('en-US')
// Labels and the title may contain <i> for taxon names; tooltips use the plain text
const plain = (s: string) => s.replace(/<[^>]+>/g, '')

defineProps<{
  segs: Seg[]
  title?: string
}>()
</script>

<template>
  <div class="sbar">
    <div v-if="title" class="sbar-title" v-html="title" />
    <div class="sbar-track">
      <div
        v-for="s in segs" :key="s.label" class="sbar-seg"
        :style="{ flexBasis: `${s.pct}%`, background: s.color, color: s.dark ? '#fff' : '#1f1d2b' }"
        :title="`${plain(s.label)}: ${s.n !== undefined ? fmt(s.n) + ', ' : ''}${s.pct}%`"
      >
        <span v-if="s.pct >= 9">{{ s.pct }}%</span>
      </div>
    </div>
    <div class="sbar-legend">
      <span v-for="s in segs" :key="s.label">
        <i :style="{ background: s.color }" /><span v-html="s.label" /><b v-if="s.n !== undefined">{{ fmt(s.n) }}</b>&nbsp;({{ s.pct }}%)
      </span>
    </div>
  </div>
</template>

<style scoped>
.sbar-title { font-size: 13px; font-weight: 600; margin-bottom: 6px; }
.sbar-track { display: flex; gap: 2px; height: 24px; }
.sbar-seg { display: flex; align-items: center; justify-content: center; font-size: 12px; font-weight: 600; min-width: 4px; }
.sbar-seg:first-child { border-radius: 4px 0 0 4px; }
.sbar-seg:last-child { border-radius: 0 4px 4px 0; }
.sbar-legend { display: flex; flex-wrap: wrap; gap: 4px 16px; margin-top: 7px; font-size: 12px; color: var(--ink2); }
.sbar-legend i { display: inline-block; width: 10px; height: 10px; border-radius: 2px; margin-right: 5px; vertical-align: -1px; }
.sbar-legend b { font-weight: 600; color: var(--ink); margin-left: 5px; }
</style>
