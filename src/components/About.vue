<template>
  <div class="about">
    <h1>About</h1>
    <p>This is a hacky clone of the very lovely game <a href="https://www.powerlanguage.co.uk/wordle/" target="_blank">Wordle</a>, with <strong>alternate board sizes</strong>.</p>
    <p>Use the controls in the top-right corner to play with words up to 12 letters long (and down to one-letter words&hellip; it's about as fun as it sounds).</p>
    <p>The words list are derived from the google n-grams dataset, roughly filtered to favour popular words.</p>
    <hr />
    <h3>See also</h3>
    <ul>
      <li><a href="https://www.powerlanguage.co.uk/wordle/" target="_blank">Wordle</a>, the original and best ❤</li>
      <li><a href="https://qntm.org/files/wordle/index.html" target="_blank">Absurdle</a>, a very creative adversarial version 😈</li>
      <li><a href="https://github.com/uniphil/wordlodex/" target="_blank">Source code</a> for this version</li>
    </ul>
    <p class="right">
      <button @click.prevent="$emit('close')">
        Close
      </button>
    </p>
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
.about {
  background: hsl(0, 0%, 5%);
  /*border: 3px solid #222;*/
  box-shadow: 1px 4px 20px #000;
  padding: 0.5em 1em;
}
.right {
  text-align: right;
}

</style>
