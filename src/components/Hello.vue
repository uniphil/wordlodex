<template>
  <div class="hello">
    <h1>Welcome!</h1>
    <p>This is a fan clone of the very lovely game <a href="https://www.powerlanguage.co.uk/wordle/" target="_blank">Wordle</a>, with <strong>alternate board sizes</strong>!</p>
    <p><strong>Use the <button>+</button> and <button>-</button> controls</strong> to play with words up to 12 letters long (and down to one-letter&hellip; it's about as fun as it sounds).</p>
    <p>Happy playing!</p>
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
.hello {}
</style>
