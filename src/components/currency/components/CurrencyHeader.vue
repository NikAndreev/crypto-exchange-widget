<template>
  <div class="currency__dropdown-header">
    <input 
      :value="modelValue"  
      @input="$emit('update:modelValue', $event.target.value)"
      type="text" 
      class="input currency__dropdown-input" 
      placeholder="Search"
      v-bind="$attrs"
    >
    <span class="currency__dropdown-close" role="button" @click="$emit('close')">
    </span>
  </div>
</template>

<script>

export default {
  name: 'CurrencyHeader',
  inheritAttrs: false,
  props: {
    modelValue: {
      type: String,
      required: true
    }
  },
  emits: ['close', 'update:modelValue'],
}
</script>

