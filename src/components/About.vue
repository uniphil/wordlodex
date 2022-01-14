<template>
  <div class="about">
    <h1>About</h1>
    <p>This is a hacky clone of the very lovely game <a href="https://www.powerlanguage.co.uk/wordle/" target="_blank">Wordle</a>, with <strong>alternate board sizes</strong>.</p>
    <p><strong>Use the <button>+</button> and <button>-</button> controls</strong> to play with words up to 12 letters long (and down to one-letter&hellip; it's about as fun as it sounds).</p>
    <p>The words list are derived from the google n-grams dataset, roughly filtered to favour popular words.</p>
    <hr />
    <h3>See also</h3>
    <ul>
      <li><a href="https://www.powerlanguage.co.uk/wordle/" target="_blank">Wordle</a>, the original and best ❤</li>
      <li><a href="https://qntm.org/files/wordle/index.html" target="_blank">Absurdle</a>, a very creative adversarial version 😈</li>
      <li><a href="https://github.com/uniphil/wordlodex/" target="_blank">Source code</a> for this version</li>
    </ul>
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
