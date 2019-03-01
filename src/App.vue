<template>
  <div id="app">
    <Header />
    <AddTodo v-on:add-todo="addTodo"/>
    <Todos v-bind:todos="todos" v-on:del-todo="deletetodo" />
  </div>
</template>

<script>
import Todos from './components/Todos'
import AddTodo from './components/AddTodo'
import Header from './components/layout/Header'
import axios from 'axios'


export default {
  name: 'app',
  components: {
    AddTodo,
    Header,
    Todos
  },
  data(){
    return{
      todos:[]
    }
  },
  methods:{
    deletetodo(id){

      axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
        .then(res => this.todos = this.todos.filter(todo => todo.id !== id))
        .catch(err => console.log(err))

      // this.todos = this.todos.filter(todo => todo.id !== id)

    },
    addTodo(newTodo){
      const {title, completed } = newTodo

      axios.post('https://jsonplaceholder.typicode.com/todos', {
        title,
        completed
      })
        .then(res => this.todos = [...this.todos, res.data])
        .catch(err => console.log(err))


      // this.todos = [...this.todos, newTodo]

    }
  },
  created(){
    axios.get('https://jsonplaceholder.typicode.com/todos?_limit=5').then(res => this.todos = res.data).catch(err => console.log(err))
  }
}
</script>

<style>
  *{
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  body{
    font-family: Arial, Helvetica, sans-serif;
    line-height: 1.4;
  }

  .btn{
    display: inline-block;
    border:none;
    background: #555;
    padding: 7px 20px;
    cursor: pointer;
  }
  .btn:hover{
    background: #666;
  }
</style>
