<template>
    <div class="tree-node">
      <div class="person">
        <h3>{{ person.name }} {{ person.surname }}</h3>
        <p>Отчество: {{ person.patronymic }}</p>
        <p>Пол: {{ person.sex }}</p>
        <p v-if="person.relationship_type">Отношение: {{ person.relationship_type }}</p>
      </div>
      <div class="relationships" v-if="person.relationships.length">
        <TreeNode v-for="relative in person.relationships" :key="relative.id" :person="relative" />
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'TreeNode',
    props: {
      person: {
        type: Object,
        required: true,
      },
    },
  };
  </script>
  
  <style scoped>
  .tree-node {
    margin-left: 20px;
    padding-left: 10px;
    border-left: 2px solid #ccc;
  }
  
  .person {
    margin-bottom: 10px;
    padding: 10px;
    background-color: #f1f1f1;
    border-radius: 5px;
  }
  
  .relationships {
    margin-top: 10px;
  }
  </style>
  