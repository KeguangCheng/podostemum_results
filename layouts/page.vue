<script setup lang="ts">
// Content page: slide number (assigned automatically in slide order) and section at top left,
// Supplementary Data section at top right, and the matching section and table numbers of the Report at the bottom.
// The fixed page text can be overridden with `labels` in the headmatter of slides.md (see README).
import { computed } from 'vue'
import { useSlideContext } from '@slidev/client'

defineProps<{
  kicker?: string
  supp?: string
  source?: string
}>()

const { $page, $slidev } = useSlideContext()
const no = computed(() => String($page.value).padStart(2, '0'))
const L = computed(() => ({
  source: 'Source',
  report: 'Report',
  supp: 'Supplement',
  mark: '<i>Podostemum ceratophyllum</i> transcriptome · progress results',
  ...(($slidev?.configs as any)?.labels ?? {}),
}))
</script>

<template>
  <div class="slidev-layout pg">
    <div class="pg-top">
      <span class="pg-no">{{ no }}</span>
      <span v-if="kicker" class="pg-kicker">{{ kicker }}</span>
      <span v-if="supp" class="pg-supp">{{ L.supp }} {{ supp }}</span>
    </div>
    <div class="pg-body">
      <slot />
    </div>
    <div class="pg-foot">
      <span v-if="source" class="pg-src"><b>{{ L.source }}</b>{{ L.report }} {{ source }}</span>
      <span class="pg-mark">
        <span v-html="L.mark" />
        <span class="triad"><i class="r" /><i class="l" /><i class="w" /></span>
      </span>
    </div>
  </div>
</template>
