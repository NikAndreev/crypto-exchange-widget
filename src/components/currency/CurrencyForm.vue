<template>
  <div class="currency currency-group__currency" v-click-outside="close">
    <ErrorComponent
      v-show="showError"
      class="currency__error" 
    >
      The amount is too small
    </ErrorComponent>
    <div class="currency__inner">
      <CurrencyInput
        v-bind="$attrs" 
        :amount="amount"  
        @change-amount="$emit('changeAmount', $event)"
      />
      <CurrencyBtn 
        :currency="currency"
        @toggle="dropdownActive = !dropdownActive"
      />
    </div>
    <transition name="fade">
      <CurrencyDropdown
        v-show="dropdownActive" 
        :currencies="currencies"
        @close="close"
        @change-currency="$emit('changeCurrency', $event)"
      />
    </transition>
  </div>
</template>

<script>
import CurrencyDropdown from './components/CurrencyDropdown.vue'
import CurrencyInput from './components/CurrencyInput.vue'
import CurrencyBtn from './components/CurrencyBtn.vue'
import ErrorComponent from '../error/ErrorComponent.vue'

export default {
  name: 'CurrencyForm',
  inheritAttrs: false,
  components: {
    CurrencyDropdown,
    CurrencyInput,
    CurrencyBtn,
    ErrorComponent
  },
  props: {
    currencies: {
      type: Array,
      required: true
    },
    currency: {
      type: Object,
      required: true
    },
    amount: {
      type: String,
      required: true
    },
    showError: {
      type: Boolean,
      required: false,
      default: false
    }
  },
  emits: ['changeAmount', 'changeCurrency'],
  data() {
    return {
      dropdownActive: false
    }
  },
  methods: {
    close() {
      this.dropdownActive = false
    }
  }
}
</script>
