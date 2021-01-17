<template>
  <div class="form-control">
    <label for="type">Тип блока</label>
    <select id="type" v-model="selectedLocal" @change="selectChange">
      <option value="title">Заголовок</option>
      <option value="subtitle">Подзаголовок</option>
      <option value="avatar">Аватар</option>
      <option value="text">Текст</option>
    </select>
  </div>
</template>

<script>
export default {
  emits: ['select-change'],
  props: {
    selected: String
  },
  data () {
    return {
      selectedLocal: this.selected
    }
  },
  methods: {
    selectChange (event) {
      this.$emit('select-change', event.target.value)
    }
  }
}
</script>

<style>

</style>
