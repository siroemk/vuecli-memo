<template>
  <PageTitle msg="Memo App"/>
  <div id="app">
    <ul>
      <li v-for="memo in memos" :key="memo.id" @click="displayEditForm(memo)">{{ memo.text }}</li>
      <button type="button" @click="addMemo">+</button>
    </ul>
  </div>
  <div v-if="edit">
    <input class="form" v-model="text">
    <button type="button" @click="editMemo">編集</button>
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
      edit: false,
      targetMemo: ''
    }
  },
  methods: {
    addMemo: function() {
      this.memos.push({
        id: new Date().getTime().toString(),
        text: '新規メモ',
      })
      localStorage.setItem('memos', JSON.stringify(this.memos))
      const memo = this.memos.slice(-1)[0]
      this.displayEditForm(memo)
    },
    editMemo: function() {
      const index = this.memos.indexOf(this.targetMemo)
      const memoToEdit = this.memos[index]
      memoToEdit['text'] = this.text
      localStorage.setItem('memos', JSON.stringify(this.memos))
    },
    displayEditForm: function(memo) {
      this.edit = true
      this.text = memo.text
      this.targetMemo = memo
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
