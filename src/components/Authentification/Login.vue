<template>
    <div>
      <h2>Login</h2>
      <form @submit.prevent="loginUser">
        <input v-model="email" type="email" placeholder="Email" required />
        <input v-model="password" type="password" placeholder="Password" required />
        <button type="submit">Login</button>
      </form>
    </div>
  </template>
  
  <script>
  import AuthService from '@/services/AuthService';
  
  export default {
    name:'Login',
    data() {
      return {
        email: '',
        password: '',
      };
    },
    methods: {
      async loginUser() {
        const credentials = {
          email: this.email,
          password: this.password,
        };
        try {
          const response = await AuthService.getToken(credentials);
          localStorage.setItem('token', response.data.token); // Stocker le JWT
          console.log('Token received:', response.data);
          this.$router.push({ name: 'ArticleList' }); 
        } catch (error) {
          console.error('Login failed:', error.response.data);
        }
      },
    },
  };
  </script>
  