<template>
    <div id = "outer">
        <Input/>
        <div id = "listForm">
            <TodoList/>
        </div>
    </div>
</template>

<script lang="ts">
    import { Options, Vue } from 'vue-class-component';
    import Input from './input.vue';
    import TodoList from './todoList.vue';

    @Options({
        components: {
            Input,
            TodoList
        },
    })
  export default class index extends Vue {

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  @import '@/assets/style.css';
</style>
