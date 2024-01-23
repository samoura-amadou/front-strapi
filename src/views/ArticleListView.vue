<!-- HomeView.vue -->
<template>
  <div class="home">
    <div class="presentation">
      <h1>Bienvenue sur notre Blog</h1>
      <p>Découvrez des articles intéressants sur divers sujets. Explorez et profitez de la lecture!</p>
    </div>

    <div class="article-list">
      <h2>Derniers Articles</h2>
      <ul>
        <li v-for="article in articles" :key="article.id">
          <router-link :to="{ name: 'article-detail', params: { id: article.id } }">
            <div class="article-card">
              <img :src="article.image.url" alt="Article Image">
              <div class="article-info">
                <h3>{{ article.title }}</h3>
                <p>{{ article.body.substring(0, 100) }}...</p>
                <p>Catégorie: {{ article.category.name }}</p>
                <p>Date de Publication: {{ formatDate(article.published_at) }}</p>
              </div>
            </div>
          </router-link>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'HomeView',
  data() {
    return {
      articles: [],
    };
  },
  mounted() {
    // Utiliser Axios pour récupérer les derniers articles depuis l'API Strapi
    axios.get('/articles?_limit=5&_sort=published_at:DESC').then((response) => {
      this.articles = response.data;
    }).catch((error) => {
      console.error('Erreur lors de la récupération des derniers articles', error);
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
.home {
  text-align: center;
  margin: 20px;
}

.presentation {
  background-color: #4bc473;
  padding: 20px;
  border-radius: 10px;
  margin-bottom: 20px;
}

.article-list {
  h2 {
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
    border: 1px solid #880e0e;
    border-radius: 10px;
    overflow: hidden;

    img {
      width: 200px;
      height: 150px;
      object-fit: cover;
    }

    .article-info {
      padding: 10px;

      h3 {
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
