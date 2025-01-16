<template>
    <div>
      <h1>Liste des Articles</h1>
      <div>
        <button @click="goToCreateArticle">Ajouter un Article</button>
      </div>
      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>Nom</th>
            <th>Description</th>
            <th>Prix</th>
            <th>Sous Garantie</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="article in articles" :key="article.id">
            <td>{{ article.id }}</td>
            <td>{{ article.nom }}</td>
            <td>{{ article.description }}</td>
            <td>{{ article.prix }}</td>
            <td>{{ article.sousGarantie ? 'Oui' : 'Non' }}</td>
            <td>
              <button @click="goToEditArticle(article.id)">Modifier</button>
              <button @click="deleteArticle(article.id)">Supprimer</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script>
  import ArticleService from '@/services/ArticleService';
  
  export default {
    name: 'ArticleList',
    data() {
      return {
        articles: [],
      };
    },
    created() {
      this.fetchArticles();
    },
    methods: {
      async fetchArticles() {
        try {
          const response = await ArticleService.getArticles();
          this.articles = response.data;
        } catch (error) {
          console.error('Erreur lors de la récupération des articles:', error);
        }
      },
      goToCreateArticle() {
        this.$router.push({ name: 'CreateArticle' });
      },
      goToEditArticle(articleId) {
        this.$router.push({ name: 'EditArticle', params: { id: articleId } });
      },
      async deleteArticle(articleId) {
        try {
          await ArticleService.deleteArticle(articleId);
          this.fetchArticles();
        } catch (error) {
          console.error('Erreur lors de la suppression de l\'article:', error);
        }
      },
    },
  };
  </script>
  
  <style scoped>
  /* Styles similaires à ceux de Client */
  </style>  