<template>
  <div class="card">
    <div class="card-body">
      <h5 class="card-title">{{ currency.Name }}</h5>
      <div>
        {{ currency.Nominal }}
        {{ currency.CharCode }}
        <i class="fa fa-arrows-h"></i>
        {{ currency.Value }}
        {{ baseCurrency }}
        <span
          class="rate"
          :class="[currency.Value > currency.Previous ? 'rateUp' : 'rateDown']"
          >{{ currencyGrowth }}</span
        >
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    currency: {
      type: Object,
      required: true,
    },
    baseCurrency: {
      type: String,
      default: "RUB",
    },
  },
  computed: {
    currencyGrowth() {
      return (this.currency.Value / this.currency.Previous).toFixed(4);
    },
  }
};
</script>

<style scoped>
.card {
  margin-top: 10px;
  width: 350px;
  text-align: left;
  margin-left: 10px;
}
.card-title {
  font-size: 8pt;
  color: grey;
  text-align: left;
}
.rate {
  float: right;
}
.rateDown:before {
  content: "\f175";
  border: none;
  font-family: "FontAwesome";
  margin-right: 3px;
}
.rateDown {
  color: red;
}
.rateUp:before {
  content: "\f176";
  border: none;
  font-family: "FontAwesome";
  margin-right: 3px;
}
.rateUp {
  color: #5dd55d;
}
</style>