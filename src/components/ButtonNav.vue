<template>
  <button class="start-button" @click="$emit('click')">
      <span class="mdi mdi-36px" :class="iconClass"></span>
  </button>
</template>

<script>
export default {
  props: {
    iconClass: {
      type: String,
      default: 'mdi-information-outline'
    }
  }
}
</script>

<style>

</style>