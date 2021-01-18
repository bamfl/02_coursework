<template>
  <div class="wrapper avatar">
    <img :src="text">
    <span @click="$emit('delete-item', id)">X</span>
  </div>
</template>

<script>
export default {
  props: {
    id: String,
    text: String,
    type: String
  }
}
</script>

<style>

</style>
