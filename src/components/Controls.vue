<template>
  <div class="controls">
    <h1>Wordlodex±</h1>
    <a
      href="#"
      @click.prevent="$emit('show-about')">
      About
    </a>
    <div class="sizer">
      Game:
      <button
        v-if="state === 'welcome'"
        @click.prevent="$emit('change-size', -1)"
        :disabled="size <= minSize">
        -
      </button>
      {{ size }}x{{ rows }}
      <button
        v-if="state === 'welcome'"
        @click.prevent="$emit('change-size', 1)"
        :disabled="size >= maxSize">
        +
      </button>
      <button
        v-if="state !== 'welcome'"
        @click.prevent="$emit('reset')"
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
    'rows': Number,
    'minSize': Number,
    'maxSize': Number,
  },
  data() {
    return {};
  },
};
</script>

<style scoped>
.title {
  display: flex;
}
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
