<template>
  <div class="keyboard">
    <div class="row">
      <button
        :key="letter"
        :class="{
          key: true,
          lost: losts.includes(letter),
          found: founds.includes(letter),
          nope: nopes.includes(letter),
        }"
        @click.prevent="$emit('letter-press', letter)"
        v-for="letter in 'qwertyuiop'">
        {{ letter }}
      </button>
    </div>
    <div class="row">
      <button
        :key="letter"
        :class="{
          key: true,
          lost: losts.includes(letter),
          found: founds.includes(letter),
          nope: nopes.includes(letter),
        }"
        @click.prevent="$emit('letter-press', letter)"
        v-for="letter in 'asdfghjkl'">
        {{ letter }}
      </button>
    </div>
    <div class="row">
      <button
        class="key command"
        @click.prevent="$emit('enter-press')">
        enter
      </button>
      <button
        :key="letter"
        :class="{
          key: true,
          lost: losts.includes(letter),
          found: founds.includes(letter),
          nope: nopes.includes(letter),
        }"
        @click.prevent="$emit('letter-press', letter)"
        v-for="letter in 'zxcvbnm'">
        {{ letter }}
      </button>
      <button
        class="key command"
        @click.prevent="$emit('backspace-press')"
        title="backspace">
        ⌫
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    losts: Array,
    founds: Array,
    nopes: Array,
  },
  mounted() {
    window.addEventListener('keyup', this.handleKeyUp);
  },
  destroyed() {
    window.removeEventListener('keyup', this.handleKeyUp);
  },
  methods: {
    handleKeyUp(e) {
      const k = e.key.toLowerCase();
      if (k === 'enter') {
        this.$emit('enter-press');
      } else if (k === 'escape') {
        this.$emit('escape-press');
      } else if (k === 'backspace') {
        this.$emit('backspace-press');
      } else if ('abcdefghijklmnopqrstuvwxyz'.includes(k)) {
        this.$emit('letter-press', k);
      }
    },
  },
  data() {
    return {};
  },
};
</script>

<style scoped>
.keyboard {
  box-sizing: border-box;
  padding: 0.25em;
}
.row {
  flex: 1 1 auto;
  justify-content: center;
  display: flex;
}
.key {
  aspect-ratio: 12 / 13;
  padding: 0;
  width: 9.333%;
  margin: 0.333%;
}
.key.lost {
  --bg-top: var(--okay);
}
.key.found {
  --bg-top: var(--good);
}
.key.nope {
  --bg-top: var(--bg-bottom);
  opacity: 0.5;
}
.key.command {
  aspect-ratio: 18 / 13;
  width: 14%;
}
</style>
