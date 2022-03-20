<template>

<div v-if="isLoading" class="modal-vue">
  <div v-if="todoList.length != 0 ">
    <div class="grid" >
      <!-- выводится список заметок -->
      <div  v-for="(todo, i) in todoList" :key="i" >
        <div @click="openTodo(i)"  class = "border" style="cursor: pointer;">
          <div class = "todoHeader" style="pointer-events: none;">
            {{todo.header}}
          </div>
          <ul id="list" class = "todoBottom" style="pointer-events: none;">
            <label v-for="(todoText, j) in todo.text.slice(0, 5)" :key="j" class="styleCheckbox">
              <input v-if="todoText.checkbox" type="checkbox" checked>
              <input v-else type="checkbox">
              <span>{{todoText.main}}</span>
            </label>
          </ul>
        </div>
        <div>
          <button @click="confirmNewTodo(i)" class="redButton" >X</button>
        </div>
      </div>
    </div>
    <!-- overlay -->
    <div class="overlay" v-if="showModal" @click="showModal = false"></div>

    <!-- modal -->
    <div class="modal" v-if="showModal" >
      <button class="close" @click="showModal = false">x</button>
      <h3>Вы точно хотите удалить ?</h3>
      <div style="display:inline-block;">
          <button @click="deleteNewTodo()" class="YesButton" >Да</button>
      </div>
      <div style="display:inline-block; float: right;">
          <button @click="showModal = false" class="NoButton" >Нет</button>
      </div>
    </div>

    <div>
      <button @click="addNewTodo()" class="greenButton" >Добавить</button>
    </div>
  </div>
  <div v-else>
    <h1>Список заметок пуст</h1>
    <div>
      <button @click="addNewTodo()" class="greenButton" >Добавить</button>
    </div>
  </div>
</div>
</template>

<script>
export default {
  data() {
    return {
      todoList: [],
      isLoading: false,
      showModal: false,
      mainIndex: 0,
    }
  },
  methods: {
    // добавление новой заметки
    addNewTodo() {
      if (!this.todoList) {
        return;
      }
      this.todoList.push({header: 'Заголовок', text: [{main: "текст", checkbox: false}]})
      this.saveTodoList()
    },
    // подтверждение добавления
    confirmNewTodo(index) {
      this.showModal = true
      this.mainIndex = index;
    },
    // удаление заметки
    deleteNewTodo() {
      if (!this.todoList) {
        return;
      }
      this.todoList.splice(this.mainIndex, 1)
      this.saveTodoList()
      this.showModal = false
    },
    //сохранение в local storage
    saveTodoList() {
      const parsed = JSON.stringify(this.todoList);
      localStorage.setItem('todoList', parsed);
    },
    // проверка на наличие данный в local storage
    validLocalStorage() {
      if (localStorage.getItem('todoList')) {
        try {
            this.todoList = JSON.parse(localStorage.getItem('todoList'));
        } catch(e) {
            localStorage.removeItem('todoList');
        }
      }
    },
    // переадресация на конкретную заметку
    openTodo(i) {
      this.$router.push({ path: `/${i}` })
    }
  },

  async mounted() {
    await this.validLocalStorage()
    this.isLoading = true
  },
}

</script>
<style scss>
.grid {
  gap: 1px;
  display: grid;
  text-align: center;
  grid-template-columns: 1fr 1fr 1fr;
   
}
.border {
  border-radius: 5%;
  max-width: 250px;
  min-height: 260px;
  word-wrap: break-word;
  border: 1px solid;
  padding: 40px;
  text-align: center;
  background-color: rgba(222, 231, 228, 0.836);
  margin: 10px 60px 10px 100px;
}
.borderComplete {
  border-radius: 5%;
  max-width: 250px;
  min-height: 200px;
  word-wrap: break-word;
  border: 1px solid;
  padding: 40px;
  text-align: center;
  background-color: green;
  margin: 10px 60px 10px 100px;
}
.todoHeader{
  text-decoration: underline;
  padding-bottom: 50px;
  font-size: 30px;
}
.todoBottom {
  font-size: 15px;
}
.greenButton {
  border-radius: 5%;
  float: right;
  background-color: #4CAF50; 
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  font-size: 16px;
  margin-right: 50px;
}
.redButton {
  position: relative;
  bottom: 340px;
  left: 150px;
  border-radius: 5%;
  background-color: red; 
  border: none;
  color: white;
  padding: 5px 10px;
  text-align: center;
  text-decoration: none;
  font-size: 16px;
  right:40px;
}
.NoButton {
  border-radius: 5%;
  background-color: #4CAF50; 
  border: none;
  color: white;
  padding: 5px 10px;
  text-align: center;
  text-decoration: none;
  font-size: 16px;
}
.YesButton {
  border-radius: 5%;
  background-color: red; 
  border: none;
  color: white;
  padding: 5px 10px;
  text-align: center;
  text-decoration: none;
  font-size: 16px;
}
.styleCheckbox {
    display: flex; 
    align-items: center;
}


/*modal window */

.modal-vue .overlay {
  position: fixed;
  z-index: 9998;
  top: 0;
  bottom: 50%;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, .5);
}

.modal-vue .modal {
  position: absolute;
  left: 600px;
  bottom: 250px;
  width: 300px;
  z-index: 9999;
  margin: 0 auto;
  padding: 20px 30px;
  background-color: #fff;
}

.modal-vue .close{
  position: absolute;
  top: 10px;
  right: 10px;
}


</style>
