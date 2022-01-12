<template>
  <v-app>
    <v-app-bar
      app
      color="#405664"
      dark
    >
      <div class="d-flex align-center">
        <h1
          class="shrink mt-1 hidden-sm-and-down"
          contain> Task Manager</h1>
      </div>
    </v-app-bar>

    <v-main class="page">
      <router-view/>
    </v-main>
  </v-app>
</template>

<script>

export default {
  name: 'App',

  data: () => ({
    //
  }),
};
</script>

<style scoped>
  .page{
    padding-top: 2%;
    padding-right: 3%;
    padding-left: 3%;
    background: url("./assets/waves.svg");
    background-position: center;
    background-size: cover;
    width: 100%;
    height: 100vh;
    position: relative;
    overflow: hidden;
  }
</style>
