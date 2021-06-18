<template>
  <v-app>
    <v-navigation-drawer app> </v-navigation-drawer>
    <v-app-bar app> Github Candidates Search </v-app-bar>
    <v-main>
      <v-container fluid style="padding: 35px">
        <SearchGitRepos @git-search="searchGitHub" :results="results" />
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
import axios from "axios";

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
  methods: {
    fetchUsers(term) {
      this.results = [];
      if (term == "") {
        return;
      }

      this.loading = true;
      let detailPromises = [];
      axios
        .get("https://api.github.com/search/users?q=".concat(term))
        .then((response) => {
          response.data.items.forEach((user) => {
            detailPromises.push(
              axios.get(user.url, {
                headers: {
                  Authorization:
                    "token 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",
                },
              })
            );
          });

          // wait unitl all user data is fetched
          Promise.all(detailPromises).then((allResults) => {
            allResults.forEach((user) => {
              this.results.push(user.data);
            });
            this.loading = false;
          });
        })
        .catch((error) => {
          console.log(error);
          return null;
        });
    },
    searchGitHub(term) {
      this.fetchUsers(term);
    },
  },
  created() {},
};
</script>
