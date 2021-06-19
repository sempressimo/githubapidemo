<template>
  <v-app>
    <v-navigation-drawer app> </v-navigation-drawer>
    <v-app-bar app> Github Candidates Search </v-app-bar>
    <v-main>
      <v-container fluid style="padding: 35px">
        <SearchGitRepos />
      </v-container>
    </v-main>

    <v-footer app style="text-align: center">
      <v-container fluid>
        <v-layout>
          <v-flex md2></v-flex>
          <v-flex md8 style="font-size: small">
            (2021) Ismael Placa, All Rights Reserved.
          </v-flex>
          <v-flex md2></v-flex>
        </v-layout>
      </v-container>
    </v-footer>
  </v-app>
</template>

<script>
import SearchGitRepos from "./components/SearchGitRepos.vue";

export default {
  name: "App",
  components: {
    SearchGitRepos,
  },
  data() {
    return {
      results: [],
    };
  },
};
</script>
