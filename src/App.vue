<template>
  <div class="todo-list">
    <div style="text-align: left ; background: linear-gradient(to right, #87CEEB, #cccccc); color: white">
      <h1>ToDoList</h1>
    </div>
    <form @submit.prevent="addTodo" style="text-align: center">
      <input type="text" v-model="newTodo" placeholder="添加待办事项" />
      <button type="submit">提交</button>
    </form>
<!--用flex布局1-->
    <div style="display: flex; margin-left: 30px; margin-right: 30px">
      <div style="flex: 50%">
        <div style="background: linear-gradient(to right, #87CEEB, #cccccc); color: white;">
          <h2>正在进行 ({{ pendingTodos.length }})</h2>
        </div>
        <ul>
          <li v-for="(todo, index) in pendingTodos" :key="index">
            {{ todo.text }}
            <button @click="completeTodo(index)">完成</button>
            <button @click="deletePendingTodo(index)">删除</button>
            <button @click="modifyTodo(index)">修改</button>
          </li>
        </ul>
      </div>

      <div style="flex: 10%"></div>

      <div style="flex: 50%">
        <div style="background: linear-gradient(to right, #87CEEB, #cccccc); color: white;">
          <h2>已完成 ({{ completedTodos.length }})</h2>
        </div>
        <ul>
          <li v-for="(todo, index) in completedTodos" :key="index">
            {{ todo.text }}
            <button @click="deleteTodo(index)">删除</button>
          </li>
        </ul>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: '',
      todos: [
         { text: '学习 Vue.js', completed: false },
         { text: '完成项目', completed: true },
      ],
    };
  },
  computed: {
    pendingTodos() {
      return this.todos.filter(todo => !todo.completed);
    },
    completedTodos() {
      return this.todos.filter(todo => todo.completed);
    },
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim() !== '') {
        this.todos.push({ text: this.newTodo, completed: false });
        this.newTodo = '';
      }
    },
    completeTodo(index) {
      // this.todos[index].completed = true;
      // 改成下面这种，通过pendingTodos计算属性来更新视图
      let todo = this.pendingTodos[index]
      let idx = this.todos.findIndex(t=>t.text === todo.text)
      this.todos[idx].completed = true;
    },
    deletePendingTodo(index){
      let todo = this.pendingTodos[index]
      let idx = this.todos.findIndex(t=>t.text === todo.text)
      this.todos.splice(idx,1)
    },
    //删除已完成
    deleteTodo(index){
      let todo = this.completedTodos[index]
      let idx = this.todos.findIndex(t=>t.text === todo.text)
      this.todos.splice(idx,1)
    },
    //修改todolist
    modifyTodo(index){
      let todo = this.pendingTodos[index]
      let idx = this.todos.findIndex(t=>t.text === todo.text)
      console.log(idx)
    }
  },
};
</script>

<style scoped>
/* 在这里添加样式，美化你的 TodoList */
.todo-list {

  margin: 0 auto;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  margin-bottom: 0.5em;
}
</style>
