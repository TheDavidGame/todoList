<template>
    <div v-if="isLoading">
        <div class = "borderElem"  id="demo">
            <h1 contenteditable="true">{{todoElem.header}}</h1>
                <ul id="list" contenteditable="true">
                    <label v-for="(todo, i) in todoElem.text" :key="i" class = "styleCheckboxTodo">
                        <input v-if="todo.checkbox" type="checkbox" checked>
                        <input v-else type="checkbox">
                        <span>{{todo.main}}</span>
                    </label>
                </ul> 
        </div>
        <div style="margin-top:20px">
            <div>
                <button @click="saveTodo()" class="greenButtonElem" >Сохранить</button>
            </div>
            <div>
                <button @click="addCheckbox" id="ButtonAdd" class="greenButtonElem" >Добавить пункт</button>
            </div>
            <div>
                <input type="text" name="" id="inputText" placeholder="Содержание пункта" style="float: right;margin-right:30px">
            </div>
            <div>
                <button @click="deleteCheckbox" id="ButtonRemove" class="greenButtonElem" >Удалить пункт</button>
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
        addCheckbox() {
            var inputText = document.getElementById("inputText")
            var list = document.getElementById('list')
            var check = document.createElement("input")
            check.type = "checkbox"
            check.addEventListener("click", function(e) {
              console.log(text.textContent)
            });
            var text = document.createElement("span")
            text.textContent = inputText.value
            var label1 = document.createElement('label')
            label1.classList.add("styleCheckboxTodo")
            label1.appendChild(check)
            label1.appendChild(text)
            list.appendChild(label1)
        },
        deleteCheckbox() {
            var checks = document.querySelectorAll("#list input[type=checkbox]:checked")
            checks.forEach(item => item.parentNode.parentNode.removeChild(item.parentNode))
        },
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
            let numChild = document.getElementById('demo').querySelector("#list").childNodes.length
            let text = []
            
            for(let i = 0; i < numChild; i++) {
                try {
                text.push({
                        main: document.getElementById('demo').querySelector("#list").children[i].children[1].innerHTML,
                        checkbox: document.getElementById('demo').querySelector("#list").children[i].children[0].checked,
                    })
                
                } catch(e) {
                    console.log("Пустой пункт")
                }
            }

            let elemTodo = {header, text}
            this.arrayTodo.splice(this.id, 1, elemTodo)
            const parsed = JSON.stringify(this.arrayTodo);
            localStorage.setItem('todoList', parsed);
        },
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
.styleCheckboxTodo {
    display: flex; 
    align-items: center;
}
</style>