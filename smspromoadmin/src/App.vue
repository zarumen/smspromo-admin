<template>
  <v-app>
    <v-navigation-drawer
      fixed
      :mini-variant="miniVariant"
      :clipped="clipped"
      v-model="drawer"
      app
    >
      <v-list>
        <v-list-tile 
          value="true"
          v-for="(item, i) in items"
          :key="i"
          exact
        >
          <v-list-tile-action>
            <v-icon v-html="item.icon"></v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title v-text="item.title"></v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer>
    <v-toolbar fixed app :clipped-left="clipped">
      <v-toolbar-side-icon @click.stop="drawer = !drawer"></v-toolbar-side-icon>
      <v-btn icon @click.stop="miniVariant = !miniVariant">
        <v-icon v-html="miniVariant ? 'chevron_right' : 'chevron_left'"></v-icon>
      </v-btn>
      <v-btn icon @click.stop="clipped = !clipped">
        <v-icon>web</v-icon>
      </v-btn>
      <v-btn icon @click.stop="fixed = !fixed">
        <v-icon>remove</v-icon>
      </v-btn>
      <v-toolbar-title v-text="appName"></v-toolbar-title>
      <v-spacer></v-spacer>


    <v-btn flat @click="userSignOut" v-if="isAuthenticated">
      <v-icon left>exit_to_app</v-icon>
      Sign Out
    </v-btn>
    <v-btn
      flat
      v-for="(item, i) in toolbarItems"
      :key="item.i"
      :to="item.link">
      <v-icon left>{{ item.icon }}</v-icon>
      {{ item.title }}
    </v-btn>

    </v-toolbar>
    <v-content>
      <v-container fluid>
        <!-- Redered Content Show Here. -->
        <router-view></router-view> 
      </v-container>
    </v-content>
    <v-navigation-drawer
      temporary
      :right="right"
      v-model="rightDrawer"
      fixed
    >
      <v-list>
        <v-list-tile @click.native="right = !right">
          <v-list-tile-action>
            <v-icon>compare_arrows</v-icon>
          </v-list-tile-action>
          <v-list-tile-title>Switch drawer (click me)</v-list-tile-title>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer>


    <v-footer :fixed="fixed" app>
      <span>&copy; RGT 2017</span>
    </v-footer>
  </v-app>
</template>

<script>
  export default {
    data () {
      return {
        clipped: false,
        drawer: true,
        fixed: false,
        items: [{
          icon: 'home',
          title: 'Campaigns'
        },{
          icon: 'contacts',
          title: 'Clients'
        },{
          icon: 'settings_phone',
          title: 'Operators'
        },{
          icon: 'settings',
          title: 'Setting'
        }],
        miniVariant: false,
        right: true,
        rightDrawer: false,
        title: ''
      }
    }, 
    computed: {
      appName() {
        return this.$store.getters.appTitle
      },
      isAuthenticated() {
        return (
          this.$store.getters.getUser !== null &&
          this.$store.getters.getUser !== undefined
        );
      },
      toolbarItems() {
        return this.isAuthenticated ? [] : [
          {
            icon: "face",
            title: "Sign Up",
            link: "/singup"
          }, {
            icon: "lock_open",
            title: "Sign In",
            link: "/singin"
          }
        ];
      }
    },
    methods: {
      userSignOut() {
        this.$store.dispatch("userSignOut");
      }
    }
  }
</script>
