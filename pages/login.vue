<template>
  <div>
    <h1>Login</h1>
    <form @submit.prevent="login">
      <input type="email" v-model="email" placeholder="Email" />
      <input type="password" v-model="password" placeholder="Password" />
      <button type="submit">Login</button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      email: '',
      password: '',
    };
  },
  methods: {
    async login() {
      try {
        await this.$axios.post('/login', {
          email: this.email,
          password: this.password,
        });
        this.$router.push('/payments');
      } catch (error) {
        console.error('Login failed', error);
      }
    },
  },
};
</script>
