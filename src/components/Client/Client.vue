<template>
    <div>
      <h1>Liste des Clients</h1>
      <div>
        <button @click="goToCreateClient">Ajouter un Client</button>
      </div>
      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>Nom</th>
            <th>Email</th>
            <th>Téléphone</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="client in clients" :key="client.id">
            <td>{{ client.id }}</td>
            <td>{{ client.nom }}</td>
            <td>{{ client.email }}</td>
            <td>{{ client.telephone }}</td>
            <td>
              <button @click="goToEditClient(client.id)">Modifier</button>
              <button @click="deleteClient(client.id)">Supprimer</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script>
  import ClientService from '@/services/ClientService';
  
  export default {
    name: 'Client',
    data() {
      return {
        clients: []
      };
    },
    created() {
      this.fetchClients();
    },
    methods: {
      async fetchClients() {
        try {
          const response = await ClientService.getClients();
          this.clients = response.data;
        } catch (error) {
          console.error('Erreur lors de la récupération des clients:', error);
        }
      },
      goToCreateClient() {
        console.log('Navigating to CreateClient');
        this.$router.push({ name: 'CreateClient' });  // Naviguer vers la page de création
      },
      goToEditClient(clientId) {
        this.$router.push({ name: 'EditClient', params: { id: clientId } }); // Naviguer vers la page de modification
      },
      async deleteClient(clientId) {
        try {
          await ClientService.deleteClient(clientId);
          this.fetchClients(); 
        } catch (error) {
          console.error('Erreur lors de la suppression du client:', error);
        }
      }
    }
  };
  </script>
  
  <style scoped>
  table {
    width: 100%;
    border-collapse: collapse;
  }
  th, td {
    padding: 8px;
    text-align: left;
  }
  th {
    background-color: #f4f4f4;
  }
  button {
    margin-right: 10px;
  }
  </style>  