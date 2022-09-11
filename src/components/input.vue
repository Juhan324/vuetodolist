<template>
    <form id="insertForm" @submit.prevent="insertTodo">
          <h1>ToDo List</h1>
          <div id = "insertDiv">
              <p id = "insertTitle">일정 입력</p>
              <input type="text" id="insert" placeholder="일정을 입력해주세요.">
              <input type="submit" id="insertButton" class="button" value="등록">
          </div>
      </form>
</template>

<script lang="ts">
    import { Vue } from 'vue-class-component';    
   
    export default class Input extends Vue {
        
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
            (document.getElementById("insert") as HTMLInputElement).value="";
            return false;
        }
    }
    </script>