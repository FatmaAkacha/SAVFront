<template>
    <div>
      <h1>Ajouter une Pièce</h1>
      <form @submit.prevent="createPiece">
        <div>
          <label for="nom">Nom:</label>
          <input type="text" id="nom" v-model="piece.nom" required />
        </div>
        <div>
          <label for="prix">Prix:</label>
          <input type="number" id="prix" v-model="piece.prix" required />
        </div>
        <div>
          <label for="article">Article:</label>
          <select v-model="piece.articleId" required>
            <option v-for="article in articles" :key="article.id" :value="article.id">
              {{ article.nom }}
            </option>
          </select>
        </div>
        <div>
          <button type="submit">Ajouter</button>
        </div>
      </form>
    </div>
</template>

<script>
import PieceService from '@/services/PieceService';
import ArticleService from '@/services/ArticleService';

export default {
  name: 'CreatePiece',
  data() {
    return {
      piece: {
        nom: '',
        prix: 0,
        articleId: null,
      },
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
    async createPiece() {
      try {
        await PieceService.createPiece(this.piece);
        this.$router.push({ name: 'Piece' }); // Retourner à la liste des pièces après la création
      } catch (error) {
        console.error('Erreur lors de la création de la pièce:', error);
      }
    }
  }
};
</script>

<style scoped>
form {
  width: 300px;
  margin: 0 auto;
}
div {
  margin-bottom: 15px;
}
</style>