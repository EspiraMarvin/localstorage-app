<template>
  <div id="app" class="card center_div">
    <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.2.0/css/all.css"
          integrity="sha384-hWVjflwFxL6sNzntih27bfxkr27PmbbK/iSvJ+a4+0owXq79v+lsFkW54bOGbiDQ" crossorigin="anonymous">
    <div class="sticky-top bg-light">
      <Header></Header>
      <AddTodoItem v-on:add-todo-event="addTodoMethod" v-on:edit-todo-event="editTodoItemEvent" v-bind:edit_data="edit_data"></AddTodoItem>
    </div>
    <div class="overflow-auto">
      <Todos v-bind:todo_items="todo_items" v-on:del-todo-event="deleteTodoItem" v-on:mark-todo-event="markComplete" v-on:edit-todo-event="editTodoItem"/>
    </div>
  </div>
</template>

<script>

  import Header from "./components/Header";
  import AddTodoItem from "./components/AddTodoItem";
  import Todos from "./components/Todos";

export default {
  name: 'App',
  components: {
    Header,
    Todos,
    AddTodoItem
  },
  data(){
    return{
      todo_items: [],
      edit_data:{
        title:'',
        id:''
      }
    }
  },
  methods:{
    addTodoMethod(newTodoItem){
      //spread operator adds value to the end of the array
      // this.todo_items = [...this.todo_items, newTodoItem]
      //Use unshift - it modifies the existing array by adding new value to the front
      //  this.todo_items.unshift(newTodoItem);
      this.todo_items.unshift(newTodoItem)
    },
    deleteTodoItem(id){
      this.todo_items = this.todo_items.filter(todo_item => todo_item.id !== id);
    },
    markComplete(id){
      //find index of this id's object
      var objIndex = this.todo_items.findIndex(obj => obj.id === id);
      //reverse true... false
      this.todo_items[objIndex].completed = !this.todo_items[objIndex].completed;
    },
    editTodoItem(id){
      //find index of this id's object
      var objIndex = this.todo_items.findIndex(obj => obj.id === id);
      console.log(objIndex);
      this.edit_data.title = this.todo_items[objIndex].title;
      this.edit_data.id = id;
    },
    editTodoItemEvent(todoItem){
      //find index of ths id's object
      var objIndex = this.todo_items.findIndex(obj => obj.id === todoItem.id);
      //update the item
      this.todo_items[objIndex].title = todoItem.title;
    }
  },
  mounted() {
    console.log('App Mounted')
    if (localStorage.getItem('todo_items')){
      this.todo_items = JSON.parse(localStorage.getItem('todo_items'));
    }
  },
  watch: {
    todo_items: {
      handler(){
        console.log('Todo Items array changed!');
        localStorage.setItem('todo_items', JSON.stringify(this.todo_items))
      },
      deep: true,
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.center_div{
  margin: 0 auto;
  width: 80%
}
</style>
