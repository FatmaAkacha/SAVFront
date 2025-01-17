<template>
    <nav class="navbar">
      <ul class="navbar-list">
        <li class="navbar-item" v-for="parent in navbarStructure" :key="parent.name">
          <router-link
  v-if="parent.path"
  class="navbar-link"
  :to="parent.path"
>{{ parent.label }}</router-link>
          <ul v-if="parent.children && parent.children.length" class="dropdown">
            <li class="dropdown-item" v-for="child in parent.children" :key="child.name">
              <router-link
  v-if="child.path"
  class="dropdown-link"
  :to="child.path"
>{{ child.label }}</router-link>
            </li>
          </ul>
        </li>
      </ul>
    </nav>
  </template>
  
  <script>
  export default {
    name: "Navbar",
    data() {
      return {
        navbarStructure: [
        {
            label: "Dashboard",
            path: "/dashboard",
            name: "Dashboard",
            children: [],
          },
          {
            label: "Clients",
            path: "/clients",
            name: "Client",
            children: [
            { label: "Liste des clients", path: "/clients", name: "Client" },
              { label: "Créer un client", path: "/create-client", name: "CreateClient" },
            ],
          },
          {
          label: "Techniciens", 
          path: "/techniciens", 
          name: "Technicien",
          children: [
            { label: "Liste des techniciens", path: "/techniciens", name: "TechnicienList" },
            { label: "Créer un technicien", path: "/technicien/create", name: "CreateTechnicien" },
          ],
        },

          {
            label: "Articles",
            path: "/articles",
            name: "ArticleList" ,
            children: [
            { label: "Lists des articles", path: "/articles", name: "ArticleList" },
              { label: "Créer un article", path: "/create-article", name: "CreateArticle" },
            ],
          },
          {
            label: "Interventions",
            path: "/interventions",
            name: "InterventionList",
            children: [
            { label: "Liste des interventions", path: "/interventions", name: "InterventionList" },
              { label: "Créer une intervention", path: "/create-intervention", name: "CreateIntervention" },
            ],
          },
          {
            label: "Réclamations",
            path: "/reclamations",
            name: "ReclamationList",
            children: [
            { label: "Liste des réclamations", path: "/reclamations", name: "ReclamationList" },
              { label: "Créer une réclamation", path: "/create-reclamation", name: "CreateReclamation" },
            ],
          },
          {
            label: "Pièces",
            path: "/pieces",
            name: "Piece",
            children: [
            { label: "Liste des pièces", path: "/pieces", name: "Piece" },
              { label: "Créer une pièce", path: "/create-piece", name: "CreatePiece" },
            ],
          },
          {
            label: "Authentification",
            path: "/login", 
            name: "Login",
            children: [
            { label: "Ajouter un rôle", path: "/add-role", name: "Role" },
            { label: "Se déconnecter", path: "/login", name: "Login" },
            ],
          },
        ],
      };
    },
  };
  </script>
  
  <style scoped>
  /* Navbar Container */
  .navbar {
    background-color: #2c3e50;
    color: white;
    padding: 10px 20px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  }
  
  /* Navbar List */
  .navbar-list {
    display: flex;
    list-style: none;
    margin: 0;
    padding: 0;
    justify-content: space-around;
    align-items: center;
  }
  
  /* Navbar Item */
  .navbar-item {
    position: relative;
  }
  
  /* Navbar Link */
  .navbar-link {
    text-decoration: none;
    color: white;
    font-size: 1.1rem;
    font-weight: 500;
    padding: 8px 12px;
    transition: color 0.3s ease;
  }
  
  .navbar-link:hover {
    color: #1abc9c;
  }
  
  /* Dropdown Menu */
  .dropdown {
    position: absolute;
    top: 100%;
    left: 0;
    background-color: #34495e;
    list-style: none;
    margin: 0;
    padding: 0;
    display: none;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    border-radius: 4px;
  }
  
  .navbar-item:hover .dropdown {
    display: block;
  }
  
  /* Dropdown Item */
  .dropdown-item {
    margin: 0;
  }
  
  .dropdown-link {
    text-decoration: none;
    color: white;
    font-size: 0.9rem;
    padding: 8px 16px;
    display: block;
    transition: background-color 0.3s ease;
  }
  
  .dropdown-link:hover {
    background-color: #1abc9c;
  }
  </style>
  