<template>
  <PageTitle msg="Memo"/>
  <div class="memo-container">
    <div id="memo-app">
      <ul>
        <li v-for="memo in memos" :key="memo.id" @click="displayEditForm(memo)" class="memo-list">
            {{ memoTitle(memo.text) }}
        </li>
        <button type="button" class="add-btn" @click="addMemo">+</button>
      </ul>
    </div>
    <div v-if="edit">
      <textarea class="form" v-model="text" cols="30" rows="6"></textarea>
      <div class="btn-box">
        <button type="button" class="edit-btn" @click="editMemo">編集</button>
        <button type="button" class="delete-btn" @click="deleteMemo">削除</button>
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
  data () {
    return {
      memos: JSON.parse(localStorage.getItem('memos')) || [],
      text: '',
      edit: false,
      editingMemo: ''
    }
  },
  methods: {
    addMemo () {
      this.memos.push({
        id: new Date().getTime().toString(),
        text: '新規メモ'
      })
      this.save(this.memos)
      const memo = this.memos.slice(-1)[0]
      this.displayEditForm(memo)
    },
    editMemo () {
      const memoToEdit = this.memos.find((memo) => memo.id === this.editingMemo.id)  
      memoToEdit.text = this.text 
      this.save(this.memos)
      this.edit = false
    },
    deleteMemo () {
      const index = this.memos.findIndex((memo) => memo.id === this.editingMemo.id)
      this.memos.splice(index, 1)
      this.save(this.memos)
      this.edit = false
    },
    displayEditForm (memo) {
      this.edit = true
      this.text = memo.text
      this.editingMemo = memo
    },
    cancelEditing () {
      this.edit = false
    },
    save (memos) {
      localStorage.setItem('memos', JSON.stringify(memos))
    }
  },
  computed: {
    memoTitle: () => (text) => {
      return text.split('\n')[0]
    }
  }
}
</script>

<style>
body {
  background: #B0BEC5;
}
ul {
  margin:0;
  padding:0;
}
li {
  list-style: none;
}
#memo-app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2b2b2b;
  width: 50%;
}
.memo-container {
  display: flex;
  background: #fff;
  border-radius: 10px;
  margin: 20px;
  padding: 40px;
}
.memo-list {
  padding-bottom: 16px;
}
.memo-list:hover {
  opacity: 0.7;
}
.add-btn {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background-color: #1976D2;
  border: none;
  font-size: 30px;
  font-weight: bold;
  color: #fff;
}
.form {
  border: 3px #dcdcdc solid;
  padding: 10px;
  border-radius: 6px;
  color: #2b2b2b;
  font-size: 16px;
}
.btn-box {
  display: flex;
  margin-top: 10px;
}
.edit-btn {
  margin-right: 20px;
  background: #26A69A;
  width: 145px;
  border-radius: 4px;
  border: none;
  color: #ffffff;
}
.delete-btn {
  margin-right: 20px;
  background: #E57373;
  width: 145px;
  border-radius: 4px;
  border: none;
  color: #ffffff;
  line-height: 30px;
}
button:hover {
  opacity: 0.8;
}
</style>
