<script setup>
//const emit = defineEmits(['next', 'prev']) //si no ponemos esta linea, hay que poner en el emit del @click el signo $

defineProps(['inicio', 'fin', 'maxlength'])
</script>


<template>
  <div class="btn-group" role="group" aria-label="Basic example">
    <button
      @click="$emit('prev')"
      type="button"
      class="btn btn-outline-primary"
      :disabled="inicio == 0"
    >
      Previous
    </button>
    <button
      @click="$emit('next')"
      type="button"
      class="btn btn-outline-primary"
      :disabled="fin == maxlength"
    >
      Next
    </button>
  </div>
</template>