<template>
  <div class="container column">
    <form class="card card-w30" @submit.prevent="addToCard">
      <app-select
        :selected="selectType"
        @select-change="selectChange"
      ></app-select>

      <app-textarea
        v-model="text"
      ></app-textarea>

      <app-button
        :disabled="isDisabled"
        color="primary"
        text="Добавить"
      ></app-button>
    </form>

    <div class="card card-w70">
      <component
        :is="`app-${item.selectType}`"
        v-for="item in card" :key="item.id"
        :id="item.id"
        :text="item.text"
        :type="item.selectType"
      >
      </component>

      <app-empty v-if="card.length === 0"></app-empty>
    </div>
  </div>

  <div class="container">
    <p>
      <app-button
        color="primary"
        text="Загрузить комментарии"
        @click="loadComments"
      ></app-button>
    </p>

    <div class="card">
      <app-subtitle text="Комментарии"></app-subtitle>

      <app-list :comments="comments"></app-list>
    </div>

    <app-loader v-if="comments.length === 0"></app-loader>
  </div>
</template>

<script>
import AppSelect from '@/components/AppSelect'
import AppTextarea from '@/components/AppTextarea'
import AppButton from '@/components/AppButton'
import AppTitle from '@/components/AppTitle'
import AppAvatar from '@/components/AppAvatar'
import AppSubtitle from '@/components/AppSubtitle'
import AppText from '@/components/AppText'
import AppEmpty from '@/components/AppEmpty'
import AppList from '@/components/AppList'
import AppLoader from '@/components/AppLoader'
import axios from 'axios'

export default {
  data () {
    return {
      text: '',
      selectType: 'title',
      card: [{ id: 1, text: 'Иван', selectType: 'title' }, { id: 2, text: 'Иванов', selectType: 'subtitle' }, { id: 3, text: 'https://pv51.ru/storage/users/default.png', selectType: 'avatar' }, { id: 4, text: 'Моряк', selectType: 'text' }],
      id: 1,
      comments: []
    }
  },
  methods: {
    selectChange (type) {
      this.selectType = `${type}`
    },
    addToCard () {
      this.card.push({
        id: this.id,
        text: this.text,
        selectType: this.selectType
      })
      this.id++
      this.text = ''
    },
    async loadComments () {
      const { data } = await axios.get('https://jsonplaceholder.typicode.com/comments?_limit=42')

      const result = Object.keys(data).map(key => {
        return {
          id: key,
          ...data[key]
        }
      })

      this.comments = result
      console.log(result)
    }
  },
  computed: {
    isDisabled () {
      return !(this.text.length > 3)
    }
  },
  components: {
    AppSelect,
    AppTextarea,
    AppButton,
    AppTitle,
    AppAvatar,
    AppSubtitle,
    AppText,
    AppEmpty,
    AppList,
    AppLoader
  }
}
</script>

<style>
  .avatar {
    display: flex;
    justify-content: center;
  }

  .avatar img {
    width: 150px;
    height: auto;
    border-radius: 50%;
  }
</style>
