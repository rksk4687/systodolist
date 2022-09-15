
import todoinput from './todoinput.vue';
<template>
    <div>
      <form class="register" @submit.prevent="handleTodoSubmit">
          <p>TodoList</p>
          <input type="text" id="register_text" v-model="text" placeholder="할일을 등록하세요." required>
          <button type="submit" id ="resiger_button">등록</button>
      </form>
      <form>
          <ul class="list" v-for="todo in todos" :key="todo.id">
            <li>
                <input type="checkbox" :id="todo.id.toString()" :checked="todo.check" @click="checkEnd"/>
                <span>{{todo.text}}</span>
            </li>
        </ul>
      </form>
    </div>
  </template>
  
  <script lang="ts">
  import { Options, Vue } from 'vue-class-component';
import todoinput from './todoinput.vue';
  
  
  interface Todo {
    text: string,
    id: number,
    check: boolean
   
  }
  @Options({
    props: {
      msg: String
    }
  })
  export default class HelloWorld extends Vue {
  
    TODOS_KEY = 'todos';
    todos:Array<Todo> = [];
    text: string = '';

    mounted(){
        let savedTodos = localStorage.getItem(this.TODOS_KEY);
        console.log(savedTodos)
        if (savedTodos) {
            const parsedTodos = JSON.parse(savedTodos);
            this.todos = parsedTodos;
        }
        
    }

  saveTodos():void {
    localStorage.setItem(this.TODOS_KEY, JSON.stringify(this.todos));
  }
  
   checkEnd(event: MouseEvent) {
    let myInput = event.target as HTMLInputElement;
  
    for (let i = 0; i < this.todos.length; i++) {
      if (this.todos[i].id.toString() == myInput.id) {
        console.log(myInput.checked);
        this.todos[i].check = myInput.checked;
        (myInput.parentNode as HTMLElement).classList.toggle('checked');
        break;
      }
    }
    this.saveTodos();
  }
  
  handleTodoSubmit() {
    const newTodoObj:Todo = {
      text:this.text,
      id:Date.now(),
      check:false
    };
    this.text = '';
    this.todos.unshift(newTodoObj);
    this.saveTodos();
  }
  
  
  
  }
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style>
    li {
      margin-bottom: 10px;
    }
    li.checked {
      font-weight: lighter;
      text-decoration: line-through;
      font-style: italic;
      color:gray
    }
  body {
      text-align: center;
    }
  p{
      font-size: 40px;
      font-weight: bold;
      margin-top: 0px;
      margin-bottom: 20px;
      padding-bottom: 10px;
      padding-top: 10px;
      background-color: deeppink;
      color: white;
      border-radius: 20px 20px 0px 0px / 20px 20px 0px 0px;
  }
   div{
      display: inline-block;
      border: 2px solid deeppink;
      border-radius: 20px 20px 20px 20px / 20px 20px 20px 20px;
      width: 550px;
      height: 700px;
      background-color: snow;
   }
    #register_text {
      width: 350px;
      height: 20px;
      border-color: deeppink;
    }
    button {
      width: 50px;
      height: 30px;
      border : none;
      background-color: deeppink;
      color: white;
      font-weight: bold;
      border-radius: 5px 5px 5px 5px / 5px 5px 5px 5px;
    }
    ul {
      list-style: none;
      text-align: left;
      margin-left: 10%;
      font-weight: bold;
      color:deeppink;
    }
  </style>
  