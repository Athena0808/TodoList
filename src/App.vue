<script setup>
import { ref, computed,onMounted } from 'vue';
const todos = ref([
  { title: '📦To buy products', completed: false },
  { title: '🧽To clean the house', completed: false }
]);
onMounted(() => {
  const savedTodos = localStorage.getItem('todos');
  if (savedTodos) {
    todos.value = JSON.parse(savedTodos);
    console.log(todos.value); 
  }
});


const newTodo = ref('');
const filter = ref('all'); 
const addTodo=()=>{
  if(newTodo.value.trim()!==''){
    todos.value.push({ title: newTodo.value, completed: false });
    newTodo.value='';
    saveTodos();
  }
}
const toggleCompletion = (index) => {
  todos.value[index].completed = !todos.value[index].completed;
  saveTodos();
};
const remaining = computed(() => {
  return todos.value.filter(todo => !todo.completed).length;
});
const filteredTodos = computed(() => {
  if (filter.value === 'active') {
    return todos.value.filter(todo => !todo.completed);
  } else if (filter.value === 'completed') {
    return todos.value.filter(todo => todo.completed);
  }
  return todos.value;
});
const filterTodos = (filterValue) => {
  filter.value = filterValue;
};
const clearCompleted = () => {
  todos.value = todos.value.filter(todo => !todo.completed);
  saveTodos();
};
const removeTodo = (index) => {
  todos.value.splice(index, 1);
  saveTodos();
};
const saveTodos = () => {
  localStorage.setItem('todos', JSON.stringify(todos.value));
};
</script>

<template>
<div class="title-container">
  <h1>📓</h1>
  <h1>Todo List</h1>
</div>
<div class="todo-app">
    <ul class="todo-list">
      <li class="todo-input-item">
          <input 
            v-model="newTodo" 
            @keyup.enter="addTodo" 
            type="text" 
            placeholder="Write your todos here..." />
      </li>
      <li v-for="(todo, index) in filteredTodos" :key="index" class="todo-item">
        <input type="checkbox" :id="'item-' + index" v-model="todo.completed">
        <label :for="'item-' + index" :class="{ completed: todo.completed }">{{ todo.title }}</label>
        <button class="delete-btn" @click="removeTodo(index)">❌</button>
      </li>
        <!--
        <input type="checkbox" v-model="todo.completed" />
        <label :class="{ completed: todo.completed }">{{ todo.title }}</label>
      -->
    </ul>
  <div class="todo-footer">
        <span>{{ remaining }} item(s) left</span>
        <div class="filters">
          <button @click="filterTodos('all')" :class="{ active: filter === 'all' }">all</button>
          <button @click="filterTodos('active')" :class="{ active: filter === 'active' }">active</button>
          <button @click="filterTodos('completed')" :class="{ active: filter === 'completed' }">completed</button>
        </div>
        <button 
        :style="{ visibility: filter === 'active' ? 'hidden' : 'visible' }" 
        @click="clearCompleted" 
        class="clear-btn">
        Clear Completed
      </button>
  </div>
  </div>
</template>

