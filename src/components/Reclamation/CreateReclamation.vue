<template>
  <div>
    <h1>Créer une Réclamation</h1>
    <form @submit.prevent="createReclamation">
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
      <button type="submit">Créer</button>
    </form>
  </div>
</template>

<script>
import ReclamationService from '@/services/ReclamationService';

export default {
  name: 'CreateReclamation',
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
  methods: {
    async createReclamation() {
      try {
        await ReclamationService.createReclamation(this.reclamation);
        this.$router.push({ name: 'ReclamationList' });
      } catch (error) {
        console.error('Erreur lors de la création:', error);
      }
    },
  },
};
</script>
