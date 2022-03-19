<template>

<div v-if="isLoading" class="modal-vue">
  <div class="grid" >
    <div  v-for="(todo, i) in todoList" :key="i" class = "border" >
      <div @click="openTodo(i)" style="cursor: pointer;">
            <div class = "todoHeader">
              {{todo.header}}
            </div>
            <div class = "todoBottom">
              {{todo.text}}
            </div>
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

</template>

<script>

export default {
  data() {
    return {
      todoList: [{header: '', text: ''}],
      isLoading: false,
      showModal: false,
      mainIndex: 0,
    }
  },
  methods: {
    addNewTodo(){
      if (!this.todoList) {
        return;
      }
      this.todoList.push({header: '346', text: 'text'})
      this.saveTodoList()
    },
    confirmNewTodo(index){
      this.showModal = true
      this.mainIndex = index;
    },
    deleteNewTodo(){
      if (!this.todoList) {
        return;
      }
      this.todoList.splice(this.mainIndex, 1)
      this.saveTodoList()
      this.showModal = false
    },
    
    saveTodoList(){
      const parsed = JSON.stringify(this.todoList);
      localStorage.setItem('todoList', parsed);
    },
    validLocalStorage(){
      if (localStorage.getItem('todoList')) {
        try {
            this.todoList = JSON.parse(localStorage.getItem('todoList'));
        } catch(e) {
            localStorage.removeItem('todoList');
        }
      }
    },
    openTodo(i){
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
  min-height: 200px;
  word-wrap: break-word;
  border: 1px solid;
  padding: 40px;
  text-align: center;
  background-color: rgba(222, 231, 228, 0.836);
  margin: 10px 60px 10px 100px;
}
.todoHeader{
  text-decoration: underline;
  padding-bottom: 100px;
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
    bottom:180px;
    left: 180px;
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
