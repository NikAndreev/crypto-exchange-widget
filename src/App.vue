<template>
  <div class="wrapper">
    <div class="container">
      <section class="widget">
        <div class="widget__inner">
          <h1 class="widget__title">
            Crypto Exchange
          </h1>
          <div class="widget__text">
            Exchange fast and easy
            <ErrorComponent
              v-show="error.isError"
              class="widget__text-error"
            >
              {{ error.message }}
            </ErrorComponent>
          </div>
          <div class="currency-group widget__currency-group">
            <CurrencyForm 
              type="number"
              :min="minAmount"
              step="0.00000001"
              :disabled="isPending"
              :currencies="currencies"
              :currency="from"
              :amount="amount.from"
              :show-error="!isValid"
              @change-amount="changeAmount"
              @change-currency="changeСurrency('from', $event)"
            />
            <div class="currency-group__swap"></div>
            <CurrencyForm 
              type="text"
              :disabled="true"
              :currencies="currencies"
              :currency="to"
              :amount="amount.to"
              @change-currency="changeСurrency('to', $event)"
            />
          </div>
          <ExchangeForm />
        </div>
      </section>
    </div>
    <PreloaderComponent
      v-show="isPending" 
    />
  </div>
</template>

<script>
import axios from 'axios'

import CurrencyForm from './components/currency/CurrencyForm.vue'
import ExchangeForm from './components/exchange/ExchangeForm.vue'
import PreloaderComponent from './components/preloader/PreloaderComponent.vue'
import ErrorComponent from './components/error/ErrorComponent.vue'

export default {
  name: 'App',
  components: {
    CurrencyForm,
    ExchangeForm,
    PreloaderComponent,
    ErrorComponent
  },
  data() {
    return {
      APIKEY: 'c9155859d90d239f909d2906233816b26cd8cf5ede44702d422667672b58b0cd',
      currencies: [],
      from: {},
      to: {},
      amount: {
        from: '',
        to: ''
      },
      minAmount: 0,
      isPending: true,
      error: {
        isError: false,
        message: '',
        dictionary: {
          pair_is_inactive: 'Pair is inactive',
          deposit_too_small: 'Out of min amount'
        }
      },
    }
  },
  computed: {
    isValid() {
      return Number(this.amount.from) >= this.minAmount
    }
  },
  methods: {
    http(config) {
      this.isPending = true

      return new Promise((resolve, reject) => {
        axios(config)
          .then(res => {
            this.removeError()
            resolve(res)
          })
          .catch(err => {
            this.doError(err)
            reject(err)
          })
          .finally(() => this.isPending = false)
      })
    },
    getCurrencies() {
      return new Promise((resolve, reject) => {
        this.http({
          method: 'get',
          url: `https://api.changenow.io/v1/currencies`
        })
          .then(res => {
            this.currencies = res.data
            resolve()
          })
          .catch(() => reject())
      })
    },
    getMinimalAmount() {
      return new Promise((resolve, reject) => {
        this.http({
          method: 'get',
          url: `https://api.changenow.io/v1/min-amount/${this.from.ticker}_${this.to.ticker}?api_key=${this.APIKEY}`
        })
          .then(res => {
            this.minAmount = res.data.minAmount
            this.amount.from = String(this.minAmount)
            resolve()
          })
          .catch(() => reject())
      })
    },
    getExchangeAmount() {
      return new Promise((resolve, reject) => {
        this.http({
          method: 'get',
          url: `https://api.changenow.io/v1/exchange-amount/${this.amount.from}/${this.from.ticker}_${this.to.ticker}?api_key=${this.APIKEY}`
        })
        .then(res => {
          this.amount.to = String(res.data.estimatedAmount)
          resolve()
        })
        .catch(() => reject())
      })  
    },
    changeAmount(amount) {
      this.amount.from = amount

      if (this.isValid) {
        this.getExchangeAmount()
      }
    },
    changeСurrency(direction, currency) {
      this[direction] = currency

      this.getMinimalAmount()
        .then(this.getExchangeAmount)
    },
    doError(error) {
      this.error.isError = true
      this.error.message = error.response.data?.message || this.error.dictionary[error.response.data?.error] || error.message
    },
    removeError() {
      this.error.isError = false
      this.error.message = ''
    },
  },
  watch: {
    isValid(valid) {
      if (!valid) {
        this.amount.to = '—'
      }
    },
    'error.isError'(error) {
      if (error) {
        this.amount.to = '—'
      }
    }
  },
  mounted() {
    this.getCurrencies()
      .then(() => {
        this.from = this.currencies[0]
        this.to = this.currencies[1]

        this.getMinimalAmount()
          .then(this.getExchangeAmount)
      })
  }
}
</script>
