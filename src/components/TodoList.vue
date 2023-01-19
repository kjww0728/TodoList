<template>
    <section>
        <ui class="todo-list" >
            <li class="todo-item" v-for="(todoItem,index) in filteredList" v-bind:key="todoItem.item" v-on:dbclick="removeTodo(todoItem, index)">
                <button class="checkbox" type="button" v-on:click="toggleTodo(todoItem, index)" > <span v-show="todoItem.completed">✔</span></button>
                <span class="todo" v-bind:class="{CheckBtnCompleted:todoItem.completed}">{{ todoItem.item }}</span>
                <button class="delBtn" type="button" v-on:click="removeTodo(todoItem, index)">X</button>
            </li>
        </ui>
    </section>
</template>

<script>
export default {
    props: ['propsdata','propsdata2'],
    computed:{
        filteredList(){
            switch(this.propsdata2){
                case 'all':{
                    return this.propsdata;
                }
                case 'active':{
                    return this.propsdata.filter((todoitem)=>!todoitem.completed);
                }
                case 'completed':{
                    return this.propsdata.filter((todoitem)=>todoitem.completed);
                }
                default:{
                    return this.propsdata;
                }
            }
        }
    },
    methods:{
        removeTodo(todoItem, index){
            this.$emit('removeTodo',todoItem, index);
        },
        toggleTodo(todoItem, index){
            this.$emit('toggleTodo',todoItem, index);
        },
    }
}
</script>

<style>
.todo-item {
    position: relative;
    display: flex;
    flex-wrap: nowrap;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    height: 3rem;
    border-bottom: 1px solid #ddd;
    list-style-type: none;
}

.todo-item:hover .delBtn {
    opacity: 1;
}

.checkbox {
    min-width: none;
    min-height: none;
    width: 1.5rem;
    height: 1.5rem;
    margin: 0.5rem 0.5rem;
    border-radius: 50px;
    border: 1px solid lightgray;
    cursor: pointer;
    text-align: center;
    opacity:1;
    color: green;
}

.todo-item.checked .checkbox{
    border: 2px solid darkgray;
    color: green;
}

.todo {
    font-size: 1.3rem;
    padding: 0 1rem;
    width: 80%;
}

.todo-item.checked .todo{
 font-style: italic;
 text-decoration: line-through;
 color: lightgray;
}

.delBtn {
    opacity: 20%;
    width: 3rem;
    height: 3rem;
    font-size: 1rem;
    font-weight: lighter;
    cursor: pointer;
}

.edit-input {
    position: absolute;
    left: 0;
    top: 0;
    width: 590px;
    height: 2.8rem;
    margin: 0;
}

.CheckBtnCompleted{
    text-decoration:line-through;
    color:darkgrey;
}


</style>