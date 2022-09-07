<template>
  <div id = "outer">
    <form id="insertForm" @submit.prevent="insertTodo">
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
  
  import { Vue } from 'vue-class-component';

  export default class index extends Vue {
      autoincrement() : string{
          if(localStorage.getItem("number") == null){
              localStorage.setItem("number", "0");
          }
          localStorage.setItem("number",(Number(localStorage.getItem("number"))+1).toString());
          return localStorage.getItem("number") as string;
      }


      insertTodo() : boolean{
          const insertValue = (document.getElementById("insert") as HTMLInputElement).value;

          interface insert {
              value : string,
              checked : boolean
          }

          const insertInput : insert = {
              value : insertValue,
              checked : false
          }

          if(insertInput.value.trim() == ""){
              alert("일정을 입력해주세요.");
              return false;
          }
          for(let i = 0; i < localStorage.length; i++){
              if(insertInput.value == JSON.parse(localStorage.getItem(localStorage.key(i) as string) as string).value){
                  alert("이미 입력된 일정입니다.");
                  return false;
              }
          }
          const key : string = this.autoincrement();
          localStorage.setItem(key, JSON.stringify(insertInput));
          this.initialize(key, JSON.stringify(insertInput));
          (document.getElementById("insert") as HTMLInputElement).value="";
          return false;
      }



      readTodo() : void {
          const list : Array<string> = [];
          for (let i = 0; i < localStorage.length; i++) {
              const key : string = localStorage.key(i) as string;
              if(key!="number"){
                  list.push(key);
              }
          }
          list.sort((a, b) => {
              return Number(a)-Number(b);
          });

          for (let index = 0; index < list.length; index++) {
              const item = localStorage.getItem(list[index]);
              if(item != null && item != "true" && item != "false" && list[index] != "number"){
                  this.initialize(list[index], item);
              }
          }
      }

      deleteTodo(item : MouseEvent) : void{
          const del = (item.target as HTMLTableRowElement).id.slice(0,-6);
          for(let i=0; i<localStorage.length; i++){
              if(JSON.parse(localStorage.getItem(localStorage.key(i) as string) as string).value == del){
                  localStorage.removeItem(localStorage.key(i) as string);
                  (document.getElementById(del+"list") as HTMLTableRowElement).remove();
              }
          }
      }

      initialize(key : string, item : string) : void{
          const item2 = JSON.parse(item);
          const td = document.createElement("td");
          td.id = item2.value;
          td.className = "todoList";
          td.appendChild(document.createTextNode(item2.value));

          const checkbox = document.createElement("input");
          checkbox.type = "checkbox";
          checkbox.className = "checkbox";
          checkbox.ariaLabel = item2.value;

          

          checkbox.onchange = () : void => {
              const checkboxTd : HTMLTableCellElement = document.getElementById(item2.value) as HTMLTableCellElement;

              if(checkbox.checked){
                  item2.checked = true;
                  checkboxTd.classList.add("checked");
                  localStorage.setItem(key, JSON.stringify(item2));
              }else {
                  item2.checked = false;
                  checkboxTd.classList.remove("checked");
                  localStorage.setItem(key, JSON.stringify(item2));
              }
          };
          
          const deleteButton = document.createElement("input");
          deleteButton.type = "button";
          deleteButton.className = "delete button";
          deleteButton.value = "[x]";
          deleteButton.id = item2.value+"delete";

          const tdCheck = document.createElement("td");
          tdCheck.appendChild(checkbox);
          tdCheck.className = "tdCheck";

          const tdDelete = document.createElement("td");
          tdDelete.appendChild(deleteButton);
          tdDelete.className = "tdDelete";

          const tr = document.createElement("tr");
          tr.appendChild(tdCheck);
          tr.appendChild(td);
          tr.appendChild(tdDelete);
          tr.id = item2.value+"list";
          (document.getElementById("todoList") as HTMLTableRowElement).after(tr);

          deleteButton.addEventListener("click", this.deleteTodo);

          if(item2.checked == true) {
              checkbox.checked = true;
              (document.getElementById(item2.value) as HTMLInputElement).classList.add("checked");
          }
      }

      test(number:Number) {
          const startTime : number = new Date().getTime();
          for(let i=0; i<number; i++){
              (document.getElementById("insert") as HTMLInputElement).value = i.toString();
              this.insertTodo();
          }
          const endTime : number = new Date().getTime();
          console.log(endTime-startTime);
      }
      created() {
        window.addEventListener("DOMContentLoaded", this.readTodo);
      }
}
  
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  @import '@/assets/style.css';
</style>
