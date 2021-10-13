<template>
  <v-app>
    <v-app-bar
        app
        color="#00BFF77F"
        absolute
    >
      <v-app-bar-nav-icon></v-app-bar-nav-icon>

      <v-toolbar-title>某个可以发帖的网站</v-toolbar-title>

      <v-spacer></v-spacer>

      <v-menu
          bottom
          right
      >
        <template v-slot:activator="{ on, attrs }">
          <v-btn
              icon
              color="yellow"
              v-bind="attrs"
              v-on="on"
          >
            <v-icon>mdi-dots-vertical</v-icon>
          </v-btn>
        </template>

        <v-list dense>
          <v-list-item-group>
            <v-list-item href="/about">
              <v-list-item-title>about</v-list-item-title>
            </v-list-item>
            <v-list-item href="/markdown">
              <v-list-item-title>markdown</v-list-item-title>
            </v-list-item>
            <v-list-item href="/">
              <v-list-item-title>home</v-list-item-title>
            </v-list-item>
          </v-list-item-group>
        </v-list>
      </v-menu>

    </v-app-bar>

    <router-view></router-view>

  </v-app>
</template>

<script>

export default {
  name: 'App',

  components: {},

  data: () => ({
  }),

  methods:{
  }
};
</script>
