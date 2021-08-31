<template>
  <button class="btn" :class="type">
    <slot />
  </button>
</template>

<script>
export default {
  name: "VButton",
  props: {
    type: {
      type: String,
    },
  },
};
</script>

<style lang="scss" scoped>
.btn {
  height: 32px;
  border: 1px solid black;
  border-radius: 5px;
  padding: 0 16px;
  display: flex;
  align-items: center;
  cursor: pointer;
  transition: background-color 0.2s linear;
  &:disabled {
    background-color: #bec5cc;
    pointer-events: none;
    box-shadow: none;
    border: none;
  }
  &:hover {
    background-color: black;
    color: white;
  }
}
</style>
