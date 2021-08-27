<template>
  <!-- Tabs navs -->
  <ul class="nav nav-tabs mb-3" id="nav" role="tablist">
    <li class="nav-item" role="presentation">
      <a
        class="nav-link active"
        id="nav-tab-1"
        data-mdb-toggle="tab"
        href="#nav-tabs-1"
        role="tab"
        aria-controls="nav-tabs-1"
        aria-selected="true"
        >Список валют</a
      >
    </li>
    <li class="nav-item" role="presentation">
      <a
        class="nav-link"
        id="nav-tab-2"
        data-mdb-toggle="tab"
        href="#nav-tabs-2"
        role="tab"
        aria-controls="nav-tabs-2"
        aria-selected="false"
        >Конвертор валют</a
      >
    </li>
  </ul>
  <!-- Tabs navs -->

  <!-- Tabs content -->
  <div class="tab-content" id="nav-content">
    <div
      class="tab-pane fade show active"
      id="nav-tabs-1"
      role="tabpanel"
      aria-labelledby="nav-tab-1"
    >
      <search-form
        :sortOptions="sortOptions"
        :currencyOptions="currencyOptions"
        :baseCurrency="baseCurrncy"
        :selectedSort="selectedSort"
        @query="setQuery"
        @sort="setSort"
        @baseCurrency="setBaseCurrency"
      />
      <currency-list
        :currencies="sortedAndSearchedCurrencies"
        :baseCurrency="baseCurrency"
        v-if="!isCurrenciesLoading"
      />
      <div v-else>Идет загрузка...</div>
    </div>
    <div
      class="tab-pane fade"
      id="nav-tabs-2"
      role="tabpanel"
      aria-labelledby="nav-tab-2"
    >
      <div id="converter" class="row">
        <converter-currency-item v-model="convertFrom" :options="currencies" @setCount="calculateСourse" @setCurrency="recalculateСourse"/>
        <button type="button" class="btn btn-primary btn-floating" @click="changeConvertCurrency">
          <i class="fa fa-arrows-h"></i>
        </button>
        <converter-currency-item v-model="convertTo" :convertCount="convertPrice" :options="currencies" :isReadOnly="true" @setCurrency="recalculateСourse"/>
      </div>
    </div>
  </div>
  <!-- Tabs content -->
</template>

<script>
import CurrencyList from "@/components/currencyList";
import SearchForm from "@/components/searchForm";
import ConverterCurrencyItem from '@/components/converterCurrencyItem';
import axios from "axios";

export default {
  components: {
    CurrencyList,
    SearchForm,
    ConverterCurrencyItem
  },
  data() {
    return {
      currencies: [],
      isCurrenciesLoading: false,
      baseCurrency: "RUB",
      currencyOptions: [
        { name: "Евро", value: "EUR"},
        { name: "Рубль", value: "RUB"},
      ],
      searchQuery: "",
      selectedSort: "",
      sortOptions: [
        { value: "Name", name: "По наименованию" },
        { value: "CharCode", name: "По коду"},
        { value: "Value", name: "По стоимости" },
        { value: "Previous", name: "По росту"},
      ],
      convertFrom:"",
      convertTo:"",
      convertCount:0,
      convertPrice:0
    };
  },
  filters: {
    currencydecimal(value) {
      return value.toFixed(4);
    },
  },
  methods: {
    calculateСourse(count) {
      this.convertCount = count;
      if (this.convertFrom != "" && this.convertTo != "") {
        let convertFromPrice = this.currencies[this.convertFrom].Value;
        let convertToPrice = this.currencies[this.convertTo].Value;
        this.convertPrice = ((convertFromPrice/convertToPrice)*this.convertCount).toFixed(2);
      }
    },
    recalculateСourse(){
      this.calculateСourse(this.convertCount);
    },
    changeConvertCurrency() {
      let currency = this.convertFrom;
      this.convertFrom = this.convertTo;
      this.convertTo = currency;
      this.calculateСourse(this.convertCount);
    },
    setQuery(text){
      this.searchQuery = text;
    },
    setSort(item){
      this.selectedSort = item;
    },
    setBaseCurrency(item){
      this.baseCurrency = item;
    },
    async fetchCurrences() {
      // axios
      //   .get("https://www.cbr-xml-daily.ru/daily_json.js")
      //   .then((response) => (this.currencies = response.data.Valute));
      try {
        this.isCurrenciesLoading = true;
        const response = await axios.get(
          "https://www.cbr-xml-daily.ru/daily_json.js"
        );
        this.currencies = response.data.Valute;
        this.currentCurrency = response.data.Valute["EUR"].Value;
      } catch (e) {
        alert("Ошибка");
      } finally {
        this.isCurrenciesLoading = false;
      }
    },
  },
  mounted() {
    this.fetchCurrences();
  },
  computed: {
    sortedCurrencies() {
      return [...Object.values(this.currencies)].sort((currency1, currency2) =>
        currency1[this.selectedSort] > currency2[this.selectedSort] ? 1 : -1
      );
    },
    sortedAndSearchedCurrencies() {
      return this.sortedCurrencies.filter(currency => currency.Name.toLowerCase().includes(this.searchQuery.toLowerCase()) || currency.CharCode.toLowerCase().includes(this.searchQuery.toLowerCase()))
    },
  },
  watch: {
    baseCurrency(value) {
      let price = this.currentCurrency;
      [...this.sortedCurrencies].map(function(currency){
        if(value != 'RUB'){ 
          currency.Value = (currency.Value / price).toFixed(4);
          currency.Previous = (currency.Previous / price).toFixed(4);
        } else {
          currency.Value = (currency.Value * price).toFixed(4);
          currency.Previous = (currency.Previous * price).toFixed(4);
        }
        return currency;
      });
    }
  },
};
</script>

<style>
body {
  background: #2697ff;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  padding: 15px;
  background: lightgrey;
  margin: 15px;
}

#nav {
  background: white;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}

#converter {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
