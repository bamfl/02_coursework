<template>
  <div class="alert" :class="alert.type" @click="$emit('delete-alert')">
    <div>
      <h3>{{ alert.title }}</h3>
      <p>{{ alert.text }}</p>
    </div>
    <button class="btn danger">Закрыть</button>
  </div>
</template>

<script>
export default {
  emits: ['delete-alert'],
  props: {
    alert: Object
  }
}
</script>

<style>

</style>
