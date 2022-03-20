<template>
    <div v-if="isLoading">
        <div class = "borderElem" contenteditable="true" id="demo">
            <h1>{{todoElem.header}}</h1>
            <h2>{{todoElem.text}}</h2>
        </div>
        <div style="margin-top:20px">
            <div>
                <button @click="saveTodo()" class="greenButtonElem" >Сохранить</button>
            </div>
            <div>
                <button @click="completed()" class="greenButtonElem" >Отметить как выполненный</button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    // проверка на валидность id
    validate({ params }) {
        return /^\d+$/.test(params.id)
    },
    data() {
        return {
           isLoading: false,
           arrayTodo: [],
           todoElem: {}
        }
    },
    methods: {
        // проверка на наличие данный в local storage
        validLocalStorage() {
          if (localStorage.getItem('todoList')) {
            try {
                this.arrayTodo = JSON.parse(localStorage.getItem('todoList'));
            } catch(e) {
                localStorage.removeItem('todoList');
            }
            this.todoElem = this.arrayTodo[this.id]
          }
        },
        //сохранение в local storage
        saveTodo() {
            let header = document.getElementById('demo').children[0].innerHTML.substring(0, 10)
            let text = document.getElementById('demo').children[1].innerHTML
            let elemTodo = {header, text}
            this.arrayTodo.splice(this.id, 1, elemTodo)
            const parsed = JSON.stringify(this.arrayTodo);
            localStorage.setItem('todoList', parsed);
        },
        completed() {
            
        }
    },

    computed: {
        // id заметки, которую мы редактируем
        id() {
            return this.$route.params.id
        }
    },
    // тут происходит ожидание завершения всех методов, только после этого появится страница 
    async mounted() {        
        await this.validLocalStorage()
        this.isLoading = true
    },
}
</script>
<style >
.borderElem {
    margin-left: 500px;       
    border-radius: 5%;
    max-width: 500px;
    min-height: 500px;
    word-wrap: break-word;
    border: 1px solid;
    padding: 40px;
    text-align: center;
    background-color: rgba(222, 231, 228, 0.836);
}
.borderElemComplete {
    margin-left: 500px;       
    border-radius: 5%;
    max-width: 500px;
    min-height: 500px;
    word-wrap: break-word;
    border: 1px solid;
    padding: 40px;
    text-align: center;
    background-color: green;
} 
.greenButtonElem {
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
</style>