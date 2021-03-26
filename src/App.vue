<template>
  <v-app>
    

    <v-main>
      <Home/>
    </v-main>
  </v-app>
</template>
<style scoped>
.v-main {
  background-color: black;
}
</style>
<script>
import Home from './components/home';

export default {
  name: 'App',

  components: {
    Home,
  },

  data: () => ({
    //
  }),
};
</script>
