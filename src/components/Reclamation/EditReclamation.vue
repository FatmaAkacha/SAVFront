<template>
    <div>
      <h1>Modifier une Réclamation</h1>
      <form @submit.prevent="updateReclamation">
        <div>
          <label for="description">Description :</label>
          <textarea id="description" v-model="reclamation.description" required></textarea>
        </div>
        <div>
          <label for="date">Date :</label>
          <input type="date" id="date" v-model="reclamation.dateReclamation" required />
        </div>
        <div>
          <label for="statut">Statut :</label>
          <select id="statut" v-model="reclamation.statut" required>
            <option value="En cours">En cours</option>
            <option value="Terminée">Terminée</option>
          </select>
        </div>
        <div>
          <label for="clientId">Client (ID) :</label>
          <input type="number" id="clientId" v-model="reclamation.clientId" required />
        </div>
        <button type="submit">Modifier</button>
      </form>
    </div>
  </template>
  
  <script>
  import ReclamationService from '@/services/ReclamationService';
  
  export default {
    name: 'EditReclamation',
    data() {
      return {
        reclamation: {
          description: '',
          dateReclamation: '',
          statut: 'En cours',
          clientId: null,
        },
      };
    },
    async created() {
      const { id } = this.$route.params;
      try {
        const response = await ReclamationService.getReclamationById(id);
        this.reclamation = response.data;
      } catch (error) {
        console.error('Erreur lors de la récupération:', error);
      }
    },
    methods: {
      async updateReclamation() {
        try {
          await ReclamationService.updateReclamation(this.reclamation.id, this.reclamation);
          this.$router.push({ name: 'ReclamationList' });
        } catch (error) {
          console.error('Erreur lors de la modification:', error);
        }
      },
    },
  };
  </script>
  