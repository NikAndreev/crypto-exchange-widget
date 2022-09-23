<template>
  <div 
    class="currency__btn" 
    role="button" 
    @click="$emit('toggle')"
  >
    <img 
      :src="currency.image" 
      :alt="currency.ticker"
    >
    {{ currency.ticker }}
  </div>
</template>

<script>
export default {
  name: 'CurrencyBtn',
  props: {
    currency: {
      type: Object,
      required: true
    }
  },
  emits: ['toggle'],
  
}
</script>

