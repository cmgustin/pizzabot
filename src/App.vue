<template>
  <v-app>
    <!-- <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <h1>PizzaBot</h1>
      </div>
    </v-app-bar> -->

    <v-main>
      <PizzaBot/>
    </v-main>
  </v-app>
</template>

<script>
import PizzaBot from './components/PizzaBot';

export default {
  name: 'App',

  components: {
    PizzaBot,
  },

  data: () => ({
    //
  }),
};
</script>
