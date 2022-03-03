<template>
  <PageTitle msg="Memo App"/>
  <div id="app">
    <ul>
      <li v-for="memo in memos" :key="memo.id">{{ memo.text }}</li>
      <button type="button" @click="displayForm">+</button>
    </ul>
  </div>
  <div v-if="edit">
    <input class="form" v-model="text">
    <button type="button" @click="addMemo">編集</button>
    <button type="button">削除</button>
  </div>
</template>

<script>
import PageTitle from './components/PageTitle.vue'

export default {
  name: 'App',
  components: {
    PageTitle
  },
  data() {
    return{
      memos: JSON.parse(localStorage.getItem('memos')),
      text: '',
      edit: false
    }
  },
  methods: {
    addMemo: function() {
      this.memos.push({
        id: new Date().getTime().toString(),
        text: this.text,
      })
      localStorage.setItem('memos', JSON.stringify(this.memos))
      this.edit = false
    },
    displayForm: function() {
      this.edit = true
      this.text = '新規メモ'
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
