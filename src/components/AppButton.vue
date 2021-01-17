<template>
  <button
    class="btn" :class="color"
    type="submit"
    :disabled="disabled"
  >Добавить</button>
</template>

<script>
export default {
  props: {
    disabled: Boolean,
    color: String
  }
}
</script>

<style>

</style>
