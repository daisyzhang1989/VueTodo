<script>
import ToDoList from './components/ToDoList.vue';
import AddTodo from './components/AddTodo.vue';
import EditTodo from './components/EditTodo.vue';

export default {
  components: {
    ToDoList,
    AddTodo,
    EditTodo
  },

  data() {
    return {
      todos:[],
      editMode: false,
      editTodo: null,
      endpoint: 'http://localhost:4000/todos'
    }
  },

  computed: {
    totalTodos() {
      return this.todos.length
    },
    pendingTodos() {
      return this.todos.filter((todo) => !todo.completed).length
    },
    completedTodos() {
      return this.todos.filter((todo) => todo.completed).length
    }
  },
   
  methods: {
    async deleteTodo(todoId) {
      if(confirm('Are you sure?')){
        const res = await fetch(`${this.endpoint}/${todoId}`, {
          method: 'DELETE'
        })
        res.status === 200 
        ? (this.todos = this.todos.filter((todo) => todo.id !== todoId))
        : alert('Failed to delete')
      }
    },
    async addTodo(todo) {
      const res = await fetch(this.endpoint, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(todo)
      })
      const data = await res.json()
      this.todos.push(data)
    },
    handleEdit(todo) {
      this.editMode = true
      this.editTodo = todo
    },
    async submitEdit(editedTodo) {
      const res = await fetch(`${this.endpoint}/${editedTodo.id}`, {
        method: 'PUT',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(editedTodo)
      })
      const data = await res.json()

      this.todos = this.todos.map((todo) => todo.id === editedTodo.id ?
      {...todo, text: data.text} : todo)
      this.editMode = false
      this.editTodo = null
    },
    async fetchTodos() {
        const response = await fetch(this.endpoint)
        const data = await response.json()
        this.todos = data
        return data
    },
    async toggleComplete(todo) {
      todo.completed = !todo.completed
      todo.dateCompleted = new Date().toDateString()
      const res = await fetch(`${this.endpoint}/${todo.id}`, {
        method: 'PUT',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(todo)
      })
      const data = await res.json()
      this.todos = this.todos.map((t) => t.id === todo.id ? data : t)
      console.log(data)
    }
  },

  async created() {
    this.todos = await this.fetchTodos()
  }
}
</script>

<template>
  <div>
    <EditTodo v-if="editMode" :editTodo="editTodo" @submit-edit="submitEdit" :key="editTodo.id"/>
    <AddTodo @add-todo="addTodo" />
    <br />
    <h4>Total Todos:{{ totalTodos }} | Pending: {{ pendingTodos }} | Completed: {{ completedTodos }}</h4>
    <ToDoList :todos="todos" @delete-todo="deleteTodo" @edit-todo="handleEdit" @toggle-complete="toggleComplete"/>
  </div>
</template>