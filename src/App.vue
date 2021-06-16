<template>
  <div id="app">
    <label>
      <p>Size:</p>
      <input type="number" v-model="size" class="size" min="1" max="20">
    </label>
    <label class="piramid">
      <p>Piramid:</p>
      <input type="checkbox" v-model="centered">
    </label>
    <hr>
    <div :class="['content', {centered: centered}]">
      <Triangle :size="size" :centered="centered" />
    </div>
  </div>
</template>

<script>
import Triangle from './components/Triangle.vue';

export default {
  name: 'App',
  components: {
    Triangle,
  },
  data() {
    return {
      size: 5,
      centered: false,
    };
  },
};
</script>

<style lang="scss">
#app {
  max-width: 1280px;
  margin: 0 auto;
    font-family: "Roboto", sans-serif;

  .size {
    width: 2.5rem;
    padding: 0.5rem 1rem;
    border-radius: 0.5rem;
    outline: none;
    border: 1px solid black;
  }

  .piramid {
    display: flex;
  }

  .content {
    width: max-content;

    &.centered {
      width: 100%;
    }
  }
}
</style>
