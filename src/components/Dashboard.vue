<template>
    <div class="dashboard">
      <h1 class="dashboard-title">Tableau de bord des statistiques</h1>
  
      <div class="stats-container">
        <!-- Affichage des statistiques sous forme de texte -->
        <div class="stat-box">
          <h3>Total Réclamations</h3>
          <p class="stat-value">{{ totalReclamations }}</p>
        </div>
  
        <div class="stat-box" v-if="customerSatisfactionRate !== null">
          <h3>Taux de Satisfaction Client</h3>
          <p class="stat-value">{{ customerSatisfactionRate }} %</p>
        </div>
  
        <div class="stat-box" v-if="totalFacturesHorsGarantie !== null">
          <h3>Factures Hors Garantie</h3>
          <p class="stat-value">{{ totalFacturesHorsGarantie }} €</p>
        </div>
      </div>
  
      <!-- Section Réclamations par statut -->
      <div class="charts-container" v-if="reclamationPercentageByStatus">
        <h3 >Réclamations par Statut</h3>
        <div class="chart-list">
          <div
            v-for="(percentage, status) in reclamationPercentageByStatus"
            :key="status"
            class="chart-item"
          >
            <span>{{ status }} :</span>
            <div class="progress-bar">
              <div
                class="progress-fill"
                :style="{ width: percentage + '%' }"
              ></div>
            </div>
            <span>{{ percentage }}%</span>
          </div>
        </div>
      </div>
  
      <!-- Section Articles les plus réclamés -->
      <div class="list-container" v-if="mostReclaimedArticles.length > 0">
        <h3>List des Réclamations</h3>
        <ul class="article-list">
          <li v-for="article in mostReclaimedArticles" :key="article" class="article-item">
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
        totalReclamations: '',
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
      // Simulation de récupération des données
      const reclamationsResponse = await StatisticsService.getTotalReclamations();
      this.totalReclamations = reclamationsResponse.data.totalReclamations; // Extraire la valeur

      const satisfactionResponse = await StatisticsService.getCustomerSatisfactionRate();
      this.customerSatisfactionRate = satisfactionResponse.data.satisfactionRate; // Extraire la valeur

      const facturesResponse = await StatisticsService.getTotalFacturesHorsGarantie();
      this.totalFacturesHorsGarantie = facturesResponse.data.totalFacturesHorsGarantie; // Extraire la valeur

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
  
  
  <style scoped>
  /* Global container styling */
  .dashboard {
    padding: 20px;
    background-color: white;
  }
  
  /* Dashboard title styling */
  .dashboard-title {
    font-size: 2rem;
    color: #333;
    margin-bottom: 30px;
    text-align: center;
  }
  
  /* Stats container for the cards */
  .stats-container {
    margin-top: 90px;
    display: flex;
    gap: 30px;
    margin-bottom: 30px;
    justify-content: space-between;
  }
  
  .stat-box {
    background-color: #fff;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
    width: 30%;
    text-align: center;
    transition: all 0.3s ease;
    border: 1px solid black;

  }
  
  .stat-box:hover {
    box-shadow: 0 6px 15px rgba(0, 0, 0, 0.15);
    transform: translateY(-5px);
  }
  
  .stat-box h3 {
    color: #4e5b6e;
    font-size:1.3rem;
    margin-bottom: 10px;
  }
  
  .stat-value {
    font-size: 1.5rem;
    font-weight: bold;
    color: #1ea809;
  }
  
  /* Chart container */
  .charts-container {
    margin-top: 40px;
  }
  .charts-container h3 {
    font-size: 1.4rem;
  }
  .chart-list {
    display: flex;
    flex-direction: column;
    gap: 15px;
  }

  .chart-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 1rem;
  }
  
  .progress-bar {
    width: 60%;
    height: 10px;
    background-color: #e0e0e0;
    border-radius: 5px;
    margin: 0 10px;
  }
  
  .progress-fill {
    height: 100%;
    background-color: #61a0af;
    border-radius: 5px;
  }
  
  .chart-item span {
    font-weight: 600;
    color: #333;
  }
  
  /* List container for most reclaimed articles */
  .list-container {
    margin-top: 40px;
 } 
 /* List container for most reclaimed articles */
 .list-container h3 {
    font-size: 1.3rem;
 } 
  
  .article-list {
    font-size: 1rem;
    list-style-type: none;
    padding: 0;
    margin: 0;
  }
  
  .article-item {
    background-color: #fff;
    padding: 15px;
    margin-bottom: 10px;
    border-radius: 8px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
    transition: all 0.3s ease;
    border: 1px solid black;

  }
  
  .article-item:hover {
    transform: translateY(-5px);
    box-shadow: 0 6px 15px rgba(0, 0, 0, 0.15);
    background-color: #f9f9f9;
  }
  
  .article-item:last-child {
    margin-bottom: 0;
  }
  </style>
  