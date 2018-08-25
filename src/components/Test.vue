<template>
  <div>
  <form @submit.prevent="addTodo" class="form-inline">
    <input type="text" v-model="todoText" class="form-control mb-2 mr-sm-2" id="text">
    <button class="btn btn-primary mb-2">追加</button>
  </form>

  <table class="table table-sm">
    <thead>
    <tr>
      <th>id</th>
      <th>Todo</th>
    </tr>
    </thead>
    <tbody>
    <tr v-for="todo of todos" :key="todo.id">
      <th scope="row">{{ todo.id }}</th>
      <td>{{ todo.text }}</td>
    </tr>
    </tbody>
  </table>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Test',
  data () {
    return {
      todoText: '',
      todos: []
    }
  },
  mounted () {
    axios
      .get('http://localhost:3000/todos')
      .then(response => { this.todos = response.data })
      .catch(error => window.alert(error))
  },
  methods: {
    addTodo: function () {
      if (this.todoText.length > 0) {
        axios.post('http://localhost:3000/todos', {
          text: this.todoText
        }).then(response => {
          this.todos.push({
            id: response.data.id,
            text: response.data.text
          })
        })
      }
    }
  }
}
</script>

<style scoped>

</style>
