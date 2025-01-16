<template>
    <div>
      <h1>{{ isEditMode ? 'Modifier' : 'Ajouter' }} un Article</h1>
      <form @submit.prevent="saveArticle">
        <div>
          <label for="nom">Nom:</label>
          <input type="text" id="nom" v-model="article.nom" required />
        </div>
        <div>
          <label for="description">Description:</label>
          <textarea id="description" v-model="article.description" required></textarea>
        </div>
        <div>
          <label for="prix">Prix:</label>
          <input type="number" id="prix" v-model="article.prix" required />
        </div>
        <div>
          <label for="sousGarantie">Sous Garantie:</label>
          <input type="checkbox" id="sousGarantie" v-model="article.sousGarantie" />
        </div>
        <div>
          <button type="submit">{{ isEditMode ? 'Modifier' : 'Ajouter' }}</button>
        </div>
      </form>
    </div>
  </template>
  
  <script>
  import ArticleService from '@/services/ArticleService';
  
  export default {
    name: 'CreateArticle',
    data() {
      return {
        article: {
          nom: '',
          description: '',
          prix: 0,
          sousGarantie: false,
        },
        isEditMode: false,
      };
    },
    async created() {
      const articleId = this.$route.params.id;
      if (articleId) {
        this.isEditMode = true;
        const response = await ArticleService.getArticleById(articleId);
        this.article = response.data;
      }
    },
    methods: {
      async saveArticle() {
        try {
          if (this.isEditMode) {
            await ArticleService.updateArticle(this.article.id, this.article);
          } else {
            await ArticleService.createArticle(this.article);
          }
          this.$router.push({ name: 'ArticleList' });
        } catch (error) {
          console.error('Erreur lors de l\'enregistrement de l\'article:', error);
        }
      },
    },
  };
  </script>
  
  <style scoped>
  /* Styles similaires à ceux de CreateClient */
  </style>
  