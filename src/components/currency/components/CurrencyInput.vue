<template>
  <div class="currency__input">
    <input
      :value="amount"  
      @change="$emit('changeAmount', $event.target.value)"
      class="input"
      v-bind="$attrs"
    >
  </div>
</template>

<script>
export default {
  name: 'CurrencyInput',
  inheritAttrs: false,
  props: {
    amount: {
      type: String,
      required: true
    }
  },
  emits: ['changeAmount']
}
</script>

