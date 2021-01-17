<template>
  <div class="form-control">
    <label for="value">Значение</label>
    <textarea id="value" rows="3" :value="modelValue" @input="textChange"></textarea>
  </div>
</template>

<script>
export default {
  emits: ['update:modelValue'],
  props: {
    modelValue: String
  },
  methods: {
    textChange (value) {
      this.$emit('update:modelValue', event.target.value)
    }
  }
}
</script>

<style>

</style>
