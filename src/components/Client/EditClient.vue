<template>
    <div>
      <h1>Modifier un Client</h1>
      <form @submit.prevent="updateClient">
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
          <button type="submit">Modifier</button>
        </div>
      </form>
    </div>
  </template>
  
  <script>
  import ClientService from '@/services/ClientService';
  
  export default {
    name: 'EditClient',
    data() {
      return {
        client: {
          id: null,
          nom: '',
          email: '',
          telephone: ''
        }
      };
    },
    created() {
      this.fetchClientData();
    },
    methods: {
      async fetchClientData() {
        const clientId = this.$route.params.id; // Récupérer l'id depuis les paramètres de la route
        try {
          const response = await ClientService.getClientById(clientId);
          this.client = response.data; // Remplir les champs avec les données du client
        } catch (error) {
          console.error('Erreur lors de la récupération du client:', error);
        }
      },
      async updateClient() {
        try {
          await ClientService.updateClient(this.client.id, this.client);
          this.$router.push({ name: 'Client' }); // Retourner à la liste des clients après modification
        } catch (error) {
          console.error('Erreur lors de la mise à jour du client:', error);
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