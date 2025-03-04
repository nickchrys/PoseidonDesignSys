<script setup>
import { computed } from 'vue';
import { useRouter } from 'vue-router';
import api from '../../../../src/assets/scripts/api.js';

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
  },
  offerId: {
    type: String,
    required: false
  },
  passId: {
    type: String,
    required: false
  }
})

const base = 'p-button';
const giveDesign = computed(() => {
  const design = props.design !== 'default' ? `${base}--${props.design}` : base;
  return [design, base]
})

const router = useRouter();

const confirmPurchase = async () => {
  alert('Purchase confirmed - Offer ID: ' + props.offerId + ' Passenger ID: ' + props.passId);

  return api.apiFetch('/flights/hold', {
    method: 'POST',
    credentials: 'include',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      offerID: props.offerId,
      passID: props.passId
    })
  }).then(
      response => console.log(response)
    ).then(
      router.push({ name: 'Event' })
    )
}

</script>

<template>
  <button v-if="giveDesign.includes('p-button--shop')" :class='giveDesign' @click="confirmPurchase">
    <p class="p-button--shop__label">{{ label }}</p>
    <p class="p-button--shop__price">${{ price }}</p>
  </button>
  <button v-else :class='giveDesign'>{{ label }}</button>
</template>
