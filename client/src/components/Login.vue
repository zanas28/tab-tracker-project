<template>
  <v-layout>
    <v-flex xs6 offset-xs3>
      <panel title="Login">
          <v-text-field
            name="email"
            label="Email"
            v-model="email"
          ></v-text-field>
          <v-text-field
            name="password"
            type="password"
            label="Password"
            v-model="password"
          ></v-text-field>
          <div class="error" v-html="error"></div>
          <v-btn
            dark
            class="cyan"
            @click='login'
          >
            login
          </v-btn>
        </panel>
    </v-flex>
  </v-layout>
</template>

<script>
import AuthenticationService from '@/services/AuthenticationService';
import Panel from '@/components/Panel';
export default {
  data () {
    return {
      email: '',
      password: '',
      error: null
    };
  },
  methods: {
    async login () {
      try {
        const response = await AuthenticationService.login({
          email: this.email,
          password: this.password
        });
        this.$store.dispatch('setToken', response.data.token);
        this.$store.dispatch('setUser', response.data.user);
      } catch (error) {
        this.error = error.response.data.error;
      }
    }
  },
  components: {
    Panel
  }
};
</script>

<style scoped>
.error {
  color: red;
}
</style>
