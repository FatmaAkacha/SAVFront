<template>
    <div class="form-container">
      <h1>Ajouter un Rôle</h1>
      <form @submit.prevent="addRole">
        <div class="form-group">
          <label for="userId">ID Utilisateur:</label>
          <input type="text" id="userId" v-model="userId" required class="input-field"/>
        </div>
  
        <div class="form-group">
          <label for="role">Rôle:</label>
          <input type="text" id="role" v-model="role" required class="input-field"/>
        </div>
  
        <div class="form-group">
          <button type="submit" class="submit-button">Ajouter</button>
        </div>
      </form>
    </div>
  </template>
  
  <script>
  import AuthService from '@/services/AuthService';
  
  export default {
    name: 'Role',
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
          console.log('Rôle ajouté:', response.data);
          this.$router.push({ name: 'RoleList' }); // Vous pouvez rediriger vers la liste des rôles si nécessaire
        } catch (error) {
          console.error('Échec de l\'ajout du rôle:', error.response.data);
        }
      },
    },
  };
  </script>
  
  <style scoped>
  /* Global form styling */
  .form-container {
    max-width: 800px;
    margin: 30px auto;
    padding: 20px;
    background-color: #fff;
    border-radius: 8px;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
    margin-top: 100px;
  }
  
  h1 {
    text-align: center;
    color: #4f6d7a;
    font-size: 2.4rem;
    margin-bottom: 30px;
  }
  
  /* Input fields */
  .input-field {
    width: 100%;
    padding: 12px;
    margin-top: 8px;
    border: 1px solid #ccc;
    border-radius: 4px;
    font-size: 1rem;
    box-sizing: border-box;
    transition: all 0.3s ease;
  }
  
  .input-field:focus {
    border-color: #61a0af;
    outline: none;
    box-shadow: 0 0 5px rgba(97, 160, 175, 0.5);
  }
  
  /* Form group for aligning elements */
  .form-group {
    margin-bottom: 20px;
  }
  
  /* Submit Button */
  .submit-button {
    padding: 12px 24px;
    background-color: #1ea809;
    color: white;
    font-size: 1rem;
    border: none;
    border-radius: 30px;
    cursor: pointer;
    transition: all 0.3s ease;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  }
  
  .submit-button:hover {
    background-color: #17a512;
    transform: translateY(-3px);
  }
  
  .submit-button:active {
    transform: translateY(0);
  }
  
  /* Mobile responsiveness */
  @media (max-width: 768px) {
    .form-container {
      padding: 15px;
    }
  }
  </style>
  