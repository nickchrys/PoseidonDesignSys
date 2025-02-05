<script setup>
import { computed } from 'vue';

/**
 * @component PButton
 * @description A reusable button component
 */

const props = defineProps({
  label: {
    type: String,
    required: true
  },
  design: {
    type: String,
    default: 'default',
    validator: (value) => ['default', 'login', 'gradient', 'gradient-small', 'dropdown', 'shop'].includes(value)
  },
  price: {
    type: Number,
    default: 0,
    required: false
  },
  class: {
    type: String,
    default: '',
  }
  })

  const base = 'p-button';
  const giveDesign = computed(() => {
    const design = props.design !== 'default' ? `${base}--${props.design}` : base;
    return [design, base]
  })


</script>

<template>
  <button v-if="giveDesign.includes('p-button--shop')" :class='giveDesign'>{{ label }} <br> {{ price }}</button>
  <button v-else :class='giveDesign'>{{ label }}</button>
</template>
