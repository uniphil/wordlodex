<template>
  <div class="screen" @click.prevent="$emit('close')">
    <div class="content" @click.stop="">
      <slot />
      <p class="right">
        <button @click.prevent="$emit('close')">
          Close
        </button>
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Modal',
  props: {},
};
</script>

<style scoped>
.screen {
  align-items: center;
  background: hsla(0, 0%, 20%, 0.5);
  bottom: 0;
  display: flex;
  justify-content: center;
  left: 0;
  position: absolute;
  right: 0;
  top: 0;
}
.content {
  background: hsl(0, 0%, 5%);
  box-shadow: 1px 4px 20px #000;
  box-sizing: border-box;
  margin: 0.5em 1em;
  max-height: calc(100% - 1em);
  max-width: 512px;
  overflow-y: auto;
  padding: 0.5em 1em;
  position: absolute;
}
.right {
  text-align: right;
}
</style>
