<template>
  <div class="family-tree-table">
    <!-- Убедимся, что root не null перед отрисовкой -->
    <div v-if="root" class="tree-container">
      <div class="family-tree">
        <!-- Дети на верхнем уровне -->
        <div class="children">
          <div v-for="child in getChildren(root.relationships)" :key="child.id" class="family-member">
            <div>{{ child.name }} {{ child.surname }} ({{ child.relationship_type }})</div>
            <FamilyTree :root="child" :isRoot="false" /> <!-- Рекурсивный компонент для детей -->
          </div>
        </div>

        <!-- Семья в центре: сёстры слева, корень по центру, братья справа -->
        <div class="family-tree-row">
          <!-- Сестры слева -->
          <div class="siblings left">
            <div v-for="relative in getSisters(root.relationships)" :key="relative.id" class="family-member">
              <div>{{ relative.name }} {{ relative.surname }} ({{ relative.relationship_type }})</div>
              <FamilyTree :root="relative" :isRoot="false" /> <!-- Рекурсивный компонент для сестёр -->
            </div>
          </div>

          <!-- Корень в центре, отображаем только для корневого элемента -->
          <div v-if="isRoot" class="root">
            <div>{{ root.name }} {{ root.surname }}</div>
          </div>

          <!-- Братья справа -->
          <div class="siblings right">
            <div v-for="relative in getBrothers(root.relationships)" :key="relative.id" class="family-member">
              <div>{{ relative.name }} {{ relative.surname }} ({{ relative.relationship_type }})</div>
              <FamilyTree :root="relative" :isRoot="false" /> <!-- Рекурсивный компонент для братьев -->
            </div>
          </div>
        </div>

        <!-- Родители ниже -->
        <div class="parents">
          <div v-for="parent in getParents(root.relationships)" :key="parent.id" class="family-member">
            <div>{{ parent.name }} {{ parent.surname }} ({{ parent.relationship_type }})</div>
            <FamilyTree :root="parent" :isRoot="false" /> <!-- Рекурсивный компонент для родителей -->
          </div>
        </div>
      </div>
    </div>

    <!-- Показываем сообщение, если root еще не загружен -->
    <div v-else>Загрузка данных...</div>
  </div>
</template>

<script>
export default {
  name: 'FamilyTree',
  props: {
    root: Object,   // Принимаем данные от родительского компонента
    isRoot: {       // Новый пропс для отображения корня
      type: Boolean,
      default: true  // По умолчанию отображаем корень
    }
  },
  methods: {
    // Функция для фильтрации сестер
    getSisters(relationships) {
      return relationships.filter(rel => rel.relationship_type === 'Сестра');
    },
    // Функция для фильтрации братьев
    getBrothers(relationships) {
      return relationships.filter(rel => rel.relationship_type === 'Брат');
    },
    // Функция для фильтрации родителей
    getParents(relationships) {
      return relationships.filter(rel => rel.relationship_type === 'Мать' || rel.relationship_type === 'Отец');
    },
    // Функция для фильтрации детей
    getChildren(relationships) {
      return relationships.filter(rel => rel.relationship_type === 'Ребенок');
    }
  }
};
</script>

<style scoped>
.family-tree-table {
  margin: 20px;
  padding: 20px;
  text-align: center;
  align-items: center;
  vertical-align: middle;
}

.tree-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  vertical-align: middle;
}

.family-tree {
  display: grid;
  grid-template-columns: 1fr;
  grid-template-areas:
    "children"
    "family"
    "parents";
  gap: 20px;
  
}

.family-tree-row {
  display: grid;
  grid-template-columns: 1fr 200px 1fr;
  gap: 20px;
  grid-area: family;
}

.siblings {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.left {
  justify-content: flex-end;
}

.right {
  justify-content: flex-start;
}

.root {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.family-member {
  margin: 5px;
  padding: 10px;
  background-color: #f1f1f1;
  border-radius: 5px;
  text-align: center;
}

.parents,
.children {
  display: flex;
  justify-content: center;
  gap: 15px;
}

.parents .family-member,
.children .family-member {
  background-color: #e1f7d5;
}

.children {
  grid-area: children;
}
</style>
