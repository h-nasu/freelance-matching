<template>
  <v-flex xs12 sm8 md4>
    <v-card class="elevation-12">
      <v-toolbar dark color="primary">
        <v-toolbar-title>Login form</v-toolbar-title>
        <v-spacer></v-spacer>
      </v-toolbar>

      <v-card-text>
        <div class="error" v-if="error">
          {{error.message}}
          <a class="close" href="javascript://" @click.prevent="dismissError">dismiss</a>
        </div>
        <v-form>
          <v-text-field v-model="email" prepend-icon="person" name="login" label="Login" type="text"></v-text-field>
          <v-text-field v-model="password" prepend-icon="lock" name="password" label="Password" id="password" type="password"></v-text-field>
        </v-form>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <n-link to="forgetPassword" class="mr-1">Forget Password</n-link>
        <v-btn to="/register" color="secondary">Sign Up</v-btn>
        <v-btn @click="onSubmit(email, password)" color="primary">Login</v-btn>
      </v-card-actions>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn @click="fbLogin" color="info">Login with Facebook</v-btn>
      </v-card-actions>
    </v-card>
  </v-flex>
</template>

<script>
import AuthMixin from "@/mixins/AuthMixin";

export default {
  layout: 'auth',
  mixins: [AuthMixin],
  data () {
    return {
      email: undefined,
      password: undefined,
      error: undefined
    }
  },
  methods: {
    dismissError () {
      this.error = undefined
      this.clearAuthenticateError()
    },
    onSubmit (email, password) {
      this.authenticate({strategy: 'local', email, password})
        .then(() => {
          this.$router.replace({ path: '/mypage' })
        })
        // Just use the returned error instead of mapping it from the store.
        .catch(error => {
          // Convert the error to a plain object and add a message.
          let type = error.className
          error = Object.assign({}, error)
          error.message = (type === 'not-authenticated')
            ? 'Incorrect email or password.'
            : 'An error prevented login.'
          this.error = error
        })
    },
    fbLogin () {
      window.location = "/auth/facebook"
    },
  }
}
</script>