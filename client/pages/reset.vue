<template>
  <v-flex xs12 sm8 md4>
    <v-card class="elevation-12">
      <v-toolbar dark color="primary">
        <v-toolbar-title>Reset Password Form</v-toolbar-title>
        <v-spacer></v-spacer>
      </v-toolbar>

      <v-card-text>
        <v-form>
          <v-text-field v-model="password" prepend-icon="lock" name="password" label="Password" id="password" type="password"></v-text-field>
        </v-form>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn @click="onSubmit(password)" color="primary">Reset Password</v-btn>
      </v-card-actions>
    </v-card>

    <v-dialog v-model="registerComplete" persistent max-width="290">
      <v-card>
        <v-card-title class="headline">Change Password Complete</v-card-title>
        <v-card-text>Please login with your new password.</v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="green darken-1" flat @click="dismissModal">Dismiss</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-flex>

</template>

<script>
import { browserClient } from '@/store/feathers-client'

export default {
  layout: 'auth',
  async asyncData ({ req }) {
    return {token: req.query.token}
  },
  data () {
    return {
      password: undefined,
      registerComplete: false,
    }
  },
  methods: {
    dismissError () {
      this.error = undefined
      this.clearAuthenticateError()
    },
    async onSubmit (password) {
      try {
        browserClient.service('authManagement').create({
          action: 'resetPwdLong',
          value: {
            token: this.token,
            password: password,
          }
        })
        this.registerComplete = true
      } catch(e) {
        console.log(e)
      }
    },
    dismissModal() {
      this.$router.replace({ path: '/' })
    }
  }
}
</script>