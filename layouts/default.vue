<template>
  <v-app app dark>
    <v-navigation-drawer app v-model="drawer" mobile-break-point="650">
      <v-list subheader>
        <v-subheader>List of users in the room</v-subheader>
        <v-list-item v-for="u in users" :key="u.id" @click.prevent>
          <v-avatar color="success" size="36">
            <span class="white--text headline">{{u.name[0]}}</span>
          </v-avatar>&nbsp;
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
      <v-toolbar-title>Chat room {{user.room}}</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn icon @click="exit">
        <v-icon dark>mdi-arrow-left</v-icon>
      </v-btn>
    </v-app-bar>
    <v-content>
      <div style="height: 100%">
        <nuxt/>
      </div>
    </v-content>
  </v-app>
</template>

<script>
  import {mapState, mapMutations} from 'vuex'

  export default {
    data: () => ({
      drawer: true
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
