<template>
    <v-app>
        <v-navigation-drawer permanent expand-on-hover app>
        </v-navigation-drawer>
        <v-app-bar app>
            <v-container fluid>
                <v-layout>
                    <v-flex md1>
                        <v-img
                            max-height="32"
                            max-width="32"
                            src="@/assets/GitHub-Mark-32px.png"
                        ></v-img>
                    </v-flex>
                    <v-flex md10>
                        <span>Github Candidates Search </span>
                    </v-flex>
                    <v-flex md1>
                        <div class="text-right">
                            <v-switch
                                label="Dark Mode"
                                @change="toggleDarkTheme()"
                            ></v-switch>
                        </div>
                    </v-flex>
                </v-layout>
            </v-container>
        </v-app-bar>
        <v-main>
            <v-container fluid style="padding: 35px">
                <SearchGitRepos />
            </v-container>
        </v-main>

        <v-footer app style="text-align: center" dark padless>
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
    methods: {
        toggleDarkTheme() {
            this.$vuetify.theme.dark = !this.$vuetify.theme.dark;
        },
    },
    data() {
        return {
            results: [],
        };
    },
};
</script>
