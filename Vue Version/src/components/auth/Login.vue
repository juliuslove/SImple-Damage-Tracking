<template>
  <v-container class="view-admin" text-xs-center fill-height>
    <v-layout row wrap align-center justify-center>
      <v-flex xs12 sm8 md6 xl4>
        <v-card>
          <v-card-text>
            <v-form ref="form" @submit.prevent="login" >
              <v-text-field
                v-model="email"
                name="email"
                label="E-mail"
                :rules="[rules.required]"
                required
              ></v-text-field>
              <v-text-field
                v-model="password"
                name="password"
                label="Password"
                :rules="[rules.required]"
                type="password"
                required
              ></v-text-field>
              <v-flex xs12>
                <v-alert type="error" dismissible v-model="alert" >
                {{ feedback }}
              </v-alert>
              </v-flex>
              <v-btn color="primary"
               type="submit"
                >
                Login
              </v-btn>
            </v-form >
          </v-card-text>
        </v-card>
      </v-flex>
    </v-layout>
  </v-container>

</template>

<script>
import firebase from 'firebase/app'
import 'firebase/auth'

export default {
  name: 'Login',
  data() {
    return {
      show3: false,
      email: null,
      password: null,
      feedback: 'test',
      alert: false,
      rules: {
        required: value => !!value || 'Required.'
      }
    }
  },
  methods: {
    login() {
      // Attempts to log in using firebase auth
      if (this.email && this.password) {
        this.feedback = null
        firebase
          .auth()
          .signInWithEmailAndPassword(this.email, this.password)
          .then(cred => {
            this.$router.push({ name: 'Index' })
          })
          .catch(err => {
            console.log(err)
            this.feedback = err.message
            this.alert = true
          })
      } else {
        this.feedback = 'Please fill in both fields'
      }
    }
  }
}
</script>

<style>
</style>
