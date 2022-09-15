<template>
    <!--list를 바인딩하여 목록 출력-->
    <table>
        <tr id="todoList">
        <td class="complete">완료</td>
        <td class="todo" colspan="2">ToDo List</td>
        </tr>
        <tr v-for="(item, idx) in list" :key="idx" class="todo">
            <td :id="item.value+'check'" class="tdCheck"><input :checked="item.checked" type="checkbox" @change="todoComplete"/></td>
            <td :id="'todo'+item.value" class="todoList" :class="item.checked.toString()">{{item.value}}</td>
            <td :id="item.value" class="tdDelete"><input class="delete button" @click="deleteTodo" value="[x]" type="button"></td>
        </tr>
    </table>
</template>

<script lang="ts">
    
    import { Vue, prop } from 'vue-class-component';    

    interface insert {
        value : string,
        checked : boolean
    }
    //부모 컴포넌트에 있는 list를 가져옴
    class Props {
        list = prop<Array<insert>>({required:true})
    }

    export default class Input extends Vue.with(Props) {
        //완료 여부를 list에 저장하여 새로고침할 때 해당 값으로 체크
        todoComplete(e:Event) {
            const checkbox : HTMLInputElement = e.target as HTMLInputElement;
            const id = (e.target as HTMLInputElement).parentElement!.id.slice(0,-5);
            if(checkbox.checked){
                this.list.forEach(obj=>{if(obj.value == id){obj.checked = true}});
                localStorage.setItem("todoList",JSON.stringify(this.list));
            }else {
                this.list.forEach(obj=>{if(obj.value == id){obj.checked = false}});
                localStorage.setItem("todoList",JSON.stringify(this.list));
            }
        }
        //list에서 todo 삭제
        deleteTodo(item : MouseEvent) : void{
            const del = (item.target as HTMLInputElement).parentElement!.id;
            this.list.splice(this.list.findIndex(obj => obj.value==del),1);
            localStorage.setItem("todoList",JSON.stringify(this.list));
        }
    }
</script>