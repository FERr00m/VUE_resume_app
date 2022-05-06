<template>
  <button
    class="btn"
    :class="type"
    @click="$emit('action')"
  >
    <slot></slot>
  </button>
</template>

<script>
export default {
  name: 'AppButton',
  props: ['type'],
  emits: ['action']
}
</script>

<style scoped>

</style>
