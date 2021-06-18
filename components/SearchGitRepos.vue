<template>
  <v-container fluid>
    <v-layout row>
      <v-flex xs12 md10>
        <v-text-field
          id="txtSearchBox"
          v-model="SearchBoxText"
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
          loading-text="Loading... Please wait"
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
export default {
  name: "SearchGitRepos",
  props: {
    results: Array,
  },
  methods: {
    onSearchClick() {
      this.$emit("git-search", this.SearchBoxText);
    },
    userProfileUrl(login) {
      return "https://github.com/".concat(login);
    },
  },
  data() {
    return {
      SearchBoxText: "",
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
