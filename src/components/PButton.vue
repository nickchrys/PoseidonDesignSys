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
    validator: (value) => ['default', 'login', 'gradient', 'gradient-small', 'dropdown'].includes(value)
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
  <button :class='giveDesign'>{{ label }}</button>
</template>
