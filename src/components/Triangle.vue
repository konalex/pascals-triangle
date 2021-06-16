<template>
  <div :class="['triangle', {centered: centered}]">
    <p>Triangle:</p>
    <div v-for="(row, index) in elements" :key="index + Date.now()" class="row">
      <div
        v-for="(number, id) in row"
        :key="number + Date.now() + Math.random()"
        :class="['number', { active: number.active, current: current(id, index)}]"
        @mouseover="info(index, id)"
        @mouseleave="info(null, null)"
      >
        {{ number.number }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Triangle',
  data() {
    return {
      active: {
        row: null,
        id: null,
      },
    };
  },
  props: {
    size: {
      type: Number,
      default: 5,
    },
    centered: {
      type: Boolean,
      default: false,
    },
  },
  computed: {
    elements() {
      if (this.size === 0) return [];
      if (this.size === 1) return [[1]];
      const result = [];
      for (let row = 1; row <= this.size; row += 1) {
        const arr = [];
        for (let col = 0; col < row; col += 1) {
          const active = (row === this.active.row && col === this.active.id)
            || (row === this.active.row && col === this.active.id - 1);
          if (col === 0 || col === row - 1) {
            arr.push({
              number: 1,
              active,
            });
          } else {
            arr.push({
              number: (result[row - 2][col - 1].number + result[row - 2][col].number),
              active,
            });
          }
        }
        result.push(arr);
      }
      return result;
    },
  },
  methods: {
    info(index, id) {
      this.active.row = index;
      this.active.id = id;
    },
    current(id, index) {
      return id === this.active.id && index === this.active.row;
    },
  },
};
</script>

<style lang="scss" scoped>
$background-color: #212121;
$text-color: #FFFFFF;
$number-color: #0288D1;
$number-active-color: #455A64;
$number-current-color: #607D8B;

.triangle {
  width: 100%;
  margin: 0;

  .row {
    display: flex;

    .number {
      position: relative;
      display: flex;
      justify-content: center;
      align-items: center;
      width: 2rem;
      height: 2rem;
      padding: 0.5rem;
      margin: 1px;
      background-color: $number-color;
      color: $text-color;
      border-radius: 0.25rem;
      cursor: default;
      transition: all 150ms ease-in-out;

      &.active {
        background-color: $number-current-color;
      }

      &.current {
        background-color: $number-active-color;
      }
    }
  }

  &.centered {
    .row {
      justify-content: center;

      .number {
        margin: -0.3rem 0.1rem;
        padding: 0.65rem 0.5rem;
        clip-path: polygon(50% 0, 100% 25%, 100% 75%, 50% 100%, 0 75%, 0 25%);
      }
    }
  }
}
</style>
