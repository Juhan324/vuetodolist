<template>
    <!--입력 폼-->
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
    import { Vue, prop } from 'vue-class-component';
    
    interface insert {
        value : string,
        checked : boolean
    }

    //부모 컴포넌트에 list를 받아옴
    class Props {
        list = prop<Array<insert>>({required:true})
    }

    export default class Input extends Vue.with(Props) {

        //Todo 입력 함수
        //공백을 입력하거나 이미 입력된 값을 입력하면 alert창의 띄우면 false return
        insertTodo() : boolean{
            const insertValue = (document.getElementById("insert") as HTMLInputElement).value;

            const insertInput : insert = {
                value : insertValue,
                checked : false
            }

            if(insertInput.value.trim() == ""){
                alert("일정을 입력해주세요.");
                return false;
            }
            for(let i = 0; i < this.list.length; i++){
                if(insertInput.value == this.list[i].value){
                    alert("이미 입력된 일정입니다.");
                    return false;
                }
            }
            this.list.unshift(insertInput);
            localStorage.setItem("todoList", JSON.stringify(this.list));
            (document.getElementById("insert") as HTMLInputElement).value="";
            return false;
        }
    }
    </script>