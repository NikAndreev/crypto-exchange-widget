<template>
  <div class="currency__dropdown-body">
    <ul 
      v-show="currencies.length"
      class="currency__dropdown-list"
      @click="changeCurrency"
    >
      <CurrencyItem 
        v-for="currency in currencies"
        :key="currency.ticker"
        :currency="currency"
      />
    </ul>
    <div 
      v-show="!currencies.length"
      class="currency__dropdown-empty"
    >
      No matches
    </div>
  </div>
</template>

<script>
import CurrencyItem from './CurrencyItem.vue'

export default {
  name: 'CurrencyList',
  props: {
    currencies: {
      type: Array,
      required: true
    }
  },
  emits: ['changeCurrency'],
  components: {
    CurrencyItem
  },
  methods: {
    changeCurrency(event) {
      const item = event.target.closest('[data-ticker]')

      if (item) {
        this.$emit('changeCurrency', this.currencies.find(currency => currency.ticker === item.dataset.ticker))
      }
    }
  }
}
</script>

