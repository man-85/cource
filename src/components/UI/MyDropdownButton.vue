<template>
  <button
    class="btn btn-primary dropdown-toggle"
    type="button"
    data-mdb-toggle="dropdown"
    aria-expanded="false"
  >
    <slot></slot>
  </button>
  <ul class="dropdown-menu dropdown-menu-end" >
    <li v-for="option in options" :key="option.value">
      <a class="dropdown-item" href="#" @click="updateOption(option.value)">{{ option.name }}</a>
    </li>
  </ul>
</template>

<script>
export default {
  name: "my-dropdown-button",
  props: {
    modelValue: {
      type: String,
    },
    options: {
      type: Array,
      default: () => [],
    },
  },
  methods: {
    updateOption(option) {
      this.$emit("update:modelValue", option);
      this.$emit('updateOption', option);
    },
  },
};
</script>

<style>
</style>