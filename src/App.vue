<template>
  <div id="app">
    <div id="game">
      <div id="messages">
        <div
          v-for="m in messages"
          :key="m.k"
          class="message">
          {{ m.message }}
        </div>
      </div>
      <div class="controls-sizer">
        <Controls
          :state="state"
          v-on:reset="resetGame"
          v-on:show-about="modal = 'about'"
        />
      </div>
      <p v-if="state === 'welcome'">
        Difficulty:
        <button
          @click.prevent="changeSize(-1)"
          :disabled="size <= minSize">
          -
        </button>
        <strong>{{ size }}</strong> letters
        <button
          @click.prevent="changeSize(1)"
          :disabled="size >= maxSize">
          +
        </button>
      </p>
      <p v-else-if="state === 'end'">
        <span v-if="loser">The word was <strong class="the-word">{{ word }}</strong>.</span>
        <span v-else>Nice work!</span>
        <span v-if="size > 1">
          <br/>Next game tomorrow.
        </span>
      </p>
      <div
        class="tiles-sizer"
        :style="{ aspectRatio: `${tiles[0].length} / ${tiles.length}` }">
        <Tiles :tiles="tiles" />
      </div>
      <div class="keyboard-sizer">
        <Keyboard
          :losts="losts"
          :founds="founds"
          :nopes="nopes"
          v-on:letter-press="handleLetterPress"
          v-on:backspace-press="handleBackspacePress"
          v-on:escape-press="handleEscapePress"
          v-on:enter-press="handleEnterPress"
          v-on:minus-press="changeSize(-1)"
          v-on:plus-press="changeSize(1)"
        />
      </div>
    </div>
    <Modal
      v-if="modal"
      v-on:close="modal = null">
      <About v-if="modal === 'about'" />
      <Hello v-if="modal === 'hello'" />
    </Modal>
  </div>
</template>

<script>
import About from './components/About';
import Controls from './components/Controls';
import Hello from './components/Hello';
import Keyboard from './components/Keyboard';
import Modal from './components/Modal';
import Tiles from './components/Tiles';

const wordSets = [
  null, // nothing for length zero
  {file: 'gwords-01.txt', selection: 3, guesses: 3},
  {file: 'gwords-02.txt', selection: 80, guesses: 5},
  {file: 'gwords-03.txt', selection: 600, guesses: 6},
  {file: 'gwords-04.txt', selection: 2222, guesses: 6},
  {file: 'gwords-05.txt', selection: 3000, guesses: 6},
  {file: 'gwords-06.txt', selection: 4000, guesses: 6},
  {file: 'gwords-07.txt', selection: 6000, guesses: 7},
  {file: 'gwords-08.txt', selection: 8400, guesses: 8},
  {file: 'gwords-09.txt', selection: 4000, guesses: 9},
  {file: 'gwords-10.txt', selection: 3600, guesses: 10},
  {file: 'gwords-11.txt', selection: 2400, guesses: 11},
  {file: 'gwords-12.txt', selection: 1200, guesses: 12},
  {file: 'gwords-13.txt', selection: 820, guesses: 13},
];

export default {
  name: 'App',
  components: {
    About,
    Controls,
    Hello,
    Keyboard,
    Modal,
    Tiles,
  },
  data: () => ({
    // game setup
    size: 5,
    minSize: 1,
    maxSize: 13,

    // data loading
    wordlists: [],

    // general notifications
    messages: [],
    modal: null,

    // game state
    state: 'welcome',
    word: null,
    entry: '',
    guesses: [],
    sizesPlayed: [],
  }),
  computed: {
    rows() {
      return wordSets[this.size].guesses;
    },
    tiles() {
      const guessTiles = this.guesses
        .map(g => Array.from(g)
          .map((letter, i) => ({
            type: 'guess',
            letter,
            outcome: letter === this.word[i]
              ? 'found'
              : Array.from(this.word).includes(letter) // TODO: double letter handling
                ? 'lost'
                : 'nope',
          })));
      if (guessTiles.length === this.rows) {
        return guessTiles;
      }
      return guessTiles
        .concat([Array.from({length: this.size})
          .map((_, i) => ({type: 'entry', letter: this.entry[i]}))])
        .concat(Array.from({length: this.rows - this.guesses.length - 1})
          .map(_ => Array.from({length: this.size})
            .map(_ => ({type: 'empty'}))));
    },
    founds() {
      if (this.guesses.length < 1) return [];
      const byGuess = this.guesses
        .map(guess => Array.from(guess)
          .filter((letter, i) => letter === this.word[i]));
      return [].concat.apply(...byGuess);
    },
    losts() {
      if (this.guesses.length < 1) return [];
      const wordLetters = Array.from(this.word);
      return Array.from(this.guesses.join(''))
        .filter(l => wordLetters.includes(l) && !this.founds.includes(l));
    },
    nopes() {
      if (this.guesses.length < 1) return [];
      const wordLetters = Array.from(this.word);
      return Array.from(this.guesses.join(''))
        .filter(l => !wordLetters.includes(l));
    },
    loser() {
      if (this.state === 'end' && !this.guesses.includes(this.word)) {
        return true;
      } else {
        return false;
      }
    },
  },
  mounted() {
    if (!localStorage.getItem('beenHereBefore')) {
      this.modal = 'hello';
      localStorage.setItem('beenHereBefore', true);
    }
  },
  methods: {
    handleLetterPress(key) {
      if (this.state === 'welcome') {
        this.startGame();
      } else if (this.state === 'end') {
        return;
      }
      if (this.entry.length < this.size) {
        this.entry += key;
      }
    },
    handleBackspacePress() {
      this.entry = this.entry.slice(0, -1);
      if (this.entry === '' && this.guesses.length === 0) {
        this.resetGame();
      }
    },
    handleEscapePress() {
      if (this.modal) {
        this.modal = null;
      } else {
        // this.endGame();
      }
    },
    handleEnterPress() {
      if (this.state !== 'playing') {
        return;
      }
      if (this.entry.length === this.size) {
        if (!this.wordlists[this.size].includes(this.entry)) {
          this.notify('Word not found :/');
          return;
        }

        const guess = this.entry;
        this.guesses.push(guess);
        if (guess === this.word) {
          this.endGame(true);
        } else if (this.guesses.length === this.rows) {
          this.endGame(false);
        }
        this.entry = '';
        this.sizesPlayed[this.size] = {
          word: this.word,
          guesses: [...this.guesses],
        };
      }
    },
    changeSize(changeBy) {
      if (this.state !== 'welcome') {
        return;
      }
      const nextSize = this.size + changeBy;
      if (nextSize < this.minSize || nextSize > this.maxSize) {
        return;
      }
      this.size = nextSize;
      this.resetGame();
    },
    notify(message) {
      this.messages.unshift({ message, k: '' + Math.random() });
      setTimeout(() => this.messages.pop(), 2000);
    },
    async startGame() {
      this.state = 'playing';

      const size = this.size;
      const loadState = this.wordlists[size];

      if (loadState === undefined) {
        this.wordlists[size] = 'loading';
        await this.loadWordsforGame(size);
      } else if (loadState === 'loading') {
        return;
      } else if (loadState === 'errored') {
        console.error('ohnooooooo');
      }

      if (this.word !== null) { return; }

      // we had an async break, check we're still working at the same game size
      if (size === this.size) {
        const list = this.wordlists[size];
        const set = wordSets[size];
        if (set.selection > list.length) {
          console.warn('Set selection reported longer than length, hmmmmmm');
        }
        if (this.size === 1) {
          this.word = list[Math.floor(Math.random() * set.selection)];
        } else {
          this.word = list[Math.floor(+new Date() / 86400000 - 52 * 365.25)];
        }
      }
    },
    resetGame() {
      this.state = 'welcome';
      this.entry = '';
      if (this.sizesPlayed[this.size]) {
        this.word = this.sizesPlayed[this.size].word;
        this.guesses = this.sizesPlayed[this.size].guesses;
      } else {
        this.word = null;
        this.guesses = [];
      }
    },
    endGame(winner) {
      if (this.state !== 'playing') {
        return;
      }
      if (winner) {
        this.notify('You won!');
      }
      this.state = 'end';
    },
    async loadWordsforGame(size) {
      const set = wordSets[size];
      if (!set) {
        console.error('missing wordset for size', size);
      }
      try {
        const res = await fetch(`static/${set.file}`);
        const rawWords = await res.text();
        const words = rawWords.split('\n').filter(w => w.length === size);
        if (words.length === 0) {
          console.error('words loaded, but none filtered. raw:', rawWords);
        }
        this.wordlists[size] = words;
      } catch (e) {
        this.wordlists[size] = 'errored';
      }
    },
  },
};
</script>

<style>
:root {
  --bg: #333;
  --fg: #ccc;
  --outline: hsla(0, 0%, 33%, 0.5);
  --outline-fg: hsla(0, 0%, 60%, 0.5);
  --okay: #c93;
  --good: #496;
  --tile-entry: #111;
  --tile-nope: var(--outline);
  --tile-lost: var(--okay);
  --tile-found: var(--good);
}

html, body {
  margin: 0;
  height: 100%;
}

body {
  background: var(--bg);
  color: var(--fg);
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
}

#app {
  height: 100%;
}
#game {
  align-items: center;
  background: #222;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  height: 100%;
  justify-content: space-between;
  max-width: 480px;
  margin: 0 auto;
  position: relative;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
#messages {
  position: absolute;
  top: 2em;
}
#messages .message {
  background: hsla(0, 0%, 0%, 0.5);
  border-radius: 0.5em;
  padding: 0.25em 0.5em;
  margin: 0.5em;
}
.controls-sizer {
  background: linear-gradient(hsla(200, 50%, 50%, 0.2), hsla(210, 50%, 50%, 0.1));
  flex-grow: 0;
  width: 100%;
}
.tiles-sizer {
  align-items: center;
  aspect-ratio: 1 / 1;
  display: flex;
  flex: 1 1 auto;
  max-width: 100%;
}
.keyboard-sizer {
  flex: 0 0 auto;
  width: 100%;
}
.the-word {
  text-transform: uppercase;
}

button {
  --bg-top: #555;
  --bg-bottom: hsla(0, 0%, 33.3%, 0.25);
  background: linear-gradient(var(--bg-top) 10%, var(--bg-bottom) 140%);
  border: 2px solid var(--outline);
  border-top-width: 1px;
  border-radius: 0.5em;
  box-sizing: border-box;
  color: var(--fg);
  font: inherit;
  font-weight: bold;
  padding: 0.15em 0.4em;
  text-align: center;
  text-shadow: 1px 1px 0 hsla(0, 0%, 0%, 0.5);
  text-transform: uppercase;
  touch-action: manipulation; /* try to prevent page zoom on double-button-tap */
}
button:hover {
  --bg-bottom: hsla(0, 0%, 33.3%, 0.667);
  --outline: #888;
}
button:active {
  --bg-bottom: hsla(0, 0%, 66.7%, 0.333);
  --outline: #666;
}
button:disabled,
button:disabled:hover,
button:disabled:active {
  --bg-top: var(--bg-bottom);
  --bg-bottom: hsla(0, 0%, 33.3%, 0.25);
  --outline: hsla(0, 0%, 33%, 0.5);
  opacity: 0.5;
  cursor: not-allowed;
}

a {
  color: inherit;
}

</style>
