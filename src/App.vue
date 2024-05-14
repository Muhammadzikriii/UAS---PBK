<template>
  <div class="todo-app" style="background-color: black; color: white;">
    <h1>My Todo List</h1>
    <div class="input-container">
      <input v-model="newTodo" @keyup.enter="addTodo" type="text" placeholder="Add a new todo" style="background-color: #333; color: white; border: 1px solid white;">
      <button @click="addTodo" class="add-btn">+</button>
    </div>
    <div>
      <label for="filterCompleted">Show only incomplete todos</label>
      <input id="filterCompleted" type="checkbox" v-model="showOnlyIncomplete">
    </div>
    <transition-group name="fade">
      <div v-for="(todo, index) in filteredTodos" :key="index" class="todo-item">
        <span :class="{ 'completed': todo.completed }" @click="toggleCompleted(index)">{{ todo.text }}</span>
        <button @click="removeTodo(index)" class="delete-btn">x</button>
      </div>
    </transition-group>
    <div class="completed-todos">Completed Todos: {{ completedTodos }}</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: '',
      todos: [],
      showOnlyIncomplete: false
    };
  },
  computed: {
    completedTodos() {
      return this.todos.filter(todo => todo.completed).length;
    },
    filteredTodos() {
      if (this.showOnlyIncomplete) {
        return this.todos.filter(todo => !todo.completed);
      } else {
        return this.todos;
      }
    }
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim() !== '') {
        this.todos.push({ text: this.newTodo, completed: false });
        this.newTodo = '';
      }
    },
    removeTodo(index) {
      this.todos.splice(index, 1);
    },
    toggleCompleted(index) {
      this.todos[index].completed = !this.todos[index].completed;
    }
  }
};
</script>

<style scoped>
.todo-app {
  font-family: Arial, sans-serif;
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
}

h1 {
  text-align: center;
  color: #fafafa;
}

.input-container {
  margin-bottom: 20px;
}

.add-btn {
  width: 30%;
  padding: 10px;
  font-size: 16px;
  border: none;
  background-color: #fafafa;
  color: #080808;
  border-radius: 4px;
  cursor: pointer;
}

.add-btn:hover {
  background-color: #4caf50;
}

.todo-item {
  display: flex;
  align-items: center;
  background-color: #040404;
  border-radius: 4px;
  margin-bottom: 10px;
  padding: 10px;
}

.todo-item span {
  flex-grow: 1;
  cursor: pointer;
}

.todo-item .completed {
  text-decoration: line-through;
}

.delete-btn {
  background-color: #f44336;
  color: #fff;
  border: none;
  padding: 5px 10px;
  border-radius: 50%;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.delete-btn:hover {
  background-color: #d32f2f;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter, .fade-leave-to {
  opacity: 0;
}

.completed-todos {
  text-align: center;
  margin-top: 20px;
}
</style>
