<script setup lang="ts">
// Figure page: the figure fills one column from the top of the page to the footer; title, key message and text sit in the other column.
// Frontmatter: side (left / right, default right) and figw (width of the figure column). Text comes first; the figure goes after ::fig::.
import { computed } from 'vue'
import { useSlideContext } from '@slidev/client'

defineProps<{
  kicker?: string
  supp?: string
  source?: string
  side?: 'left' | 'right'
  figw?: string
  figpos?: string
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
  <div
    class="slidev-layout pg fp"
    :class="side === 'left' ? 'fp-l' : 'fp-r'"
    :style="{ '--fp-w': figw ?? '50%', '--fp-pos': figpos ?? 'center' }"
  >
    <div class="fp-main">
      <div class="pg-top">
        <span class="pg-no">{{ no }}</span>
        <span v-if="kicker" class="pg-kicker">{{ kicker }}</span>
        <span v-if="supp" class="pg-supp">{{ L.supp }} {{ supp }}</span>
      </div>
      <div class="pg-body">
        <slot />
      </div>
    </div>
    <div class="fp-fig">
      <slot name="fig" />
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
