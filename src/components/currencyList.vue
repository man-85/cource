<template>
  <div id="list">
    <currency-item
      v-for="currency in currencies"
      :currency="currency"
      :baseCurrency="baseCurrency"
      :key="currency.id"
    />
  </div>
</template>

<script>
import CurrencyItem from "@/components/currencyItem";
export default {
  components: {
    CurrencyItem,
  },
  props: {
    currencies: {
      type: Object,
      required: true,
    },
    baseCurrency: {
      type: String,
      default: 'RUB'
    },
  },
};
</script>

<style scoped>
#list {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
}
</style>