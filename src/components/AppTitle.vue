<template>
  <div class="wrapper">
    <h1>
      {{text}}
    </h1>
    <span @click="$emit('delete-item', id)">X</span>
  </div>
</template>

<script>
export default {
  emit: ['delete-item'],
  props: {
    id: String,
    text: String,
    type: String
  }
}
</script>

<style>

</style>
