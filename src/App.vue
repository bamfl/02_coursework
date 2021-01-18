<template>
  <div class="container column">
    <form class="card card-w30" @submit.prevent="addInfo">
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
      <app-loader
        v-if="loadingCard"
        color="black"
      ></app-loader>

      <app-alert
        v-if="isAlert"
        :alert="alert"
        @delete-alert="alert = null"
      ></app-alert>

      <div v-if="!loadingCard">
        <component
          :is="`app-${item.selectType}`"
          v-for="item in card" :key="item.id"
          :id="item.id"
          :text="item.text"
          :type="item.selectType"
          @delete-item="deleteItem"
        >
        </component>
      </div>

      <app-empty v-if="isEmpty"></app-empty>
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

    <app-loader
      v-if="loadingComments"
      color="white"
    ></app-loader>
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
import AppAlert from '@/components/AppAlert'
import axios from 'axios'

export default {
  data () {
    return {
      text: '',
      selectType: 'title',
      card: [],
      id: '',
      comments: [],
      loadingCard: true,
      loadingComments: false,
      alert: null
    }
  },
  mounted () {
    this.loadInfo()
  },
  methods: {
    selectChange (type) {
      this.selectType = `${type}`
    },
    async loadInfo () {
      try {
        const { data } = await axios.get('https://vue-coursework-default-rtdb.firebaseio.com/info.json')

        const result = Object.keys(data).map(key => {
          return {
            id: key,
            ...data[key]
          }
        })

        this.card = result
        this.loadingCard = false
      } catch (error) {
        this.alert = {
          title: 'Ошибка!',
          type: 'danger',
          text: 'Список в базе данных пуст'
        }
        this.loadingCard = false
      }
    },
    async addInfo () {
      const response = await axios.post('https://vue-coursework-default-rtdb.firebaseio.com/info.json', {
        text: this.text,
        selectType: this.selectType
      })

      const firebaseDataId = await response.data.name

      this.card.push({
        id: firebaseDataId,
        text: this.text,
        selectType: this.selectType
      })
      this.text = ''
      this.selectType = 'title'
      this.alert = {
        title: 'Успешно!',
        type: 'primary',
        text: 'Данные добавлены'
      }
    },
    async loadComments () {
      this.loadingComments = true

      const { data } = await axios.get('https://jsonplaceholder.typicode.com/comments?_limit=42')

      const result = Object.keys(data).map(key => {
        return {
          id: key,
          ...data[key]
        }
      })

      this.comments = result

      this.loadingComments = false
    },
    async deleteItem (id) {
      await axios.delete(`https://vue-coursework-default-rtdb.firebaseio.com/info/${id}.json`)

      this.card = this.card.filter(item => item.id !== id)

      this.alert = {
        title: 'Успешно!',
        type: 'danger',
        text: 'Данные удалены'
      }
    }
  },
  computed: {
    isDisabled () {
      return !(this.text.length > 3)
    },
    isEmpty () {
      return !!((!this.loadingCard && this.card.length === 0))
    },
    isAlert () {
      return !!((this.alert !== null && !this.loadingCard))
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
    AppLoader,
    AppAlert
  }
}
</script>

<style lang="scss">
  .avatar {
    display: flex;
    justify-content: center;
  }

  .avatar img {
    width: 150px;
    height: auto;
    border-radius: 50%;
  }

  .wrapper {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  span {
    color: red;
    font-weight: 700;
    cursor: pointer;
    font-size: 20px;
  }
</style>
