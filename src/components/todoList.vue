<template>
    <table>
        <tr id="todoList">
        <td class="complete">완료</td>
        <td class="todo" colspan="2">ToDo List</td>
        </tr>
        <tr v-for="(item, idx) in data()" :key="idx">
            <td><input type="checkbox" @change="todoComplete"/></td>
            <td>{item.value}</td>
            <td><input type="button"></td>
        </tr>
    </table>
</template>

<script lang="ts">

import { Vue } from 'vue-class-component';    
   
    export default class Input extends Vue {
        data() {
            return {
                list : [
                    {
                        value : "1",
                        checked : "2",
                    },
                    {
                        value : "1",
                        checked : "2",
                    }
                ],
            }
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
                const item = localStorage.getItem(list[index]) as string;
                this.data().list.push({
                    value : item,
                    checked : item
                });
            }
        }
        todoComplete(e:Event) {
            const checkbox : HTMLInputElement = e.target as HTMLInputElement;
            if(checkbox.checked){
                checkbox.classList.add("checked");
            }else {
                checkbox.classList.remove("checked");
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
    }
</script>