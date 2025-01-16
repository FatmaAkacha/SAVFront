<template>
    <div>
      <h1>Liste des Pièces</h1>
      <button @click="goToCreatePiece">Ajouter une Pièce</button>
      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>Nom</th>
            <th>Prix</th>
            <th>Article</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="piece in pieces" :key="piece.id">
            <td>{{ piece.id }}</td>
            <td>{{ piece.nom }}</td>
            <td>{{ piece.prix }}</td>
            <td>{{ piece.articleId}}</td> 
            <td>
              <button @click="goToEditPiece(piece.id)">Modifier</button>
              <button @click="deletePiece(piece.id)">Supprimer</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
</template>

<script>
import PieceService from '@/services/PieceService';

export default {
  name: 'PieceList',
  data() {
    return {
      pieces: [],
    };
  },
  created() {
    this.fetchPieces();
  },
  methods: {
    async fetchPieces() {
      try {
        const response = await PieceService.getPieces();
        this.pieces = response.data;
      } catch (error) {
        console.error('Erreur lors de la récupération des pièces:', error);
      }
    },
    goToCreatePiece() {
      this.$router.push({ name: 'CreatePiece' });
    },
    goToEditPiece(pieceId) {
      this.$router.push({ name: 'EditPiece', params: { id: pieceId } });
    },
    async deletePiece(pieceId) {
      try {
        await PieceService.deletePiece(pieceId);
        this.fetchPieces(); // Rafraîchir la liste après suppression
      } catch (error) {
        console.error('Erreur lors de la suppression de la pièce:', error);
      }
    }
  }
};
</script>

<style scoped>
table {
  width: 100%;
  border-collapse: collapse;
}
th, td {
  padding: 8px;
  text-align: left;
}
th {
  background-color: #f4f4f4;
}
button {
  margin-right: 10px;
}
</style>
