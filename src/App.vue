<template>
  <v-app>
    <v-main>
      <v-container>
        <h1>Дерево рубрик</h1>
        <RubricTree />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import RubricTree from './components/RubricTree.vue';

export default {
  name: 'App',
  components: {
    RubricTree,
  },
};
</script>
