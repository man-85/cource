<template>
  <div class="card">
    <div class="card-body">
      <form @submit.prevent>
        <div class="input-group" style="justify-content: center;">
          <my-dropdown-button v-model="baseCurrency" :options="currencyOptions" v-on:updateOption="setBaseCurrency">Основная валюта</my-dropdown-button>
          <my-input v-model="searchQuery" @input="updateInput"/>
          <!-- <my-select v-model="selectedSort" :options="options" @change="setSort">Сортировка</my-select> -->
          <my-dropdown-button v-model="selectedSort" :options="sortOptions" v-on:updateOption="setSort">Сортировка</my-dropdown-button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import MyInput from "@/components/UI/MyInput";
import MySelect from "@/components/UI/MySelect";
import MyDropdownButton from "@/components/UI/MyDropdownButton";
export default {
  name: "search-form",
  components: {
    MyInput,
    MySelect,
    MyDropdownButton,
  },
  props: {
    searchQuery: {
      type: String
    },
    selectedSort: {
      type: String
    },
    baseCurrency: {
      type: String
    },
    sortOptions: {
      type: Array,
      default: () => []
    },
    currencyOptions: {
      type: Array,
      default: () => []
    },
  },
  methods: {
    updateInput() {
      this.$emit("query", this.searchQuery);
    },
    setSort() {
      this.$emit("sort", this.selectedSort);
    },
    setBaseCurrency() {
      this.$emit("baseCurrency", this.baseCurrency);
    }
  },
};
</script>

<style scoped>
form {
  display: flex;
  flex-direction: column;
}
</style>
