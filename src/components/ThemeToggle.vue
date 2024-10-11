<template>
    <div class="theme-toggle" @click="toggleTheme">
      <div class="moon-icon"></div>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';

  const isNightMode = ref(localStorage.getItem('theme') === 'night' || false);

  const toggleTheme = () => {
    isNightMode.value = !isNightMode.value;
    if (isNightMode.value) {
      document.body.classList.add('night-mode');
      document.body.classList.remove('day-mode');
      localStorage.setItem('theme', 'night');
    } else {
      document.body.classList.add('day-mode');
      document.body.classList.remove('night-mode');
      localStorage.setItem('theme', 'day');
    }
  };
  
  if (isNightMode.value) {
    document.body.classList.add('night-mode');
    document.body.classList.remove('day-mode');
  } else {
    document.body.classList.add('day-mode');
    document.body.classList.remove('night-mode');
  }
  </script>
  
  <style scoped>
  .theme-toggle {
    position: fixed;
    top: 20px;
    right: 20px;
    width: 50px;
    height: 50px;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .moon-icon {
    width: 30px;
    height: 30px;
    border-radius: 50%;
    background-color: var(--moon-color);
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
  }
  
  body.day-mode .moon-icon {
    background-color: black;
  }
  
  body.night-mode .moon-icon {
    background-color: white;
  }
  </style>
  