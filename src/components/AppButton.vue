<template>
  <button
    class="btn" :class="color"
    type="submit"
    :disabled="disabled"
  >{{ text }}</button>
</template>

<script>
export default {
  props: {
    disabled: Boolean,
    color: String,
    text: String
  }
}
</script>

<style>

</style>
