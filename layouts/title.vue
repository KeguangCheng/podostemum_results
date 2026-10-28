<script setup lang="ts">
// Cover: the nine libraries on the right (root and leaf split from the same plants; whole plant in another sequencing batch)
const rows = [
  { k: 'r', name: 'Root', note: 'root part of the plant', libs: ['PR1', 'PR2', 'PR3'] },
  { k: 'l', name: 'Leaf', note: 'all parts remaining after root removal', libs: ['PL1', 'PL2', 'PL3'] },
  { k: 'w', name: 'Whole plant', note: 'root and remaining parts together', libs: ['P1', 'P2', 'P3'] },
]
</script>

<template>
  <div class="slidev-layout ttl">
    <div class="ttl-main">
      <div class="ttl-kicker">
        <span class="triad"><i class="r" /><i class="l" /><i class="w" /></span>
        PROGRESS REPORT
      </div>
      <slot />
    </div>
    <div class="ttl-side">
      <div class="lib-title">NINE LIBRARIES</div>
      <template v-for="(row, i) in rows" :key="row.k">
        <div v-if="i === 2" class="lib-split">
          <span>different sequencing batch</span>
        </div>
        <div class="lib-row">
          <div class="lib-name">
            <b>{{ row.name }}</b>
            <span>{{ row.note }}</span>
          </div>
          <div class="lib-tiles">
            <div v-for="l in row.libs" :key="l" class="lib-tile" :class="row.k">{{ l }}</div>
          </div>
        </div>
        <div v-if="i === 0" class="lib-pair">Split from the same plants · pairing of library numbers to be confirmed</div>
      </template>
    </div>
  </div>
</template>

<style scoped>
.ttl {
  height: 100%;
  display: grid;
  grid-template-columns: 1fr 380px;
  gap: 56px;
  padding: 70px 64px 56px 72px;
  align-items: center;
}

.ttl-kicker {
  display: flex;
  align-items: center;
  gap: 14px;
  font-size: 15px;
  font-weight: 600;
  letter-spacing: 0.12em;
  color: var(--accent);
  margin-bottom: 26px;
}

.ttl-kicker .triad i {
  width: 22px;
  height: 5px;
}

.ttl-main :deep(h1) {
  font-size: 44px;
  line-height: 1.2;
  font-weight: 700;
  letter-spacing: -0.01em;
  color: var(--ink);
  margin: 0 0 14px;
}

.ttl-main :deep(h1 + p),
.ttl-main :deep(.sub) {
  opacity: 1;
  font-size: 21px;
  line-height: 1.5;
  color: var(--ink2);
  margin: 0 0 34px;
}

.ttl-main :deep(.byline) {
  margin-top: 6px;
  padding-top: 22px;
  border-top: 1px solid var(--rule2);
  font-size: 20px;
  font-weight: 600;
  color: var(--ink);
  letter-spacing: 0.01em;
}

.ttl-main :deep(.date) {
  margin-top: 8px;
  font-size: 16px;
  color: var(--ink2);
}

.ttl-side {
  border-left: 1px solid var(--rule2);
  padding-left: 40px;
}

.lib-title {
  font-size: 12.5px;
  font-weight: 650;
  letter-spacing: 0.1em;
  color: var(--ink3);
  margin-bottom: 14px;
}

.lib-row {
  display: grid;
  grid-template-columns: 1fr;
  gap: 6px;
}

.lib-name {
  display: flex;
  align-items: baseline;
  gap: 8px;
  font-size: 14px;
}

.lib-name b {
  font-weight: 650;
}

.lib-name span {
  font-size: 12px;
  color: var(--ink3);
}

.lib-tiles {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 6px;
}

.lib-tile {
  height: 54px;
  border-radius: 6px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 600;
  font-size: 15px;
  color: var(--ink);
}

.lib-tile.r { background: var(--root); }
.lib-tile.l { background: var(--leaf); }
.lib-tile.w { background: var(--whole); }

.lib-pair {
  font-size: 12px;
  color: var(--ink2);
  margin: 8px 0 10px;
  padding-left: 12px;
  border-left: 2px solid var(--rule2);
}

.lib-split {
  position: relative;
  margin: 18px 0 14px;
  border-top: 1.5px dashed var(--rule2);
  height: 0;
}

.lib-split span {
  position: absolute;
  right: 0;
  top: -9px;
  background: #fff;
  padding-left: 8px;
  font-size: 12px;
  color: var(--ink2);
}
</style>
