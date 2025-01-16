<template>
    <div>
      <h1>Liste des Réclamations</h1>
      <button @click="goToCreateReclamation">Ajouter une Réclamation</button>
      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>Description</th>
            <th>Date</th>
            <th>Statut</th>
            <th>Client</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="reclamation in reclamations" :key="reclamation.id">
            <td>{{ reclamation.id }}</td>
            <td>{{ reclamation.description }}</td>
            <td>{{ reclamation.dateReclamation }}</td>
            <td>{{ reclamation.statut }}</td>
            <td>{{ reclamation.clientId || 'N/A' }}</td>
            <td>
              <button @click="goToEditReclamation(reclamation.id)">Modifier</button>
              <button @click="deleteReclamation(reclamation.id)">Supprimer</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script>
  import ReclamationService from '@/services/ReclamationService';
  
  export default {
    name: 'ReclamationList',
    data() {
      return {
        reclamations: [],
      };
    },
    async created() {
      this.fetchReclamations();
    },
    methods: {
      async fetchReclamations() {
        try {
          const response = await ReclamationService.getReclamations();
          this.reclamations = response.data;
        } catch (error) {
          console.error('Erreur lors de la récupération des réclamations:', error);
        }
      },
      goToCreateReclamation() {
        this.$router.push({ name: 'CreateReclamation' });
      },
      goToEditReclamation(id) {
        this.$router.push({ name: 'EditReclamation', params: { id } });
      },
      async deleteReclamation(id) {
        try {
          await ReclamationService.deleteReclamation(id);
          this.fetchReclamations();
        } catch (error) {
          console.error('Erreur lors de la suppression:', error);
        }
      },
    },
  };
  </script>
  