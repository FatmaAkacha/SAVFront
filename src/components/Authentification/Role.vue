<template>
    <div>
      <h2>Add Role</h2>
      <form @submit.prevent="addRole">
        <input v-model="userId" type="text" placeholder="User ID" required />
        <input v-model="role" type="text" placeholder="Role" required />
        <button type="submit">Add Role</button>
      </form>
    </div>
  </template>
  
  <script>
  import AuthService from '@/services/AuthService';
  
  export default {
    name:'role',
    data() {
      return {
        userId: '',
        role: '',
      };
    },
    methods: {
      async addRole() {
        const roleData = {
          userId: this.userId,
          role: this.role,
        };
        try {
          const response = await AuthService.addRole(roleData);
          console.log('Role added:', response.data);
        } catch (error) {
          console.error('Failed to add role:', error.response.data);
        }
      },
    },
  };
  </script>
  