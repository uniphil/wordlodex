<template>
  <div class="controls">
    <h1>Wordlodex±</h1>
    <div class="sizer">
      Game:
      <button
        v-if="state === 'welcome'"
        @click="$emit('change-size', -1)"
        :disabled="size <= minSize">
        -
      </button>
      {{ size }}x{{ size }}
      <button
        v-if="state === 'welcome'"
        @click="$emit('change-size', 1)"
        :disabled="size >= maxSize">
        +
      </button>
      <button
        v-if="state !== 'welcome'"
        @click="$emit('reset')"
        title="reset game">
        ↺
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    'state': String,
    'size': Number,
    'minSize': Number,
    'maxSize': Number,
  },
  data() {
    return {};
  },
};
</script>

<style scoped>
.controls {
  align-items: baseline;
  display: flex;
  flex-grow: 0;
  justify-content: space-between;
  padding: 0.25em 1em;
}
h1 {
  margin: 0;
}
</style>
