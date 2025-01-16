<template>
    <div>
      <h2>Register</h2>
      <form @submit.prevent="registerUser">
        <input v-model="firstname" type="text" placeholder="FirstName" required />
        <input v-model="lastname" type="text" placeholder="LastName" required />
        <input v-model="username" type="text" placeholder="Username" required />
        <input v-model="email" type="email" placeholder="Email" required />
        <input v-model="password" type="password" placeholder="Password" required />
        <button type="submit">Register</button>
      </form>
    </div>
  </template>
  
  <script>
  import AuthService from '@/services/AuthService';
  
  export default {
    name:'registerUser',
    data() {
      return {
        firstname: '',
        lastname: '',
        email: '',
        username: '',
        password: '',
      };
    },
    methods: {
      async registerUser() {
        const user = {
            firstname: this.firstname,
            lastname: this.lastname,
          email: this.email,
          username: this.username,
          password: this.password,
        };
        try {
          const response = await AuthService.register(user);
          console.log('User registered:', response.data);
          this.$router.push({ name: 'Login' }); 
        } catch (error) {
          console.error('Registration failed:', error.response.data);
        }
      },
    },
  };
  </script>
  