<template>
    <div>
      <h1>Ajouter un Client</h1>
      <form @submit.prevent="createClient">
        <div>
          <label for="nom">Nom:</label>
          <input type="text" id="nom" v-model="client.nom" required />
        </div>
        <div>
          <label for="email">Email:</label>
          <input type="email" id="email" v-model="client.email" required />
        </div>
        <div>
          <label for="telephone">Téléphone:</label>
          <input type="tel" id="telephone" v-model="client.telephone" required />
        </div>
        <div>
          <button type="submit">Ajouter</button>
        </div>
      </form>
    </div>
  </template>
  
  <script>
  import ClientService from '@/services/ClientService';
  import { useRouter } from 'vue-router';
  
  export default {
    name: 'CreateClient',
    data() {
      return {
        client: {
          nom: '',
          email: '',
          telephone: ''
        }
      };
    },
    methods: {
  async createClient() {
    try {
      // Vous vous êtes trompés de méthode, cela doit être createClients au lieu de createClient
      await ClientService.createClients(this.client);
      this.$router.push({ name: 'Client' }); // Retourner à la liste des clients
    } catch (error) {
      console.error('Erreur lors de la création du client:', error);
    }
  }
}
  };
  </script>
  
  <style scoped>
  form {
    width: 300px;
    margin: 0 auto;
  }
  div {
    margin-bottom: 15px;
  }
  </style>  