<template>
  <div class="form-outline" style="width: 400px;">
    <input type="search" id="typeText" class="form-control" :value="modelValue" @input="updateInput" placeholder="Введите наименование или код валюты"/>
    <label class="form-label" for="typeText">Поиск</label>
  </div>
</template>

<script>
export default {
  name: "my-input",
  props: {
    modelValue: [String, Number],
  },
  methods: {
    updateInput(event) {
      this.$emit("update:modelValue", event.target.value);
    },
  },
};
</script>

<style scoped>

</style>
