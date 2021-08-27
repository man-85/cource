<template>
  <div class="card">
    <div class="card-body">
      <h5 class="card-title">{{currencyName}}</h5>
      <div class="row">
        <div class="col-md-6"><my-select v-model="modelValue" :options="options" @change="changeOption"/></div>
        <div class="col-md-6"><input v-model="convertCount" @input="inputCount" class="cource" type="number" min="0.00" step="0.01" :readonly="isReadOnly" :disabled="isReadOnly" /></div>
      </div>
    </div>
  </div>
</template>

<script>
import MyInput from "@/components/UI/MyInput";
import MySelect from "@/components/UI/MySelect"
export default {
  name: "convert-corrency-item",
  components: {
    MyInput,
    MySelect,
  },
  props: {
    modelValue: {
      type: String,
    },
    convertCount: {
      type: String,
    },
    options: {
      type: Object,
      default: () => []
    },
    isReadOnly: Boolean,
  },
  data() {
    return {
      currencyName:"",
    }
  },
  methods: {
    changeOption(event) {
      this.$emit('update:modelValue', event.target.value);
      this.$emit('setCurrency', event.target.options[event.target.options.selectedIndex].text);
    },
    inputCount(event) {
      this.$emit('setCount', event.target.value);
    }
  },
   watch: {
    modelValue(value) {
      this.currencyName = this.options[value].Name;
    }
  }
};
</script>

<style scoped>
select {
  width: 100%;
}
.cource {
  border: 0;
  width: 100%;
  border-bottom: 1px solid gray;
}
.cource:focus {
  border: 0;
  border-bottom: 1px solid blue;
}
.cource:disabled {
      background: white;
    border-bottom: 1px solid lightgray;
}
.card {
  margin: 0px 10px;
  width: 350px;
  text-align: left;
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