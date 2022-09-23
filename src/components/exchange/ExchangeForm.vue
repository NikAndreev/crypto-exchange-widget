<template>
  <div class="address">
    <div class="address__label">
      Your Ethereum address
    </div>
    <div class="address__row">
      <ExchangeInput />
      <ExchangeBtn />
    </div>
  </div>
</template>

<script>
import ExchangeInput from './components/ExchangeInput.vue'
import ExchangeBtn from './components/ExchangeBtn.vue'

export default {
  name: 'ExchangeForm',
  components: {
    ExchangeInput,
    ExchangeBtn
  }
}
</script>
