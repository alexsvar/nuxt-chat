<template>
  <div class="wrap" justify-center align-center>
    <v-layout column justify-center align-center>
      <v-flex xs12 sm8>
        <v-card min-width="300">
          
          <v-snackbar v-model="snackbar" :timeout="5000" bottom>
            {{message}}
            <v-btn
              dark
              text
              color="success"
              @click="snackbar = false"
            >
              Close
            </v-btn>
          </v-snackbar>
          
          <v-card-title><h1>Nuxt Chat</h1></v-card-title>
          <v-card-text>
            <v-form
              ref="form"
              v-model="valid"
              lazy-validation
            >
              <v-text-field
                color="success"
                v-model="name"
                :counter="16"
                :rules="nameRules"
                label="Your Name"
                @keydown.enter="submit"
                required
              ></v-text-field>
              
              <v-text-field
                color="success"
                v-model="room"
                :rules="roomRules"
                label="Room"
                @keydown.enter="submit"
                required
              ></v-text-field>
              
              <v-btn
                :disabled="!valid"
                color="success"
                class="mr-4"
                @click="submit"
              >
                Sign In
              </v-btn>
            
            </v-form>
          </v-card-text>
        </v-card>
      </v-flex>
    </v-layout>
  </div>
</template>


<script>
  import {mapMutations} from 'vuex'

  export default {
    layout: 'empty',
    head: {
      title: 'Welcome to the NUXT CHAT'
    },
    sockets: {
      connect() {
        console.log('Socket Connected')
      }
    },
    data: () => ({
      valid: true,
      snackbar: false,
      message: '',
      name: '',
      nameRules: [
        v => !!v || 'Name is required',
        v => (v && v.length <= 10) || 'Name must be less than 16 characters',
      ],
      room: '',
      roomRules: [v => !!v || 'Room is required'],
      lazy: false,
    }),
    mounted() {
      const {message} = this.$route.query
      if (message === 'noUser') {
        this.message = 'Enter the data'
      } else if (message === 'leftChat') {
        this.message = 'You\'re left the chat'
      }

      this.snackbar = !!this.message
    },
    methods: {
      ...mapMutations(['setUser']),
      submit() {
        if (this.$refs.form.validate()) {
          const user = {
            name: this.name,
            room: this.room
          }

          this.$socket.emit('userJoin', user, data => {
            if (typeof data === 'string') {
              console.error(data)
            } else {
              user.id = data.userId
              this.setUser(user)
              this.$router.push('/chat')
            }
          })
        }
      }
    },
  }
</script>

<!--<style lang="scss">-->
<!--  .v-content__wrap {-->
<!--    display: flex;-->
<!--    flex-direction: column;-->
<!--    justify-content: center;-->
<!--    align-items: center;-->
<!--  }-->
<!--</style>-->
