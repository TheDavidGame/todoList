<template>
    <div v-if="isLoading">
        <div class = "borderElem" contenteditable="true">
            <h1>{{todoElem.header}}</h1>
            <h2>{{todoElem.text}}</h2>
        </div>
        <div>
            <button @click="saveTodo()" class="greenButtonElem" >Сохранить</button>
        </div>
    </div>
</template>

<script>
export default {
    validate ({ params }) {
        return /^\d+$/.test(params.id)
    },
    data () {
        return {
           isLoading: false,
           elem: [],
           todoElem: {}
        }
    },
    methods: {
        validLocalStorage () {
          if (localStorage.getItem('todoList')) {
            try {
                this.elem = JSON.parse(localStorage.getItem('todoList'));
            } catch(e) {
                localStorage.removeItem('todoList');
            }
            this.todoElem = this.elem[this.id]
          }
        },
        saveTodo(){
            const parsed = JSON.stringify(this.todoElem);
            localStorage.setItem('todoList', parsed);
        }
    },

    computed: {
        id () {
            return this.$route.params.id
        }
    },
    async mounted () {
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