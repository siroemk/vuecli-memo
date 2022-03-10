<template>
  <PageTitle msg="Memo"/>
  <div class="memo-container">
    <div id="memo-app">
      <ul>
        <li v-for="memo in memos" :key="memo.id" @click="displayEditForm(memo)" class="memo–text">
            {{ memo.text.split('\n')[0] }}
        </li>
        <button type="button" @click="addMemo">+</button>
      </ul>
    </div>
    <div class="form-box" v-if="edit">
      <textarea class="form" v-model="text" cols="30" rows="6"></textarea>
      <div class="button-box">
        <button type="button" class="edit-button" @click="editMemo">編集</button>
        <button type="button" class="delete-button" @click="deleteMemo">削除</button>
      </div>
    </div>
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
    addMemo () {
      this.memos.push({
        id: new Date().getTime().toString(),
        text: '新規メモ',
      })
      localStorage.setItem('memos', JSON.stringify(this.memos))
      const memo = this.memos.slice(-1)[0]
      this.displayEditForm(memo)
    },
    editMemo () {
      const memoToEdit = this.memos.find((memo) => memo.id === this.targetMemo.id)
      memoToEdit.text = this.text
      localStorage.setItem('memos', JSON.stringify(this.memos))
      this.edit = false
    },
    deleteMemo () {
      const index = this.memos.findIndex((memo) => memo.id === this.targetMemo.id)
      this.memos.splice(index, 1)
      localStorage.setItem('memos', JSON.stringify(this.memos))
      this.edit = false
    },
    displayEditForm (memo) {
      this.edit = true
      this.text = memo.text
      this.targetMemo = memo
    }
  }
}
</script>

<style>
body {
  background: #B0BEC5;
}
li {
  list-style: none;
}
#memo-app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2b2b2b;
  margin-top: 20px;
  width: 50%;
}
.memo-container {
  display: flex;
  background: #fff;
  border-radius: 10px;
  margin: 0 20px;
}
.memo–text {
  padding-bottom: 16px;
}
.memo–text:hover {
  opacity: 0.7;
}
.form-box {
  margin-top: 40px;
}
.form {
  border: 3px #dcdcdc solid;
  padding: 10px;
  border-radius: 6px;
  color: #2b2b2b;
  font-size: 16px;
}
.button-box {
  display: flex;
  margin-top: 10px;
}
.edit-button {
  margin-right: 20px;
  background: #26A69A;
  width: 140px;
  border-radius: 4px;
  border: none;
  color: #ffffff;
}
.delete-button {
  margin-right: 20px;
  background: #E57373;
  width: 140px;
  border-radius: 4px;
  border: none;
  color: #ffffff;
  line-height: 30px;
}
button:hover {
  opacity: 0.8;
}
</style>
