<!-- ArticleList.vue -->
<template>
    <div class="article-list">
      <h1>Liste des Articles</h1>
  
      <ul>
        <li v-for="article in articles" :key="article.id">
          <router-link :to="{ name: 'article-detail', params: { id: article.id } }">
            <div class="article-card">
              <img :src="article.image.url" alt="Article Image">
              <div class="article-info">
                <h2>{{ article.title }}</h2>
                <p>{{ article.body.substring(0, 100) }}...</p>
                <p>Catégorie: {{ article.category.name }}</p>
                <p>Date de Publication: {{ formatDate(article.published_at) }}</p>
              </div>
            </div>
          </router-link>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    name: 'ArticleList',
    data() {
      return {
        articles: [],
      };
    },
    mounted() {
      // Utiliser Axios pour récupérer la liste complète des articles depuis l'API Strapi
      axios.get('/articles').then((response) => {
        this.articles = response.data;
      }).catch((error) => {
        console.error('Erreur lors de la récupération de la liste des articles', error);
      });
    },
    methods: {
      formatDate(date) {
        return new Date(date).toLocaleDateString('fr-FR', { year: 'numeric', month: 'long', day: 'numeric' });
      },
    },
  };
  </script>
  
  <style scoped>
  /* Styles existants... */
  
  .article-list {
    text-align: center;
    margin: 20px;
  
    h1 {
      color: #3498db;
    }
  
    ul {
      list-style: none;
      padding: 0;
      margin: 0;
    }
  
    .article-card {
      display: flex;
      margin-bottom: 20px;
      border: 1px solid #ddd;
      border-radius: 10px;
      overflow: hidden;
  
      img {
        width: 200px;
        height: 150px;
        object-fit: cover;
      }
  
      .article-info {
        padding: 10px;
  
        h2 {
          margin: 0;
          color: #333;
        }
  
        p {
          margin: 5px 0;
          color: #555;
        }
      }
    }
  }
  </style>
  