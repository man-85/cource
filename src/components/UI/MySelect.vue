<template>
  <select class="btn btn-light" v-model="modelValue" @change="changeOption">
    <!-- <option disabled value="">Выберите из списка</option> -->
    <option
      v-for="option in options"
      :key="option.CharCode"
      :value="option.CharCode"
    >
      {{ option.CharCode }}
    </option>
  </select>
</template>

<script>
export default {
  name: 'my-select',
  props: {
    modelValue: String,
    options: {
      type: Array,
      default: () => []
    },
  },
  methods: {
    changeOption(event) {
      this.$emit('update:modelValue', event.target.value);
    },
  },
}
</script>

<style scoped>
</style>