<template>
  <div>
    <h3>You have {{ todosCount }} Todos!</h3>
    <div>
      <input
        type="text"
        v-model="data.newTodoName"
        @keyup.enter="addTodo"
        placeholder="Add a Todo"
      />
    </div>
    <div>
      <!-- <ul>
        <li v-for="(todo, index) in data.todos" :key="todo.id">
          <span>{{ todo.name }}</span>
          <button @click="deleteTodo(index)">X</button>
        </li>
      </ul> -->
      <div v-for="(todo, index) in data.todos" :key="todo.id" class="list">
        <input type="text" v-model="todo.name" readonly disabled />
        <button @click="deleteTodo(index)">X</button>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive, watch, computed } from 'vue'
export default {
  setup () {
    let data = reactive({
      newTodoName: '',
      todos: []
    })

    let todosCount = computed(() => data.todos.length)

    const addTodo = () => {
      data.todos.push({ id: Date.now(), name: data.newTodoName })
      data.newTodoName = ''
    }

    const rudeWords = ['shit', 'fuck', 'bitch']

    const deleteTodo = index => data.todos.splice(index, 1)

    watch(data, newValue => {
      if (rudeWords.includes(newValue.newTodoName.toLowerCase())) {
        setTimeout(alert(`You must never say ${newValue.newTodoName} !!`), 3000)
        data.newTodoName = ''
      }
    })

    return { data, todosCount, addTodo, deleteTodo }
  }
}
</script>

<style scoped>
.list {
  margin-top: 10px;
  margin-bottom: 10px;
}
.list input {
  height: 20px;
}
button {
  height: 25px;
}
</style>
