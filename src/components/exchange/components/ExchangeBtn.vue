<template>
  <div class="address__btn">
    <button 
      v-bind="$attrs"
      class="btn btn_color_blue"
    >
      Exchange
    </button>
  </div>
</template>

<script>
export default {
  name: 'ExchangeBtn',
  inheritAttrs: false,
}
</script>
