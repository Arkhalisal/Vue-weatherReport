<script setup>
const props = defineProps({
  picId: {
    type: Number,
    required: true,
  },
});
</script>

<template>
  <div class="w-12 h-12">
    <img
      :src="`https://www.hko.gov.hk/images/HKOWxIconOutline/pic${props.picId}.png`"
      :alt="`Weather icon ${props.picId}`"
      class="w-full h-full object-contain"
    />
  </div>
</template>
