<template>
  <div class="wrapper">
    <p>
      {{text}}
    </p>
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
