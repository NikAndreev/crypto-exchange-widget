<template>
  <div 
    class="currency__dropdown"
  >
    <CurrencyHeader 
      @close="$emit('close')"
      v-model.trim="filter"
    />
    <CurrencyList 
      :currencies="filteredCurrencies"
      @change-currency="changeCurrency"
    />
  </div>
</template>

<script>
import CurrencyHeader from './CurrencyHeader.vue'
import CurrencyList from './CurrencyList.vue'

export default {
  name: 'CurrencyDropdown',
  components: {
    CurrencyHeader,
    CurrencyList
  },
  props: {
    currencies: {
      type: Array,
      required: true
    }
  },
  emits: ['close', 'changeCurrency'],
  data() {
    return {
      filter: '',
    }
  },
  computed: {
    filteredCurrencies() {
      return this.currencies.filter(currency => currency.ticker.indexOf(this.filter) !== -1)
    }
  },
  methods: {
    changeCurrency(currency) {
      this.$emit('changeCurrency', currency)
      this.$emit('close')
    }
  }
  
}
</script>

