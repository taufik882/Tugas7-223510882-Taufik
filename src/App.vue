<template>
  <div id="app" class="container">
    <div class="todo-app">
      <h1 class="title">Daftar Todo List Anda</h1>
      <div class="input-group">
        <input
          v-model="newTodo"
          @keyup.enter="addNewTodo"
          placeholder="Tambahkan Todo List"
          class="input"
        />
        <button @click="addNewTodo" class="add-button">Tambah</button>
      </div>
      <ul class="todo-list">
        <li v-for="(todo, index) in todos" :key="index" class="todo-item">
          <div class="todo-text">
            <input
              type="checkbox"
              :checked="todo.completed"
              @change="toggleTodoStatus(index)"
              class="checkbox"
            />
            <span :class="{ 'completed': todo.completed }">{{ todo.text }}</span>
          </div>
          <button @click="removeTodoItem(index)" class="remove-button">Hapus</button>
        </li>
      </ul>
      <p class="incomplete-count">Total Todo List Yang Belum Selesai: {{ incompleteTodosCount }}</p>
    </div>
  </div>
</template>

<script>
import { ref, computed } from 'vue';
import { useTodoStore } from './stores/todoStore';

export default {
  setup() {
    const newTodo = ref('');
    const todoStore = useTodoStore();

    const addNewTodo = () => {
      if (newTodo.value.trim()) {
        todoStore.addTodo(newTodo.value.trim());
        newTodo.value = '';
      }
    };

    const removeTodoItem = (index) => {
      todoStore.removeTodo(index);
    };

    const toggleTodoStatus = (index) => {
      todoStore.toggleTodo(index);
    };

    const incompleteTodosCount = computed(() => todoStore.incompleteTodosCount);

    return {
      newTodo,
      todos: todoStore.todos,
      addNewTodo,
      removeTodoItem,
      toggleTodoStatus,
      incompleteTodosCount,
    };
  },
};
</script>

<style scoped>
.container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
  box-sizing: border-box;
}

.todo-app {
  background-color: #ffffff;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  padding: 20px;
  max-width: 400px;
  width: 100%;
  box-sizing: border-box;
}

.title {
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 20px;
  text-align: center;
  color: #00796b;
}

.input-group {
  display: flex;
  margin-bottom: 20px;
}

.input {
  flex: 1;
  padding: 10px;
  border: 1px solid #b2dfdb;
  border-radius: 4px 0 0 4px;
  outline: none;
  transition: border-color 0.3s;
}

.input:focus {
  border-color: #00796b;
}

.add-button {
  padding: 10px;
  background-color: #00796b;
  border: 1px solid #00796b;
  color: white;
  border-radius: 0 4px 4px 0;
  cursor: pointer;
  transition: background-color 0.3s, border-color 0.3s;
}

.add-button:hover {
  background-color: #004d40;
  border-color: #004d40;
}

.todo-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.todo-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  border-bottom: 1px solid #eee;
  transition: background-color 0.3s;
}

.todo-item:hover {
  background-color: #e0f2f1;
}

.todo-item:last-child {
  border-bottom: none;
}

.todo-text {
  display: flex;
  align-items: center;
}

.checkbox {
  margin-right: 10px;
}

.completed {
  text-decoration: line-through;
  color: #888;
}

.remove-button {
  background-color: transparent;
  border: none;
  color: #d32f2f;
  cursor: pointer;
  transition: color 0.3s;
}

.remove-button:hover {
  color: #b71c1c;
}

.incomplete-count {
  text-align: center;
  margin-top: 20px;
  color: #00796b;
}

@media (max-width: 600px) {
  .todo-app {
    padding: 15px;
    max-width: 100%;
  }

  .title {
    font-size: 20px;
  }

  .input-group {
    flex-direction: column;
  }

  .input {
    border-radius: 4px;
  }

  .add-button {
    margin-top: 10px;
    border-radius: 4px;
  }

  .todo-item {
    flex-direction: column;
    align-items: flex-start;
  }

  .remove-button {
    margin-top: 10px;
    align-self: flex-end;
  }
}
</style>
