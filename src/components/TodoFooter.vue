<template> 
    <div class="todo-bottom" >
        <div class="left-items" >{{ countTotal }} Items left</div>
        <div class="button-group">
            <button type="button" class="show-all-btn" data-type="all" v-on:click="showAll()" v-bind:class="{selected:isBind==='all'}">All</button>
            <button type="button" class="show-active-btn" data-type="active" v-on:click="showActive()" v-bind:class="{selected:isBind==='active'}">Active</button>
            <button type="button" class="show-completed-btn" data-type="completed" v-on:click="showCompleted()" v-bind:class="{selected:isBind==='completed'}">Completed</button>
        </div>
        
        <button type ="button" class="delete-all-btn" v-on:click="clearTodo()">Delete All</button>
        <button type="button" class="clear-completed-btn" v-bind:propsdata="todoItems" v-on:click="clearCompleted()">Clear Completed</button>    
    </div>
    <p class="info">더블클릭 시 수정 가능!</p>
</template>

<script>


export default {
    data(){
        return {
            isBind:'all',
        }
    },
    props:['propsdata'],

    methods: {
        clearTodo(){
            this.$emit('removeAll');
        },
        clearCompleted(){
            this.$emit('clearCompleted');
        },
        showAll(){
            this.$emit('changeFilter','all' );
            this.isBind='all';
        },
        showActive(){
            this.$emit('changeFilter', 'active');
            this.isBind='active';
        },
        showCompleted(){
            this.$emit('changeFilter', 'completed');
            this.isBind='completed';
        },
    },
    computed: {
        countTotal(){
            let cnt= 0;
            for(var i = 0; i<this.propsdata.length ; i++){
                if(this.propsdata[i].completed === true){
                    cnt++;
                }
            }
            return this.propsdata.length-cnt;
        }
    }
}
</script>

<style>
.todo-bottom {
    height: 3rem;
    display: flex;
    flex-wrap: nowrap;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    padding: 0 1rem;
}

.button-group button {
    border: 1px solid #eee;
    padding: 0.2rem 0.5rem;
    margin: 0 0.5rem;
    border-radius: 8px;
    cursor: pointer;
}

.button-group button.selected {
    border: 2px solid rosybrown;
    padding: 0.2rem 0.5rem;
    margin: 0 0.5rem;
    border-radius: 8px;
}

p.info {
    margin-top: 1.5rem;
    text-align: center;
    color: #ccc;
}

.clear-completed-btn:hover {
    font-style: italic;
    text-decoration: underline;
    cursor: pointer;
}

.delete-all-btn:hover {
    font-style: italic;
    text-decoration: underline;
    cursor: pointer;
}

</style>