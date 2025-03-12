<template>
  <div class="todo-list">
    <div style="text-align: left ; background: linear-gradient(to right, #87CEEB, #cccccc); color: white">
      <h1>ToDoList</h1>
    </div>
    <form @submit.prevent="addTodo" style="text-align: center">
      <el-input style="width: 260px" type="text" v-model="newTodo" placeholder="添加待办事项"></el-input>
      <el-button type="primary" native-type="submit">提交</el-button>
    </form>
<!--用flex布局1-->
    <div style="display: flex; margin-left: 30px; margin-right: 30px">
      <div style="flex: 50%">
        <div style="background: linear-gradient(to right, #87CEEB, #cccccc); color: white;">
          <h2>正在进行 ({{ pendingTodos.length }})</h2>
        </div>
        <ul>
          <li class="pendingLi" v-for="(todo, index) in pendingTodos" :key="index">
            <span>{{ todo.text }}</span>
            <button @click="completeTodo(index)"></button>
            <button @click="deletePendingTodo(index)"></button>
            <button @click="modifyTodo(index)"></button>
          </li>
        </ul>
      </div>

      <div style="flex: 10%"></div>

      <div style="flex: 50%">
        <div style="background: linear-gradient(to right, #87CEEB, #cccccc); color: white;">
          <h2>已完成 ({{ completedTodos.length }})</h2>
        </div>
        <ul>
          <li class="completedLi" v-for="(todo, index) in completedTodos" :key="index">
            {{ todo.text }}
            <button @click="deleteTodo(index)"></button>
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
      this.$prompt('请输入修改后的ToDoList', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
      }).then(({ value }) => {
        this.$message({
          type: 'success',
          message: '修改后的ToDoList是: ' + value
        });
        this.todos[idx].text=value;
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '取消输入'
        });
      });
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

ul {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
  padding: 0;
  margin: 0;
}

li {
  /* 列表项样式 */
  position: relative;
  list-style: none;
  width: calc(33.33% - 8px); /* 三列布局 */
  height: 20px;
  padding: 16px;
  border: 1px solid #e0e0e0;
  border-radius: 12px;
  background: #ffffff;
  box-shadow: 0 2px 4px rgba(0,0,0,0.05);
  transition: all 0.3s ease;

  /* 文字布局 */
  display: flex;
  align-items: center;

}

li:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(108,92,231,0.15);
  border-color: #6c5ce7;
}

/* 按钮容器 */
li button {
  opacity: 0;
  width: 32px;
  height: 32px;
  border: none;
  border-radius: 50%;
  background: #f8f9fa;
  cursor: pointer;
  transition: all 0.3s ease;
  margin-left: auto; /* 按钮靠右 */
}

li:hover button {
  opacity: 1;
}

/* 按钮图标 */
button::after {
  font-family: Arial;
  font-weight: bold;
}
.pendingLi button:nth-child(2)::after { content: '✓'; color: #6c5ce7; } /* 完成 */
.pendingLi button:nth-child(3)::after { content: '×'; color: #ff7675; } /* 删除 */
.pendingLi button:nth-child(4)::after { content: '✎'; color: #00b894; } /* 修改 */
.completedLi button:nth-child(1)::after { content: '×'; color: #ff7675; } /* 修改 */


/* 按钮悬停效果 */
button:hover {
  background: #6c5ce7 !important;
  transform: scale(1.1);
}
button:hover::after {
  color: white !important;
}

/* 文字区域 */
li span {
  flex: 1;
  padding-right: 8px;
  font-size: 16px;
  color: #2d3436;
  word-break: break-word;
}
</style>
