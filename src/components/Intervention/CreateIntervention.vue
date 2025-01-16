<template>
    <div>
      <h1>{{ isEditMode ? 'Modifier' : 'Ajouter' }} une Intervention</h1>
      <form @submit.prevent="saveIntervention">
        <div>
          <label for="date">Date de l'intervention :</label>
          <input
            type="date"
            id="date"
            v-model="intervention.dateIntervention"
            required
          />
        </div>
        <div>
          <label for="sousGarantie">Sous Garantie :</label>
          <input
            type="checkbox"
            id="sousGarantie"
            v-model="intervention.sousGarantie"
          />
        </div>
        <div>
          <label for="montantFacture">Montant Facture (€) :</label>
          <input
            type="number"
            id="montantFacture"
            v-model="intervention.montantFacture"
            placeholder="Ex : 150.00"
            step="0.01"
            min="0"
          />
        </div>
        <div>
          <label for="reclamationId">Réclamation associée (ID) :</label>
          <input
            type="number"
            id="reclamationId"
            v-model="intervention.reclamationId"
            placeholder="Ex : 1"
            required
          />
        </div>
        <div>
          <label for="technicienId">Technicien affecté (ID) :</label>
          <input
            type="number"
            id="technicienId"
            v-model="intervention.technicienId"
            placeholder="Ex : 2"
            required
          />
        </div>
        <div>
          <button type="submit">{{ isEditMode ? 'Modifier' : 'Ajouter' }}</button>
          <button type="button" @click="cancel">Annuler</button>
        </div>
      </form>
    </div>
  </template>
  
  <script>
  import InterventionService from '@/services/InterventionService';
  
  export default {
    name: 'CreateIntervention',
    data() {
      return {
        intervention: {
          dateIntervention: '',
          sousGarantie: false,
          montantFacture: 0.0,
          reclamationId: null,
          technicienId: null,
        },
        isEditMode: false,
      };
    },
    async created() {
      const interventionId = this.$route.params.id;
      if (interventionId) {
        this.isEditMode = true;
        const response = await InterventionService.getInterventionById(interventionId);
        this.intervention = response.data;
      }
    },
    methods: {
      async saveIntervention() {
        try {
          if (this.isEditMode) {
            await InterventionService.updateIntervention(this.intervention.id, this.intervention);
          } else {
            await InterventionService.createIntervention(this.intervention);
          }
          this.$router.push({ name: 'InterventionList' });
        } catch (error) {
          console.error('Erreur lors de l\'enregistrement de l\'intervention:', error);
        }
      },
      cancel() {
        this.$router.push({ name: 'InterventionList' });
      },
    },
  };
  </script>
  
  <style scoped>
  /* Ajouter vos styles ici */
  </style>  