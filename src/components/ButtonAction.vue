<template>
  <button class="action-button" @click="$emit('click')">
      <span class="mdi mdi-36px" :class="iconClass"></span>
  </button>
</template>

<script>
export default {
  props: {
    iconClass: {
      type: String,
      default: 'mdi-play-circle-outline'
    }
  }
}
</script>

<style>

</style>