<template>
  <div class="wrapper">
    <h2>
      {{text}}
    </h2>
    <span v-if="text !== 'Комментарии'" @click="$emit('delete-item', id)">X</span>
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
