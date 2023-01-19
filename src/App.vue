<template>
  <div id="app" class="todo-wrapper">
    <TodoHeader></TodoHeader>
    <div class="todo-box">
      <TodoInput v-bind:propsdata="todoItems" v-on:addTodo="addTodo" v-on:checkAllTodo="checkAllTodo"></TodoInput>
      <TodoList v-bind:propsdata="todoItems" v-bind:propsdata2="filterType" v-on:removeTodo="removeTodo" v-on:toggleTodo="toggleTodo" ></TodoList>
      <TodoFooter v-bind:propsdata="todoItems" v-on:removeAll="clearAll" v-on:clearCompleted="clearCompleted" v-on:changeFilter="changeFilter"></TodoFooter>
    </div>
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader';
import TodoInput from './components/TodoInput';
import TodoList from './components/TodoList';
import TodoFooter from './components/TodoFooter';

export default{
  data(){
    return {
      todoItems: [],
      filterType: ''
    }
  },
  created(){
    if(localStorage.length>0){
      for(var i = 0; i<localStorage.length; i++){
        if(localStorage.key(i) !== 'loglevel:webpack-dev-server'){
          //this.todoItems.push(localStorage.key(i));
          this.todoItems.push(JSON.parse(localStorage.getItem(localStorage.key(i)))); ///순서보장
        }
      }
    }
  },
  
  methods:{
    addTodo(todoItem){
      console.log(todoItem);
      var obj = {completed:false, item:todoItem};
      localStorage.setItem(todoItem, JSON.stringify(obj));
      this.todoItems.push(obj);
    },

    removeTodo(todoItem, index){
      localStorage.removeItem(todoItem.item);
      this.todoItems.splice(index,1);
    },

    toggleTodo(todoItem){
      if(todoItem.completed === false){
        todoItem.completed = true;
      } else{
        todoItem.completed = false;
      }
      localStorage.setItem(todoItem.item,JSON.stringify(todoItem));
    },

    clearAll(){
      localStorage.clear();
      this.todoItems = [];
    },

    // clearCompleted(){
    //   var checklen = this.todoItems.length; // 2개 (0,1)
    //   var checkId = checklen-1; //1번부터 시작
    //   this.Recursive(checklen, checkId);
    // },

    // Recursive(checklen, checkId){
    //   if(checklen>0){
    //     if(this.todoItems[checkId].completed === true){
    //       this.removeTodo(this.todoItems[checkId], checkId);
    //       checkId--; 0
    //       checklen--; 1
    //       return this.Recursive(checklen, checkId);
    //     }
    //     else{
    //       checkId--; 0
    //       checklen--; 1
    //       return this.Recursive(checklen, checkId);
    //     }
    //   }
    //   else{
    //     return;
    //   }
    // }, //재귀함수 테스트 성공

      clearCompleted(){
      var checklen = this.todoItems.length;
      var checkId = checklen-1;

      for(var i = 0; i <checklen; i++){ 
        if(this.todoItems[checkId].completed===true){
          this.removeTodo(this.todoItems[checkId], checkId);
          checkId--;
        }
        else{
          checkId--;
        }
      }
    },

    
    checkAllTodo(){
      var cnt = 0;
      var todoItem;

      for(var i = 0; i<localStorage.length; i++){
        todoItem = JSON.parse(localStorage.getItem(localStorage.key(i)));
        if(todoItem.completed === false){
        cnt++;
        }
      }   

      if(cnt>0){
        this.checkAllOrNot(todoItem, true);
      } else {
        this.checkAllOrNot(todoItem, false);
      }
      
    },

    checkAllOrNot(todoItem, value){
      for(var i = 0; i<localStorage.length; i++){
        todoItem = JSON.parse(localStorage.getItem(localStorage.key(i)));
        todoItem.completed = value;
        localStorage.setItem(todoItem.item, JSON.stringify(todoItem));
        this.todoItems[i] = todoItem;
      }
    },

    changeFilter(value){
      this.filterType = value;
      // console.log(this.filterType);
    }

  },
    components: {
      'TodoHeader' : TodoHeader,
      'TodoInput' : TodoInput,
      'TodoList' : TodoList,
      'TodoFooter' : TodoFooter
  }
}
</script>

<style>
html {
	height: 100%;
}

body {
    display: flex;
    flex-wrap: nowrap;
    justify-content: center;
    background-color: #F5F5F5;
    min-height: 100%;
}

.todo-wrapper {
    justify-content: center;
    margin-top: 3rem;
    min-width: 600px;
}

.todo-box {
    background-color: white;
    border: 1px solid #ddd;
}

button {
    background-color: transparent;
    border: 0;
}
</style>
