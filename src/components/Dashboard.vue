<template>
    <div class="dashboard">
      <h1>Tableau de bord des statistiques</h1>
  
      <div class="stats-container">
        <div class="stat-box" v-if="totalReclamations !== null">
          <h3>Total Réclamations</h3>
          <p>{{ totalReclamations }}</p>
        </div>
  
        <div class="stat-box" v-if="customerSatisfactionRate !== null">
          <h3>Taux de Satisfaction Client</h3>
          <p>{{ customerSatisfactionRate }}%</p>
        </div>
  
        <div class="stat-box" v-if="totalFacturesHorsGarantie !== null">
          <h3>Factures Hors Garantie</h3>
          <p>{{ totalFacturesHorsGarantie }} €</p>
        </div>
      </div>
  
      <div class="charts-container" v-if="reclamationPercentageByStatus">
        <h3>Réclamations par Statut</h3>
        <ul>
          <li v-for="(percentage, status) in reclamationPercentageByStatus" :key="status">
            {{ status }} : {{ percentage }}%
          </li>
        </ul>
      </div>
  
      <div class="list-container" v-if="mostReclaimedArticles.length > 0">
        <h3>Articles les plus réclamés</h3>
        <ul>
          <li v-for="article in mostReclaimedArticles" :key="article">
            {{ article }}
          </li>
        </ul>
      </div>
    </div>
  </template>
  
  <script>
  import StatisticsService from "@/services/StatisticsService";
  
  export default {
    name: "DashboardAdmin",
    data() {
      return {
        totalReclamations: null,
        customerSatisfactionRate: null,
        totalFacturesHorsGarantie: null,
        reclamationPercentageByStatus: null,
        mostReclaimedArticles: [],
      };
    },
    created() {
      this.fetchStatistics();
    },
    methods: {
      async fetchStatistics() {
        try {
          const reclamationsResponse = await StatisticsService.getTotalReclamations();
          this.totalReclamations = reclamationsResponse.data;
  
          const satisfactionResponse = await StatisticsService.getCustomerSatisfactionRate();
          this.customerSatisfactionRate = satisfactionResponse.data;
  
          const facturesResponse = await StatisticsService.getTotalFacturesHorsGarantie();
          this.totalFacturesHorsGarantie = facturesResponse.data;
  
          const statusResponse = await StatisticsService.getReclamationPercentageByStatus();
          this.reclamationPercentageByStatus = statusResponse.data;
  
          const articlesResponse = await StatisticsService.getMostReclaimedArticles();
          this.mostReclaimedArticles = articlesResponse.data;
        } catch (error) {
          console.error("Erreur lors de la récupération des statistiques :", error);
        }
      },
    },
  };
  </script>
  
  <style>
  .dashboard {
    padding: 20px;
  }
  .stats-container {
    display: flex;
    gap: 20px;
    margin-bottom: 20px;
  }
  .stat-box {
    background: #f4f4f4;
    padding: 10px;
    border-radius: 5px;
    text-align: center;
  }
  .charts-container, .list-container {
    margin-top: 20px;
  }
  </style>  