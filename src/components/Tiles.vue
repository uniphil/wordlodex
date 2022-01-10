<template>
  <div class="tiles">
    <div
      :key="`tile-${i}`"
      class="row"
      v-for="(row, i) in tiles">
      <div
        :key="`tile-${i}-${j}`"
        :class="{
          tile: true,
          guess: t.type === 'guess',
          entry: t.type === 'entry',
          nope: t.type === 'guess' && t.outcome === 'nope',
          lost: t.type === 'guess' && t.outcome === 'lost',
          found: t.type === 'guess' && t.outcome === 'found',
        }"
        v-for="(t, j) in row">
        {{ t.letter }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    tiles: Array,
    losts: Array,
    founds: Array,
    nopes: Array,
  },
  data() {
    return {};
  },
};
</script>

<style scoped>
.tiles {
  aspect-ratio: 1 / 1;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  font-size: 1.5em;
  justify-content: center;
  padding: 0.5em;
  width: 100%;
}
.row {
  --border: 2px;
  --margin: 3px;
  --max-side: 3em;
  display: flex;
  flex: 1 1 auto;
  justify-content: center;
  max-height: calc(var(--max-side) + 2*var(--margin));
}
.tile {
  align-items: center;
  aspect-ratio: 1 / 1;
  background: linear-gradient(-19deg, #333, #222);
  box-sizing: border-box;
  border: var(--border) solid var(--outline);
  border-radius: 2px;
  display: flex;
  flex: 1 1 auto;
  font-weight: bold;
  justify-content: center;
  margin: var(--margin);
  max-width: var(--max-side);
  padding-top: 0.2em;  /* nicer vertical centring */
  text-transform: uppercase;
}
.tile.guess {
  border-width: 0;
}
.tile.entry {
  /*background: var(--tile-entry);*/
}
.tile.nope {
  background: var(--tile-nope);
}
.tile.lost {
  background: var(--tile-lost);
}
.tile.found {
  background: var(--tile-found);
}
</style>
