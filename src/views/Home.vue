<template>
  <div id="app">
    <AddTodo v-on:add-todo="addTodo" class="addTodo"/>
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" class="todos" />
  </div>
</template>

<script>
import AddTodo from '../components/AddTodo';
import Todos from '../components/Todos';
import axios from 'axios';

export default {
  name: 'Home',
  components: {
    AddTodo,
    Todos
  },
  data() {
    return {
      todos: []
    }
  },
  methods: {
    deleteTodo(id) {
      axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
        .then(this.todos = this.todos.filter(todo => todo.id !== id))
        .catch(err => console.log(err));
    },
    addTodo(newTodo) {
      const { title, completed } = newTodo;

      axios.post('https://jsonplaceholder.typicode.com/todos', { 
        title,
        completed
      })
        .then(res => this.todos = [...this.todos, res.data])
        .catch(err => console.log(err))
    }
  },
  created() {
    axios.get('https://jsonplaceholder.typicode.com/todos?_limit=5')
      .then(res => this.todos = res.data)
      .catch(err => console.log(err));
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
    height: 100vh;
    background-image: url("../assets/background.jpg");
    background-position: center; 
    background-repeat: no-repeat; 
    background-size: cover;
  }

  .todos {
    width: 50%;
    margin: 0 auto;
    padding: 10px;
    border: 2px solid #655;
    border-radius: 20px;
  }

  .addTodo {
    width: 50%;
    margin: 0 auto;
    padding: 100px 10px 10px 10px;
  }

  .btn {
    display: inline-block;
    border: none;
    background: #555;
    color: #fff;
    padding: 7px 20px;
    cursor: pointer;
  }

  .btn:hover {
    background: #656565;
  }

  @media screen and (max-width: 800px) {
    .todos {
      width: 80%;
    }

    .addTodo {
      width: 80%;
    }
  }
</style>

