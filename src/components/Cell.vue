<template>
  <div
    class="column"
    :class="[tmpClass, isFocus && 'hover']"
  >
    <p>{{ data !== 0 ? data : '' }}</p>
  </div>
</template>
<script>
import { ref, watch } from 'vue';

export default {
  props: {
    data: {
      type: Number
    },
    column: {
      type: Number
    },
    row: {
      type: Number
    },
    isFocus: {
      type: Boolean
    },
  },
  setup(props) {
    const tmpClass = ref('');

    watch(() => props.data, (newValue, oldValue) => {
      if(newValue === 0) {
        tmpClass.value = 'green';
        setTimeout(() => {
          tmpClass.value = '';
        }, 250);
        return;
      }
      if(oldValue !== newValue) {
        tmpClass.value = 'yellow';
        setTimeout(() => {
          tmpClass.value = '';
        }, 250);
      }
    })

    return {
      tmpClass,
    }
  },
}
</script>

<style lang="scss" scoped>
.column {
  height: 25px;
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;

  transition: all 0.3s ease;
  cursor: pointer;
  font-weight: bold;
  font-size: 0.8rem;

  border: 1px solid blue;
  position: relative;

  p {
    margin: 0;
  }

  &.hover {
    background: rgba(253, 164, 164, 0.5);
  }
  &:hover {
    background: rgba(249, 78, 78, 0.7);
  }
  &.yellow {
    background: rgb(253, 241, 79);
  }
  &.green {
    background: rgb(24, 253, 24);
  }
}
</style>
