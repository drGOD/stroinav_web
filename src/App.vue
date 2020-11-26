<template>
  <v-app id="inspire">

    <v-navigation-drawer v-model="drawer" app>
      <v-sheet color="grey lighten-4" class="pa-4">
        <v-list-item class="px-2">
          <v-list-item-avatar>
            <v-img src="https://randomuser.me/api/portraits/men/85.jpg"></v-img>
          </v-list-item-avatar>

          <v-list-item-title>Demo User</v-list-item-title>
        </v-list-item>

        <div>demo@user.com</div>
      </v-sheet>

      <v-divider></v-divider>

      <v-list>
        <v-list-item v-for="[icon, text, link] in links" :key="icon" link v-on:click="$router.push(link)">
          <v-list-item-icon>
            <v-icon>{{ icon }}</v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>{{ text }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-main>
      <v-container class="py-8 px-6" fluid>
        <v-row>
          <v-col cols="12">
            <v-card>
              <router-view></router-view>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data: () => ({
    drawer: null,
    links: [
      ["mdi-city-variant", "Объекты строительства", "objects"],
      /*["mdi-account-supervisor", "Субъекты", "subjects"],
      ["mdi-clipboard-list-outline", "Журнал событий", "journal"],
      ["mdi-cog", "Настройки", "settings"],*/
    ],
  }),
};
</script>