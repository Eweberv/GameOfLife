<template>
  <v-app>
    <v-app-bar dark color="green" app>
      <v-toolbar-title>
        Workshop
      </v-toolbar-title>
    </v-app-bar>

    <v-content>
      <router-view></router-view>
    </v-content>

  </v-app>
</template>

<script>
    export default {
  name: 'App',

  components: {
  },

  data: () => ({
    //
  }),
};
</script>
