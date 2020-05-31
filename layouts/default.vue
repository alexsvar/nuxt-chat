<template>
  <v-app app dark>
    <v-navigation-drawer app v-model="drawer" mobile-break-point="650">
      <v-list subheader>
        <v-subheader>List of users in the room</v-subheader>
        <v-list-item v-for="u in users" :key="u.id" @click.prevent>
          <v-avatar color="success" size="36">
            <span class="white--text headline">{{u.name[0]}}</span>
          </v-avatar>
          &nbsp;&nbsp;&nbsp;
          <v-list-item-content>
            <v-list-item-title>{{u.name}}</v-list-item-title>
          </v-list-item-content>
          <v-list-item-icon>
            <v-icon :color="u.id === user.id ? 'success' : 'grey'">mdi-message</v-icon>
          </v-list-item-icon>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar app>
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
      <v-toolbar-title>Chat room: {{user.room}}</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-tooltip left>
        <template v-slot:activator="{on}">
          <v-btn icon @click="snackbar = true" v-on="on">
            <v-icon dark>mdi-arrow-left</v-icon>
          </v-btn>
        </template>
        <span>Exit</span>
      </v-tooltip>
    </v-app-bar>
    <v-content>
      <div style="height: 100%">
        
        <v-snackbar v-model="snackbar" :timeout="10000" top>
          <v-btn dark text color="success" @click="snackbar = false">Stay logged in</v-btn>
          <v-btn dark text color="error" @click="exit">Log out</v-btn>
        </v-snackbar>
        
        <nuxt/>
      </div>
    </v-content>
  </v-app>
</template>

<script>
  import {mapState, mapMutations} from 'vuex'

  export default {
    data: () => ({
      drawer: true,
      snackbar: false
    }),
    computed: mapState(['user', 'users']),
    methods: {
      ...mapMutations(['clearData']),
      exit() {
        this.$socket.emit('userLeft', this.user.id, () => {
          this.$router.push('/?message=leftChat')
          this.clearData()
        })
      }
    }
  }
</script>

