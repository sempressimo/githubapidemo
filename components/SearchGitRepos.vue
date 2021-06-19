<template>
  <v-container fluid>
    <v-layout row>
      <v-flex xs12 md10>
        <v-text-field
          id="txtSearchBox"
          v-model="SearchBoxText"
          v-on:keyup.enter="onEnter"
          dense
          outlined
          label="Enter Name or Email"
        ></v-text-field>
      </v-flex>
      <v-flex xs1> &nbsp; </v-flex>
      <v-flex xs12 md1>
        <v-btn block @click="onSearchClick()" depressed color="primary">
          Search
        </v-btn>
      </v-flex>
    </v-layout>
    <v-layout row>
      <v-flex xs12>
        <v-data-table
          item-key="login"
          :headers="headers"
          :items="results"
          :items-per-page="10"
          :loading="this.isLoading"
          :loading-text="loadingText"
        >
          <template v-slot:item.login="{ item }">
            <a target="_blank" :href="userProfileUrl(item.login)">{{
              item.login
            }}</a>
          </template>
          <template v-slot:item.avatar_url="{ item }">
            <v-avatar
              ><img :src="item.avatar_url" alt="" width="50" height="50"
            /></v-avatar>
          </template>
        </v-data-table>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import axios from "axios";

export default {
  name: "SearchGitRepos",
  computed: {},
  methods: {
    fetchUsers(term) {
      if (term == "") {
        return;
      }

      let tempResults = [];
      this.isLoading = true;
      let detailPromises = [];

      axios
        .get("https://api.github.com/search/users?q=".concat(term))
        .then((response) => {
          response.data.items.forEach((user) => {
            detailPromises.push(
              axios.get(user.url, {
                headers: {
                  Authorization:
                    "token  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",
                },
              })
            );
          });

          // wait unitl all user data is fetched
          Promise.all(detailPromises).then((allResults) => {
            allResults.forEach((user) => {
              tempResults.push(user.data);
            });
            this.isLoading = false;
            this.results = tempResults;
          });
        })
        .catch((error) => {
          console.log(error);
          return null;
        });
    },
    searchGitHubUsers() {
      this.fetchUsers(this.SearchBoxText);
    },
    onEnter: function () {
      this.searchGitHubUsers();
    },
    onSearchClick() {
      this.searchGitHubUsers();
    },
    userProfileUrl(login) {
      return "https://github.com/".concat(login);
    },
  },
  data() {
    return {
      results: [],
      SearchBoxText: "",
      isLoading: false,
      loadingText: "Please wait...",
      headers: [
        {
          text: "Login",
          align: "start",
          sortable: false,
          value: "login",
        },
        {
          text: "Avatar",
          value: "avatar_url",
          sortable: false,
        },
        {
          text: "Full Name",
          value: "name",
        },
        {
          text: "Email",
          value: "email",
        },
        {
          text: "Public Repos #",
          sortable: true,
          value: "public_repos",
        },
        {
          text: "Location",
          value: "location",
        },
        {
          text: "Created At",
          value: "created_at",
        },
        {
          text: "Last Update",
          value: "updated_at",
        },
      ],
    };
  },
};
</script>
