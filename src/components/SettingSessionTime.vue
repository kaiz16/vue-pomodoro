<template>
  <div class="session-setting">
    <button @click="$emit('decrement')">
      <span class="mdi mdi-minus mdi-18px"></span>
    </button>
    <p>{{workTime / 60}}</p>
    <button @click="$emit('increment')">
      <span class="mdi mdi-plus mdi-18px"></span>
    </button>
  </div>
</template>

<script>
export default {
    props: {
        workTime: {
            type: Number
        }
    },
}
</script>
<style>
.session-setting{
  display: flex;
  flex-direction: row;
  justify-content: center;
  color: #fff
}
</style>