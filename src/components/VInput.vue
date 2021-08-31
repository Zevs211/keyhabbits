<template>
  <div class="input">
    <div class="input__container" :class="[{ danger: isNotValid }]">
      <input :placeholder="inputLabel" :value="value" @input="onInput" />
    </div>
    <div class="remark" v-if="isNotValid">Это имя уже задано</div>
  </div>
</template>

<script>
export default {
  name: "VInput",
  props: {
    required: {
      type: Boolean,
      default: false,
    },
    value: {},
    inputLabel: {
      type: String,
      default: false,
    },
    isNotValid: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    onInput(event) {
      this.$emit("input", event.target.value);
    },
  },
};
</script>

<style lang="scss" scoped>
.input {
  color: #000000;
  &__container {
    width: 200px;
    height: 32px;
    padding: 0 8px;
    border: 1px solid black;
    border-radius: 5px;

    input {
      outline: none;
      border: none;
      width: 100%;
      height: 30px;
    }
  }
}
.danger {
  border-color: #ea0029;
}
.remark {
  margin-top: 4px;
  font-style: normal;
  font-weight: normal;
  font-size: 10px;
  line-height: 12px;

  color: #ea0029;
}
</style>
