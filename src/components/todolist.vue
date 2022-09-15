<template>
    <ul class="list">
        
    </ul>
</template>
<script lang="ts">
    import { Vue, prop } from "vue-class-component";

    interface Todo {
        text: string,
        id: number,
        check: boolean
    }
    class Props {
        todos = prop<Array<Todo>>({required : true});
    }
    export default class todolist extends Vue.with(Props) {
        TODOS_KEY = 'todos';
    }
</script>