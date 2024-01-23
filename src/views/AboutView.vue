<!-- AboutView.vue -->
<template>
  <div class="about">
    <h1>À Propos de Notre Blog</h1>

    <div v-if="aboutArticle">
      <div class="about-article">
        <img :src="aboutArticle.image.url" alt="About Article Image">
        <div class="article-info">
          <h2>{{ aboutArticle.title }}</h2>
          <p>{{ aboutArticle.body }}</p>
          <p>Date de Publication: {{ formatDate(aboutArticle.published_at) }}</p>
        </div>
      </div>
    </div>

    <div v-else>
      <p>Chargement de l'article...</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'AboutView',
  data() {
    return {
      aboutArticle: null,
    };
  },
  mounted() {
    // Utiliser Axios pour récupérer un article spécifique depuis l'API Strapi
    axios.get('/articles/1').then((response) => {
      this.aboutArticle = response.data;
    }).catch((error) => {
      console.error('Erreur lors de la récupération de l\'article à propos', error);
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

.about {
  text-align: center;
  margin: 20px;

  h1 {
    color: #3498db;
  }

  .about-article {
    display: flex;
    justify-content: space-around;
    align-items: center;
    margin-top: 20px;
    background-color: aqua;

    img {
      width: 300px;
      height: 200px;
      object-fit: cover;
    }

    .article-info {
      max-width: 600px;

      h2 {
        margin: 0;
        color: #333;
      }

      p {
        margin: 10px 0;
        color: #555;
      }
    }
  }
}
</style>
