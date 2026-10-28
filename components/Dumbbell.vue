<script setup lang="ts">
// One-row dumbbell: root and leaf TPM (log scale)
import { computed } from 'vue'

const props = withDefaults(defineProps<{
  r: number
  l: number
  min?: number
  max?: number
  width?: number
}>(), { min: 0.1, max: 300, width: 170 })

const pad = 7
const x = (v: number) => {
  const a = Math.log10(props.min)
  const b = Math.log10(props.max)
  return pad + ((Math.log10(Math.max(v, props.min)) - a) / (b - a)) * (props.width - 2 * pad)
}
const ticks = computed(() => [0.1, 1, 10, 100].filter(t => t >= props.min && t <= props.max))
</script>

<template>
  <svg :viewBox="`0 0 ${width} 18`" :width="width" height="18" class="db" role="img">
    <line v-for="t in ticks" :key="t" :x1="x(t)" :x2="x(t)" y1="2" y2="16" stroke="#dcd7ee" stroke-width="1" />
    <line :x1="x(r)" :x2="x(l)" y1="9" y2="9" stroke="#c3bcdc" stroke-width="2.5" stroke-linecap="round" />
    <circle :cx="x(r)" cy="9" r="5" fill="#d4893a" stroke="#fff" stroke-width="1.5"><title>Root {{ r }} TPM</title></circle>
    <circle :cx="x(l)" cy="9" r="5" fill="#16958b" stroke="#fff" stroke-width="1.5"><title>Leaf {{ l }} TPM</title></circle>
  </svg>
</template>

<style scoped>
.db { display: block; }
</style>
