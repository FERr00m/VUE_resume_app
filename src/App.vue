<template>
  <div class="container column">
    <form class="card card-w30" @submit.prevent="addBlock">

      <AppSelect
        label="Тип блока"
        :options="[
          {
            value: 'title',
            name: 'Заголовок'
          },
          {
            value: 'subtitle',
            name: 'Подзаголовок'
          },
          {
            value: 'avatar',
            name: 'Аватар'
          },
          {
            value: 'text',
            name: 'Текст'
          }
        ]"
        v-model="typeBlock"
      ></AppSelect>

      <div class="form-control">
        <label for="value">Значение</label>
        <textarea
          id="value" rows="3"
          v-model.trim="propertyBlock"
        ></textarea>
      </div>

      <AppButton
        type="primary"
        :disabled="isValid"
      >
        Добавить
      </AppButton>

    </form>

    <div class="card card-w70">
      <h3 v-if="!mainContent.length">Добавьте первый блок, чтобы увидеть результат</h3>
      <div v-else v-html="mainContent"></div>
    </div>
  </div>
  <div class="container">
    <p>
      <AppButton
        type="primary"
        @action="loadComments"
      >
        Загрузить комментарии
      </AppButton>
    </p>
    <div v-if="comments.length && !loading" class="card">
      <h2>Комментарии</h2>
      <ul class="list">
        <li
          class="list-item"
          v-for="comment in comments"
          :key="comment.id"
        >
          <div>
            <p><strong>{{ comment.email }}</strong></p>
            <small>{{ comment.body }}</small>
          </div>
        </li>
      </ul>
    </div>
    <div v-if="loading" class="loader"></div>
  </div>
</template>

<script>
import AppSelect from '@/components/AppSelect'
import AppButton from '@/components/AppButton'

export default {
  components: {
    AppSelect,
    AppButton
  },
  data () {
    return {
      typeBlock: 'title',
      propertyBlock: '',
      templates: {
        title: {
          content: '<h1>@</h1>'
        },
        subtitle: {
          content: '<h2>@</h2>'
        },
        avatar: {
          content: `
            <div class="avatar">
                <img src="@">
            </div>
          `
        },
        text: {
          content: '<p>@</p>'
        }
      },
      mainContent: JSON.parse(localStorage.getItem('resume')) || '',
      comments: [],
      loading: false
    }
  },
  methods: {
    addBlock () {
      const res = this.templates[this.typeBlock].content.replace('@', this.htmlEncode(this.propertyBlock))
      this.mainContent += res
      localStorage.setItem('resume', JSON.stringify(this.mainContent))
      this.typeBlock = 'title'
      this.propertyBlock = ''
    },
    htmlEncode (str) {
      return String(str).replace(/[^\w. ]/gi, function (c) {
        console.log(c.charCodeAt(0))
        return '&#' + c.charCodeAt(0) + ';'
      })
    },
    loadComments () {
      this.loading = true
      try {
        setTimeout(async () => {
          const res = await fetch('https://jsonplaceholder.typicode.com/comments?_limit=42')
          this.comments = await res.json()
          this.loading = false
        }, 1500)
      } catch (e) {
        this.loading = false
        console.warn(e.message)
      }
    }
  },
  computed: {
    isValid () {
      return this.propertyBlock.length < 4
    }
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
