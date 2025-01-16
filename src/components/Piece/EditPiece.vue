<template>
    <div>
      <h1>Modifier une Pièce</h1>
      <form @submit.prevent="updatePiece">
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
          <button type="submit">Modifier</button>
        </div>
      </form>
    </div>
</template>

<script>
import PieceService from '@/services/PieceService';
import ArticleService from '@/services/ArticleService';

export default {
  name: 'EditPiece',
  data() {
    return {
      piece: {
        id: null,
        nom: '',
        prix: 0,
        articleId: null,
      },
      articles: [],
    };
  },
  created() {
    this.fetchPieceData();
    this.fetchArticles();
  },
  methods: {
    async fetchPieceData() {
      const pieceId = this.$route.params.id;
      try {
        const response = await PieceService.getPieceById(pieceId);
        this.piece = response.data;
      } catch (error) {
        console.error('Erreur lors de la récupération de la pièce:', error);
      }
    },
    async fetchArticles() {
      try {
        const response = await ArticleService.getArticles();
        this.articles = response.data;
      } catch (error) {
        console.error('Erreur lors de la récupération des articles:', error);
      }
    },
    async updatePiece() {
      try {
        await PieceService.updatePiece(this.piece.id, this.piece);
        this.$router.push({ name: 'Piece' });
      } catch (error) {
        console.error('Erreur lors de la mise à jour de la pièce:', error);
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
