<template>
  <div class="avatar">
    <img :src="text">
  </div>
</template>

<script>
export default {
  props: {
    id: Number,
    text: String,
    type: String
  }
}
</script>

<style>

</style>
