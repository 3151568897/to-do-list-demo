<template>
  <div id="app">
    <div class="todo-container">
      <h1>Todo List</h1>
      <div class="input-section">
        <input
            type="text"
            v-model="newTodo"
            @keyup.enter="addTodo"
            placeholder="输入新任务..."
            class="todo-input"
        >
        <button @click="addTodo" class="add-btn">添加</button>
      </div>

      <div class="todo-list">
        <div
            v-for="todo in filteredTodos"
            :key="todo.id"
            class="todo-item"
            :class="{ completed: todo.completed }"
        >
          <input
              type="checkbox"
              v-model="todo.completed"
              class="checkbox"
          >
          <span class="todo-text">{{ todo.text }}</span>
          <button @click="removeTodo(todo.id)" class="delete-btn">×</button>
        </div>
      </div>

      <div class="filter-buttons">
        <button @click="filter = 'all'" :class="{ active: filter === 'all' }">全部</button>
        <button @click="filter = 'active'" :class="{ active: filter === 'active' }">未完成</button>
        <button @click="filter = 'completed'" :class="{ active: filter === 'completed' }">已完成</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: '',
      todos: [],
      filter: 'all'
    }
  },
  computed: {
    filteredTodos() {
      switch (this.filter) {
        case 'active':
          return this.todos.filter(t => !t.completed)
        case 'completed':
          return this.todos.filter(t => t.completed)
        default:
          return this.todos
      }
    }
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim()) {
        this.todos.push({
          id: Date.now(),
          text: this.newTodo.trim(),
          completed: false
        })
        this.newTodo = ''
      }
    },
    removeTodo(id) {
      this.todos = this.todos.filter(t => t.id !== id)
    }
  }
}
</script>

<style>
#app {
  max-width: 600px;
  margin: 20px auto;
  padding: 20px;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
}

.todo-container {
  background: #f8f9fa;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

h1 {
  color: #2c3e50;
  text-align: center;
  margin-bottom: 25px;
}

.input-section {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.todo-input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 16px;
}

.add-btn {
  padding: 10px 20px;
  background: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background 0.3s;
}

.add-btn:hover {
  background: #3aa876;
}

.todo-list {
  margin-bottom: 20px;
}

.todo-item {
  display: flex;
  align-items: center;
  padding: 12px;
  background: white;
  margin-bottom: 8px;
  border-radius: 4px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

.checkbox {
  margin-right: 10px;
}

.todo-text {
  flex: 1;
  font-size: 16px;
}

.completed .todo-text {
  text-decoration: line-through;
  color: #999;
}

.delete-btn {
  background: none;
  border: none;
  color: #ff6b6b;
  font-size: 20px;
  cursor: pointer;
  padding: 0 8px;
}

.delete-btn:hover {
  color: #ff5252;
}

.filter-buttons {
  display: flex;
  gap: 10px;
  justify-content: center;
}

.filter-buttons button {
  padding: 6px 12px;
  border: 1px solid #ddd;
  border-radius: 4px;
  background: white;
  cursor: pointer;
}

.filter-buttons button.active {
  background: #42b983;
  color: white;
  border-color: #42b983;
}
</style>
