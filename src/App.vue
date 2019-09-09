<template>
  <div id="app">
    <Header />
    <AddTodo v-on:add-todo="addTodo"/>
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" />
  </div>
</template>

<script>
import Header from './components/layout/Header';
import Todos from './components/Todos';
import AddTodo from "./components/AddTodo";
import axios from 'axios';

export default {
  name: 'app',
  components: {
    Header,
    Todos,
    AddTodo
  },
  data() {
    return {
      todos: [

      ]
    }
  },
  methods: {

    deleteTodo(id) {
      //this.todos = this.todos.filter(todo => todo.id !== id)
      axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
              .then(res => this.todos = this.todos.filter( todo => todo.id !== id))
              .catch(e => console.log(e))
    },

    addTodo(newTodo) {
        const {title, completed} = newTodo;
        axios.post('https://jsonplaceholder.typicode.com/todos', {
          title: title,
          completed: completed
        })
          .then( res => this.todos = [...this.todos, res.data])
          .catch( e => console.log(e));
    }
  },
  created() {
    axios.get('https://jsonplaceholder.typicode.com/todos?_limit=5')
            .then(res => this.todos = res.data)
            // eslint-disable-next-line no-console
            .catch(e => console.log(e))
  }
}
</script>

<style>
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }
   body {
     font-family: Arial, Helvetica, sans-serif;
     line-height: 1.4;
   }
</style>
