<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
    </v-app-bar>

    <v-main>
      <TodoApp/>
    </v-main>
  </v-app>
</template>

<script>
import TodoApp from './components/TodoApp';

export default {
  name: 'App',

  components: {
    TodoApp,
  },

  data: () => ({
    //
  }),
};
</script>
