<template>
  <div id="app" class="app-container">
    <!-- Левая часть: дерево -->
    <div class="main-area">
      <h1>Генеалогическое древо</h1>
      <!-- Передаем загруженные данные в компонент FamilyTree -->
      <FamilyTree :root="root" />
    </div>

    <!-- Правая панель: список участников -->
    <div class="sidebar">
      <h2>Участники дерева</h2>
      <UserList />
    </div>
  </div>
</template>

<script>
import UserList from './components/UserList.vue';
import FamilyTree from './components/FamilyTree.vue';

export default {
  name: 'App',
  components: {
    UserList,
    FamilyTree,
  },
  data() {
    return {
      root: null, // корневой элемент дерева
    };
  },
  async mounted() {
    try {
      // Выполняем запрос только один раз при монтировании компонента
      const response = await fetch('http://localhost:8888/family-tree/v1/relationship/1');
      this.root = await response.json();
    } catch (error) {
      console.error('Ошибка загрузки дерева:', error);
    }
  }
};
</script>

<style scoped>
.app-container {
  display: flex;
  height: 100vh;
}
.main-area {
  flex: 1;
  padding: 20px;
}
.sidebar {
  width: 20%;
  border-left: 1px solid #ccc;
  padding: 20px;
  background-color: #f9f9f9;
}
</style>
