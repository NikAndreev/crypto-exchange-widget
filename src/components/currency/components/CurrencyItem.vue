<template>
  <li class="currency__dropdown-list-item" :data-ticker="currency.ticker">
    <img 
      class="currency__dropdown-list-item-img"
      :src="currency.image" 
      :alt="currency.ticker"
    >
    <span class="currency__dropdown-list-item-ticker">
      {{ currency.ticker }}
    </span>
    <span class="currency__dropdown-list-item-name">
      {{ currency.name }}
    </span>
  </li> 
</template>

<script>
export default {
  name: 'CurrencyItem',
  props: {
    currency: {
      type: Object,
      required: true
    }
  },
}
</script>

