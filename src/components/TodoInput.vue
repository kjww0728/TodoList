<template>
    <div class="todo-input-box">
        <button class="complete-all-btn" v-on:click="CheckAllTodo()">✔</button>
        <input type="text" v-model="newTodoItem" v-on:keyup.enter="addTodo" placeholder="해야 할 일을 입력해주세요" class="todo-input">
    </div>
</template>

<script>
export default {
    props: ['propsdata'],
    data(){
        return{
            newTodoItem:'',
        }
    },
    computed:{

    },
    methods: {
        addTodo(){
            if(this.newTodoItem !== ''){
                //inputbox에 입력된 텍스트의 앞, 뒤 공백문자열 제거
                var value = this.newTodoItem && this.newTodoItem.trim();
                //완료 여부를 포함한 객체 내보내기
                var obj = {completed: false, item: value};
                //App컴포넌트로 이벤트 전달
                this.$emit('addTodo', obj.item);//****/
                //this.$emit('addTodo', value);
                this.clearInputbox();
            }
        },
        clearInputbox(){
            this.newTodoItem = ''; 
        },
        CheckAllTodo(){
            this.$emit('CheckAllTodo');
        }
    }
}
</script>

<style>

.todo-input-box {
    display: flex;
    flex-wrap: nowrap;
    flex-direction: row;
    height: 3rem;
    border-bottom: 1px solid #ddd;
    justify-content: flex-start;
    align-items: center;
}

.todo-input {
    width: 80%;
    text-align: center;
    border: 0;
    outline: none;
    font-size: 1.3rem;
}

.complete-all-btn {
    color: gray;
    min-width: none;
    min-height: none;
    width: 1.5rem;
    height: 1.5rem;
    margin: 0.5rem 0.5rem;
    border-radius: 50px;
    cursor: pointer;
    font-size: 1.2rem;
    
}

.complete-all-btn.checked {
    color: green
}
</style>