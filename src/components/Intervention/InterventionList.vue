<template>
    <div>
      <h1>Liste des Interventions</h1>
      <div>
        <button @click="goToCreateIntervention">Ajouter une Intervention</button>
      </div>
      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>Date</th>
            <th>Sous Garantie</th>
            <th>Montant Facture</th>
            <th>Technicien</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="intervention in interventions" :key="intervention.id">
            <td>{{ intervention.id }}</td>
            <td>{{ new Date(intervention.dateIntervention).toLocaleDateString() }}</td>
            <td>{{ intervention.sousGarantie ? 'Oui' : 'Non' }}</td>
            <td>{{ intervention.montantFacture }}</td>
            <td>{{ intervention.technicienId || 'Non assigné' }}</td>
            <td>
              <button @click="goToEditIntervention(intervention.id)">Modifier</button>
              <button @click="deleteIntervention(intervention.id)">Supprimer</button>
              <button @click="calculateInvoice(intervention.id)">Calculer Facture</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script>
  import InterventionService from '@/services/InterventionService';
  
  export default {
    name: 'InterventionList',
    data() {
      return {
        interventions: [],
      };
    },
    created() {
      this.fetchInterventions();
    },
    methods: {
      async fetchInterventions() {
        try {
          const response = await InterventionService.getInterventions();
          this.interventions = response.data;
        } catch (error) {
          console.error('Erreur lors de la récupération des interventions:', error);
        }
      },
      goToCreateIntervention() {
        this.$router.push({ name: 'CreateIntervention' });
      },
      goToEditIntervention(id) {
        this.$router.push({ name: 'EditIntervention', params: { id } });
      },
      async deleteIntervention(id) {
        try {
          await InterventionService.deleteIntervention(id);
          this.fetchInterventions();
        } catch (error) {
          console.error('Erreur lors de la suppression de l\'intervention:', error);
        }
      },
      async calculateInvoice(id) {
        try {
          const response = await InterventionService.calculateInvoice(id);
          alert(`Montant de la facture : ${response.data} €`);
          this.fetchInterventions();
        } catch (error) {
          console.error('Erreur lors du calcul de la facture:', error);
        }
      },
    },
  };
  </script>
  
  <style scoped>
  /* Ajouter vos styles ici */
  </style>  