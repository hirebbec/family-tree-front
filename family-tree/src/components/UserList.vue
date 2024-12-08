<template>
    <div>
      <ul>
        <li v-for="user in users" :key="user.id" class="user-item">
          <h3>{{ user.name }} {{ user.surname }}</h3>
          <p>Отчество: {{ user.patronymic }}</p>
          <p>Пол: {{ user.sex }}</p>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  export default {
    name: 'UserList',
    data() {
      return {
        users: [],
      };
    },
    async mounted() {
      try {
        const response = await fetch('http://localhost:8888/family-tree/v1/person/all');
        this.users = await response.json();
      } catch (error) {
        console.error('Ошибка загрузки пользователей:', error);
      }
    },
  };
  </script>
  
  <style scoped>
  .user-item {
    margin-bottom: 15px;
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 5px;
    background-color: #fff;
  }
  </style>
  