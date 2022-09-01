<template>
  <div id = "outer">
    <form id="insertForm">
        <h1>ToDo List</h1>
        <div id = "insertDiv">
            <p id = "insertTitle">일정 입력</p>
            <input type="text" id="insert" placeholder="일정을 입력해주세요.">
            <input type="submit" id="insertButton" class="button" value="등록">
        </div>
    </form>
    <div id = "listForm">
        <table>
            <tr id="todoList">
                <td class="complete">완료</td>
                <td class="todo" colspan="2">ToDo List</td>
            </tr>
        </table>
    </div>
  </div>
</template>

<script lang="ts">
  import '@/assets/script.ts';  
  import { Vue } from 'vue-class-component';

  export default class index extends Vue {
  }
  
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  @import '@/assets/style.css';
</style>
