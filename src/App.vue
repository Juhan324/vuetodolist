<template>
    <div id = "outer">
        <Input v-bind:list="list"/>
        <div id = "listForm">
            <TodoList v-bind:list="list"/>
        </div>
    </div>
</template>

<script lang="ts">
    import { Options, Vue } from 'vue-class-component';
    import Input from './components/input.vue';
    import TodoList from './components/todoList.vue';

    interface insert {
        value : string,
        checked : boolean
    }

    @Options({
        components: {
            Input,
            TodoList
        },
    })
    export default class App extends Vue {
        list : Array<insert> = [];

        created(){
            if(localStorage.getItem("todoList")!=null){
                this.list = JSON.parse(localStorage.getItem("todoList")!);
            }
        }
    }
</script>

<style scoped>
    @import '@/assets/style.css';
</style>
