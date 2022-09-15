<template>
    <form class="register" @submit.prevent="handleTodoSubmit">
        <p>TodoList</p>
        <input type="text" id="register_text" placeholder="할일을 등록하세요." required>
        <button type="submit" id ="resiger_button">등록</button>
    </form>
</template>

<script lang="ts">
    import { prop, Vue } from 'vue-class-component';
    
    
    interface Todo {
      text: string,
      id: number,
      check: boolean
    }
    class Props {
        todos = prop<Array<Todo>>({required : true});
    }
    export default class todoinput extends Vue.with(Props) {
        TODOS_KEY = 'todos';
        

        handleTodoSubmit() {
            const register_text:HTMLInputElement = document.querySelector('#register_text') as HTMLInputElement; 
            console.log(register_text);
            const newTodo: string = register_text?.value as string;
            const newTodoObj:Todo = {
                text:newTodo,
                id:Date.now(),
                check:false
            };
            (register_text as HTMLInputElement).value = '';
            this.todos.push(newTodoObj);
            this.insertTodo(newTodoObj);
            this.saveTodos();
        }

        insertTodo(newTodo: Todo) {
            const list:any = document.querySelector('.list');
            console.log(this.todos);
            const li = document.createElement('li');
            const input = document.createElement('input');
            const span = document.createElement('span');
            input.id = newTodo.id as unknown as string;
            input.addEventListener('click', this.checkEnd);

            if (newTodo.check) {
                input.checked = true;
                li.classList.toggle('checked');
            }

            span.innerText = newTodo.text;
            input.type = 'checkbox';
            li.appendChild(input);
            li.appendChild(span);
            list.prepend(li);
        }

        saveTodos():void {
            localStorage.setItem(this.TODOS_KEY, JSON.stringify(this.todos));
        }

        checkEnd(event: MouseEvent) {
            let myInput = event.target as HTMLInputElement;

            for (let i = 0; i < this.todos.length; i++) {
                if (this.todos[i].id.toString() == myInput.id) {
                    this.todos[i].check = myInput.checked;
                    (myInput.parentNode as HTMLElement).classList.toggle('checked');
                    break;
                }
            }
            this.saveTodos();
        }
    }
</script>