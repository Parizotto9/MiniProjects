<template>
  <v-app>
    <v-app-bar app color="bgHeader" dark>
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="./assets/logo.png"
          transition="scale-transition"
          width="40"
        />
      </div>

      <v-spacer></v-spacer>
    </v-app-bar>

    <v-main class="main 12">
      <Calendar class="alo" />
    </v-main>
  </v-app>
</template>

<script>
import Calendar from "./components/Calendar";

export default {
  name: "App",

  components: {
    Calendar,
  },

  data: () => ({
    //
  }),
};
</script>

<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  font-family: "Quicksand", sans-serif;
}
.main {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.bgHeader {
  background-color: #152837;
}
</style>
