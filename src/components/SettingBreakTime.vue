<template>
<div class="break-setting">
    <button @click="$emit('decrement')">
        <span class="mdi mdi-minus mdi-18px"></span>
      </button>
      <p>{{breakTime / 60}}</p>
      <button @click="$emit('increment')">
        <span class="mdi mdi-plus mdi-18px"></span>
      </button>
  </div>
   
  
</template>

<script>
export default {
    props: {
        breakTime: {
            type: Number
        }
    },
}
</script>
<style>
.break-setting{
  display: flex;
  flex-direction: row;
  justify-content: center;
  color: #fff
}
</style>