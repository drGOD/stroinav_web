<template>
  <v-app id="inspire">
    <router-view></router-view>
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