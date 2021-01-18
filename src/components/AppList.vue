<template>
  <ul class="list" v-for="comment in comments" :key="comment.id">
    <li class="list-item">
      <div>
        <p><strong>{{comment.email}}</strong></p>
        <small>{{comment.body}}</small>
      </div>
    </li>
  </ul>
</template>

<script>
export default {
  props: {
    comments: Array
  }
}
</script>

<style>

</style>
